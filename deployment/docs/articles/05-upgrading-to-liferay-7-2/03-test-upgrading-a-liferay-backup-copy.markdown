---
header-id: test-upgrading-a-product-backup-copy
---

# Test Upgrading a @product@ Backup Copy

[TOC levels=1-4]

Before upgrading your production @product@ instance, you'll want to make sure
that you can upgrade it correctly and efficiently. By removing unused objects
from @product@, you can both reduce upgrade time and improve your server's
performance on the new version. Taking the time to optimize your installation
before upgrading can save time and keep your installation running smoothly.
Doing a test run (or several test runs) on a production copy makes sense. After
going through the upgrade process, resolving any issues, and testing the
upgraded server successfully you can confidently upgrade your @product@
database. 

| **Tip:** This step and
| [preparing a new @product@ server](/deployment/docs/-/knowledge_base/7-2/preparing-a-new-product-server)
| can be done in parallel to save time. 

## Step 1: Copy the Production Installation to a Test Server

Prepare a test server to use a copy of your production installation. Your test
server will use the same Liferay version you're using on production. Configure
your server to use a new empty database for testing data upgrades. 

## Step 2: Copy the Production Backup to the Test Database

Import data from your
[production database backup](/discover/deployment/-/knowledge_base/7-1/backing-up-a-product-installation)
to the new empty database. 

| **Important:** Make sure to save the data import log---you'll examine it in
| the next steps. 

## Step 3: Remove Duplicate Web Content Structure Field Names [](id=remove-duplicate-web-content-structure-field-names)

If you've used Web Content Management extensively, you might have structures
whose field names aren't unique.  
[Find and remove any duplicate field names](/discover/deployment/-/knowledge_base/6-2/upgrading-liferay#find-and-remove-duplicate-field-names)
before upgrading. If you upgraded to Liferay Portal 6.2 previously and skipped
doing this, you'll encounter this error: 

    19:29:35,298 ERROR [main][VerifyProcessTrackerOSGiCommands:221] com.liferay.portal.verify.VerifyException: com.liferay.dynamic.data.mapping.validator.DDMFormValidationException$MustNotDuplicateFieldName: The field name page cannot be defined more than once
    com.liferay.portal.verify.VerifyException: com.liferay.dynamic.data.mapping.validator.DDMFormValidationException$MustNotDuplicateFieldName: The field name page cannot be defined more than once
 
If this is the case, roll back to your previous backup of Liferay Portal 6.2 and
[find and remove duplicate field names](/discover/deployment/-/knowledge_base/6-2/upgrading-liferay#find-and-remove-duplicate-field-names). 

## Step 4: Find and Remove Unused Objects

In the UI or using database queries, identify unused objects. Then remove them using Liferay's API, via Liferay's UI, the
[script console](/discover/portal/-/knowledge_base/7-2/running-scripts-from-the-script-console),
or a portlet you create. 

| **Important**: You should only use Liferay's API because it accounts for 
| relationships between @product@ objects. 
|
| Never use SQL directly on your database to remove records. Your SQL might miss
| object relationships, orphaning objects and causing performance problems.

Here are some common places to check for unused objects:

### Step 4a: Objects From the Large/Populated Tables

The table records reflect @product@ objects. Tables that are large or that have
many records might contain lots of unused objects. The greater the following
values are the longer upgrading takes: 

-   Records per table

-   Table size 

Finding and removing unused objects associated with such tables reduces upgrade
times. Your data import log (from the previous step) can provide valuable table
information. Database engines show this information in different ways. Your
database import log might look like this:

    Processing object type SCHEMA\_EXPORT/TABLE/TABLE\_DATA

    imported "LIFERAY"."JOURNALARTICLE" 13.33 GB 126687 rows

    imported "LIFERAY"."RESOURCEPERMISSION" 160.9 MB 1907698 rows

    imported "LIFERAY"."PORTLETPREFERENCES" 78.13 MB 432285 rows

    imported "LIFERAY"."LAYOUT" 52.05 MB 124507 rows

    imported "LIFERAY"."ASSETENTRY" 29.11 MB 198809 rows

    imported "LIFERAY"."MBMESSAGE" 24.80 MB 126185 rows

    imported "LIFERAY"."PORTALPREFERENCES" 4.091 MB 62202 rows

    imported "LIFERAY"."USER\_" 17.32 MB 62214 rows
    
    ...

Several items stand out in the example database import:

-   The `JOURNALARTICLE` table makes up 98% of the database size.
-   There are many `RESOURCEPERMISSION` records.
-   There are many `PORTLETPREFERENCES` records.

Search for unused objects associated with the tables that stand out and use
Liferay's API (e.g., the UI or
[script console](/discover/portal/-/knowledge_base/7-2/running-scripts-from-the-script-console))
to delete the objects. 

### Step 4b: Common Object Types Worth Checking 

Some object types are often worthwhile to check for unused objects. Here are some reasons for checking them:

-   Removing them frees related unused objects for removal
-   They're version objects that aren't worth keeping

Check these object types: 

-   **Sites**: Remove sites you don't need. When you remove a site,
    remove its related objects:

    -   Layouts

    -   Portlet preferences

    -   File entries (document library objects)

    -   Asset Entries

    -   Tags

    -   Vocabularies and categories

    -   Expando fields and their values

    -   `ResourcePermission` objects

    -   (and everything else)

-   **Instances**: Unused instances are rare, but since they are the highest
    object in the hierarchy, removing their objects can
    optimize upgrades considerably:

    -   Sites (and all their related content)

    -   Users

    -   Roles

    -   Organizations

    -   Global `ResourcePermission` objects

-   **Intermediate web content versions:** @product@ generates a new web
    content version after any modification (including translations).
    Consider removing versions you don't need. Removing a Journal Article,
    for example, also removes related objects such as image files
    (`JournalArticleImage`) that are part of the content. Removing unneeded
    image files frees space in your database and file system. 

-   **Document versions**: As with Journal Articles, if you don't need
    intermediate document versions, delete them. This saves space both
    in the database and on the file system, space that no longer needs
    to be upgraded. 

-   **Layouts:** Layouts are site pages, and they affect upgrade performance
    because they relate to other entities such as portlet preferences,
    permissions, assets, ratings, and more. Remove unneeded layouts. 

-   **Roles**: Remove any roles you don't need. Deleting them also deletes
    related `ResourceBlockPermission` and `ResourcePermission` objects.

-   **Users:** If you have users that aren't active anymore, remove them.

-   **Vocabularies**: Remove any unused vocabularies. Note that removing a
    vocabulary also removes its categories.

-   **Orphaned data**: Check for unused objects that are not connected to
    anything. Here are some examples:

    -   `DLFileEntries` with no file system data.

    -   `ResourcePermission` objects associated to a role, layout, user, 
        portlet instances, etc. that no longer exists.

    -   `PortletPreference` objects associated with a portlet or layout that
        no longer exists. This is common in environments with many embedded
        portlets. These portlet instances have a different lifecycle, and
        aren't deleted when the portlet is removed from a template.

## Step 5: Test @product@ with its pruned database copy

Find and resolve any issues related to the objects you removed. You can always
restart pruning a new copy of your production database if you can't resolve an
issue. 

Once you've successfully tested @product@ with its pruned database copy, you can
upgrade the database to @product-ver@. 

## Step 6: Install @product-ver@ on a test server and configure it to use the pruned database 

[Prepare a new test server with @product-ver@](/deployment/docs/-/knowledge_base/7-2/preparing-a-new-product-server). 
Configure it to use the pruned database copy. You'll use the new test server's
Liferay upgrade tool next. 

## Step 7: Upgrade the database 

Upgrade the database to @product-ver@ (see
[Upgrade the Database](/deployment/docs/-/knowledge_base/7-2/upgrade-the-database));
then return here. 

If the upgrade took too long, search the upgrade log to identify more unused
objects. Then start back at Step 1 with a fresh copy of the production database. 

## Step 8: Test the upgraded portal and resolve any issues 

Test this upgraded @product-ver@ instance and resolve any issues. If you can't resolve an issue, start back at Step 1 with a fresh copy of the production database. 

## Checkpoint: You've pruned and upgraded a production database copy 

By removing unused objects from @product@ in your test environment, you've made
upgrading feasible to do in production. You identified unused objects,
documented/scripted their removing them, and successfully upgraded the @product@
database copy. 

It's time to prepare your production environment for upgrading. 
