---
header-id: preparing-a-new-product-server-for-data-upgrade
---

# Preparing a New @product@ Server for Data Upgrade

[TOC levels=1-4]

As you progress toward upgrading your @product@ database, it's best to prepare a
new server for hosting @product-ver@. You'll use this server to run the
@product@ database upgrade and run @product-ver@. In this way your current
production server continues running while you configure a new server to host
@product-ver@ exactly the way you want. 

| **Note:** these steps can be done in parallel with any of the upgrade 
| preparation steps: planning for deprecated apps, testing upgrades on a 
| @product@ backup copy, or preparing to upgrade the @product@ database. 

Get the latest fixes for @product-ver@ by requesting an upgrade patch. 

## Step 1: Request an Upgrade Patch from Liferay Support (Liferay DXP only)

An *upgrade patch* contains the latest fix pack and hot fixes planned for the
next service pack. Upgrade patches provide the latest fixes available for your
data upgrade. 

## Step 2: Install @product-ver@ 

[Install @product@ on your application server](/deployment/docs/-/knowledge_base/7-2/deploying-product)
or
[use @product@ bundled with your application server of choice](/deployment/docs/-/knowledge_base/7-2/installing-product). 

| **Important:** Do not start your application server. It's not ready to start 
| until after the @product@ database upgrade. 

## Step 3: Install the Latest Upgrade Patch or Fix Pack (Liferay DXP only)

Install the upgrade patch (if you requested it from Liferay Support) or the 
[latest Fix Pack](/deployment/docs/-/knowledge_base/7-2/patching-product). 

## Step 4: Migrate Your OSGi Configurations (@product@ 7.0+)

Copy your
[OSGi configuration files](/user/-/knowledge_base/7-2/understanding-system-configuration-files)
(i.e., `.config` files) to your new server's `[Liferay Home]/osgi/configs`
folder. 

## Step 5: Migrate Your Portal Properties 

It is likely that you have overridden portal properties to customize your
installation to your requirements. If so, you must update the properties files
(e.g., `portal-setup-wizard.properties` and `portal-ext.properties` files) to
@product-ver@. For features that use OSGi Config Admin, you'll convert your
properties to OSGi configurations. As you do this, you must account for property
changes in all versions of @product@ since your current version up to and
including @product-ver@. Start with updating your portal properties. 

### Update Your Portal Properties 

If you're coming from a version prior to Liferay Portal 6.2, start with these
property-related updates:

-   If you're on Liferay Portal 6.1,
    [adapt your properties to the new defaults that Liferay Portal 6.2 introduced](/discover/deployment/-/knowledge_base/6-2/upgrading-liferay#review-the-liferay-6). 

-   If you're on Liferay 6.0.12, 
    [migrate the Image Gallery](/discover/deployment/-/knowledge_base/6-2/upgrading-liferay#migrate-your-image-gallery-images).

-   If you have a sharded environment,
    [configure your upgrade to generate a non-sharded environment](/discover/deployment/-/knowledge_base/7-0/upgrading-sharded-environment).

When a new version of @product@ is released, there are often changes to default
settings, and this release is no different. If you rely on the defaults from
your old version, you'll want to review the changes and decide whether you want
to keep the defaults from your old version or accept the defaults of the new. 

Here's a list of the 6.2 properties that have changed in 7.2: 

```properties
users.image.check.token=false
organizations.types=regular-organization,location
organizations.rootable[regular-organization]=true
organizations.children.types[regular-organization]=regular-organization,location
organizations.country.enabled[regular-organization]=false
organizations.country.required[regular-organization]=false
organizations.rootable[location]=false
organizations.country.enabled[location]=true
organizations.country.required[location]=true
layout.set.prototype.propagate.logo=true
editor.wysiwyg.portal-web.docroot.html.taglib.ui.discussion.jsp=simple
web.server.servlet.check.image.gallery=true
blogs.trackback.enabled=true
discussion.comments.format=bbcode
discussion.max.comments=0
dl.file.entry.thumbnail.max.height=128
dl.file.entry.thumbnail.max.width=128
```

This property was removed:

```properties
organizations.children.types[location]
```

The latest
[portal properties reference](@platform-ref@/7.2-latest/propertiesdoc/portal.properties.html)
provides property details and examples. Some properties are replaced by OSGi
configurations. 

### Convert Applicable Properties to OSGi Configurations 

Properties in features that have been modularized have changed and must now be
deployed separately in
[OSGi configuration files](/user/-/knowledge_base/7-2/system-settings#exporting-and-importing-configurations). 

| **Tip:** The Control Panel's *Configuration &rarr; System Settings* screens 
| are the most accurate way to create `.config` files. They let you
| [export a screen's configuration](/user/-/knowledge_base/7-2/system-settings#exporting-and-importing-configurations)
| to a `.config` file. 

## Step 6: Configure Your Documents and Media File Store 

General document store configuration (e.g., `dl.store.impl=[File Store Impl
Class]`) continues to be done using `portal-ext.properties`. But here's what's
changed for document storage:

-   Store implementation class package names changed from 
    `com.liferay.portlet.documentlibrary.store.*` in Liferay Portal 6.2 to
    `com.liferay.portal.store.*` in @product@ 7.0+. Make sure your
    `portal-ext.properties` file sets `dl.store.impl` one of these ways:

    ```properties
    dl.store.impl=com.liferay.portal.store.file.system.FileSystemStore
    dl.store.impl=com.liferay.portal.store.db.DBStore
    dl.store.impl=com.liferay.portal.store.file.system.AdvancedFileSystemStore
    dl.store.impl=com.liferay.portal.store.s3.S3Store
    ```

-   JCR Store was deprecated as of Liferay DXP Digital Enterprise 7.0 Fix Pack 
    14 and Liferay Portal CE 7.0 GA4. The
    [Document Repository Configuration](/discover/deployment/-/knowledge_base/7-1/document-repository-configuration)
    documentation describes other store options.

-   Since @product@ 7.0, document store type specific configuration (e.g., 
    specific to Simple File Store, Advanced File Store, S3, etc.) is done in the
    Control Panel at *Configuration &rarr; System Settings &rarr; File Storage*
    or done using OSGi configuration files (`.config` files). Type specific
    configuration is no longer done using `portal-ext.properties`. 

    For example, these steps to create a `.config` file that specifies a
    different root file location for a Simple File Store or Advanced File Store:
    
    1.  Create a `.config` file named after your store implementation class.
    
        Simple File Store: 
        `com.liferay.portal.store.file.system.configuration.FileSystemStoreConfiguration.config`
    
        Advanced File Store:
        `com.liferay.portal.store.file.system.configuration.AdvancedFileSystemStoreConfiguration.config`
    
    2.  Set the following `rootDir` property and replace 
        `{document_library_path}` with  your file store's path.

        ```properties
        rootDir="{document_library_path}"
        ```
    
    3.  Copy the `.config` file to your `[Liferay Home]/osgi/configs` folder.

The
[Document Repository Configuration](/deployment/docs/-/knowledge_base/7-2/document-repository-configuration)
provides more document store configuration details. 

## Step 7: Disable Indexing

Before starting the upgrade process in your new installation, you must disable
indexing to prevent upgrade process performance issues that arise when the
indexer attempts to reindex content. 

To disable indexing, create a file called
`com.liferay.portal.search.configuration.IndexStatusManagerConfiguration.config`
in your `[Liferay Home]/osgi/configs` folder and add the following content: 

```properties
indexReadOnly="true"
```

After you complete the upgrade, re-enable indexing by removing the `.config`
file or setting `indexReadOnly="false"`. 

## Checkpoint: Your new @product-ver@ server is ready for upgrading your database
