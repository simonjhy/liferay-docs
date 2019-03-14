# Prepare to Upgrade the @product@ Database 

Now that you've successfully upgraded a copy of your @product@ database, you're
ready to prepare for upgrading your production database. 

## Step 1: Remove All Unused Objects You Identified Earlier

Previously you identified and removed unused objects from a copy of your
@product@ production backup. In the same way (in the script console or UI),
remove the noted unused objects from your test database, remove them from your
production database. 

## Step 2: Test @product@ with its Pruned Database 

Find and resolve any issues related to the objects you removed. By removing the
objects from production and testing your changes before upgrading, you can more
easily troubleshoot any issues, knowing that they're not related to upgrade
processes. 

## Step 3: Upgrade Your Marketplace Apps 

Upgrade each Marketplace app (Kaleo, Calendar, Notifications, etc.) that you're
using to its latest version for your @product@ installation. Before proceeding
with the upgrade, troubleshoot any issues regarding these apps.

## Step 4: Publish all Staged Changes to Production 

If you have
[local/remote staging enabled](/user/-/knowledge_base/7-2/enabling-staging)
and have content or data saved on the staged site, 
[publish](/user/-/knowledge_base/7-2/publishing-staged-content-efficiently)
it to the live site. If you skip this step, you must run a full publish (or
manually publish changes) after the upgrade, since the system won't know what
content changed since the last publishing date.

## Step 5: Synchronize a Complete @product@ Backup 

[Completely back up your @product@ installation, pruned production database, and document repository](/discover/deployment/-/knowledge_base/7-1/backing-up-a-liferay-installation). 

It's time to prepare a new @product@ production server.  
