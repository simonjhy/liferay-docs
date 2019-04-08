---
header-id: liferay-upgrade-plan
---

# Liferay Upgrade Plan

[TOC levels=1-4]

Here are the steps for upgrading your @product@ database and configuration to @product-ver@. The section
[Upgrading to @product-ver@](/docs/7-2/deploy/-/knowledge_base/deploy/upgrading-to-product-ver)
demonstrate the process. The steps here outline the process and link to the
detailed articles. 

| **Important:** If you're upgrading a
| [cluster](/docs/7-2/deploy/-/knowledge_base/deploy/updating-a-cluster)
| or
| [sharded environment](/docs/7-2/deploy/-/knowledge_base/deploy/upgrading-sharded-environment),
| read their upgrade instructions first. 

| **Important:** If you're on Liferay Portal 6.0.x,
| [upgrade to Liferay Portal 6.2](/docs/6-2/deploy/-/knowledge_base/deploy/upgrading-liferay)
| before upgrading to @product-ver@. 

| **Important:** If you're running on Liferay Portal 6.1.x,
| [upgrade to @product@ 7.1](/docs/7-1/deploy/-/knowledge_base/deploy/upgrading-to-liferay-71)
| before upgrading to @product-ver@. 

| **Note:** You can
| [prepare a new @product@ server for data upgrade]
| in parallel with the steps up to and including the step to
| [upgrade the @product@ data.](/docs/7-2/deploy/-/knowledge_base/deploy/upgrading-the-product-data)

Here are the upgrade steps:

<ol type="1">
    <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/02-planning-for-deprecated-apps.markdown">Examine the deprecated applications: Remove unwanted applications from production and note ones to modify after upgrading to @product-ver@.</a></li>
    <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/03-test-upgrading-a-liferay-backup-copy.markdown#test-upgrading-a-product-backup-copy">Test upgrading a @product@ backup copy.</a></li>
    <ol type="A">
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/03-test-upgrading-a-liferay-backup-copy.markdown#copy-the-production-installation-to-a-test-server">Copy the Production Installation to a Test Server.</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/03-test-upgrading-a-liferay-backup-copy.markdown#copy-the-production-backup-to-the-test-database">Copy the Production Backup to the Test Database and Save the Logs.</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/03-test-upgrading-a-liferay-backup-copy.markdown#remove-duplicate-web-content-structure-field-names-">Remove Duplicate Web Content Structure Field Names.</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/03-test-upgrading-a-liferay-backup-copy.markdown#find-and-remove-unused-objects">Find and Remove Unused Objects.</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/03-test-upgrading-a-liferay-backup-copy.markdown#test-product-with-its-pruned-database-copy">Test @product@ with its Pruned Database Copy.</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/03-test-upgrading-a-liferay-backup-copy.markdown#install-product-ver-on-a-test-server-and-configure-it-to-use-the-pruned-database">Install @product-ver@ on a Test Server and Configure it to Use the Pruned Database.</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/03-test-upgrading-a-liferay-backup-copy.markdown#tune-your-database-for-the-upgrade">Tune Your Database for the Upgrade.</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/03-test-upgrading-a-liferay-backup-copy.markdown#upgrade-the-database">Upgrade the database to @product-ver@</a>; then return here.</a></li>
      <li>If the upgrade took too long, <a href="upgrading-the-product-data">search the upgrade log for more unused objects</a>. Then <a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/03-test-upgrading-a-liferay-backup-copy.markdown#test-upgrading-a-product-backup-copy">restart testing upgrades on with a fresh copy of the @product@ database.</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/03-test-upgrading-a-liferay-backup-copy.markdown#test-the-upgraded-portal-and-resolve-any-issues">Test the Upgraded Portal and Resolve Any Issues.</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/03-test-upgrading-a-liferay-backup-copy.markdown#checkpoint-youve-pruned-and-upgraded-a-production-database-copy">Checkpoint: You've pruned and upgraded a production database copy</a></li>
    </ol>
    <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/04-preparing-to-upgrade-the-liferay-database.markdown#preparing-to-upgrade-the-product-database">Prepare to upgrade the @product@ database.</a></li>
    <ol type="A">
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/04-preparing-to-upgrade-the-liferay-database.markdown#remove-all-unused-objects-you-identified-earlier">Remove all noted unused objects.</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/04-preparing-to-upgrade-the-liferay-database.markdown#test-product-with-its-pruned-database">Test @product@ with its pruned database.</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/04-preparing-to-upgrade-the-liferay-database.markdown#upgrade-your-marketplace-apps">Upgrade your Marketplace apps.</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/04-preparing-to-upgrade-the-liferay-database.markdown#publish-all-staged-changes-to-production">Publish all staged changes to production.</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/04-preparing-to-upgrade-the-liferay-database.markdown#synchronize-a-complete-product-backup">Synchronize a complete @product@ backup, including your pruned production database.</a></li>
      <li>Checkpoint: Ready to upgrade the production database</li>
    </ol>
    <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/04-preparing-to-upgrade-the-liferay-database.markdown#synchronize-a-complete-product-backup">Prepare a new @product@ server for data upgrade.</a></li>
    <ol type="A">
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/05-preparing-a-new-liferay-server.markdown#request-an-upgrade-patch-from-liferay-support-liferay-dxp-only">Request an upgrade patch (DXP only)</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/05-preparing-a-new-liferay-server.markdown#install-product-ver">Install @product-ver@.</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/05-preparing-a-new-liferay-server.markdown#install-product-ver">Install the latest upgrade patch of fix pack. (DXP only)</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/05-preparing-a-new-liferay-server.markdown#migrate-your-osgi-configurations-product-70">Migrate your OSGi configurations. (@product@ 7.0+)</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/05-preparing-a-new-liferay-server.markdown#migrate-your-portal-properties">Migrate your portal properties.</a></li>
      <ol type="a">
        <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/05-preparing-a-new-liferay-server.markdown#update-your-portal-properties">Update your portal properties.</a></li>
        <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/05-preparing-a-new-liferay-server.markdown#convert-applicable-properties-to-osgi-configurations">Convert applicable properties to OSGi configurations.</a></li>
      </ol>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/05-preparing-a-new-liferay-server.markdown#configure-your-documents-and-media-file-store">Configure your Documents and Media file store.</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/05-preparing-a-new-liferay-server.markdown#configure-your-documents-and-media-file-store">Disable indexing.</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/05-preparing-a-new-liferay-server.markdown#configure-your-documents-and-media-file-store">Checkpoint: Prepared the new @product@ production server</a></li>
    </ol>
    <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/06-upgrading-the-liferay-database/01-upgrading-the-liferay-database-intro.markdown#upgrading-the-product-data">Upgrade the @product@ data.</a></li>
    <ol type="A">
      <li><a href="/docs/7-2/deploy/-/knowledge_base/deploy/tuning-your-database-for-the-upgrade">Tune your database for the upgrade.</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/06-upgrading-the-liferay-database/03-configuring-the-data-upgrade.markdown#configuring-the-data-upgrade">Configure the data upgrade.</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/06-upgrading-the-liferay-database/04-upgrading-the-core-using-the-upgrade-tool.markdown#upgrading-the-core-using-the-upgrade-tool">Upgrade the core.</a></li>
      <ol type="a">
        <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/06-upgrading-the-liferay-database/04-upgrading-the-core-using-the-upgrade-tool.markdown#upgrade-tool-usage">Run the data upgrade tool.</a></li>
        <li>Resolve any core upgrade issues. If the issues were with upgrades to @product@ 7.0 or lower, <a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/04-preparing-to-upgrade-the-liferay-database.markdown#synchronize-a-complete-product-backup">restore the pruned production database backup.</a></li>
        <li>If there were issues, resolve them and <a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/06-upgrading-the-liferay-database/04-upgrading-the-core-using-the-upgrade-tool.markdown#upgrading-the-core-using-the-upgrade-tool">restart the data upgrade tool;</a> continue if there were no issues.</li>
      </ol>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/06-upgrading-the-liferay-database/05-upgrading-modules-using-gogo-shell.markdown#upgrading-modules-using-gogo-shell">Upgrade the @product@ modules, if you didn't upgrade them automatically with the core.</a></li>
      <ol type="a">
        <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/06-upgrading-the-liferay-database/05-upgrading-modules-using-gogo-shell.markdown#executing-module-upgrades">Upgrade modules that are ready for upgrade.</a></li>
        <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/06-upgrading-the-liferay-database/05-upgrading-modules-using-gogo-shell.markdown#executing-verify-processes">Verify upgraded modules.</a></li>
        <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/06-upgrading-the-liferay-database/05-upgrading-modules-using-gogo-shell.markdown#checking-upgrade-status">Resolve any module upgrade issues.</a></li>
        <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/06-upgrading-the-liferay-database/05-upgrading-modules-using-gogo-shell.markdown#executing-module-upgrades">Upgrade your resolved module issues (step <em>a</em>); continue if there were no issues.</a></li>
      </ol>
      <li>Checkpoint: Completed upgrading the @product@ data</li>
    </ol>
    <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/07-executing-post-upgrade-tasks.markdown#executing-post-upgrade-tasks">Execute post-upgrade Tasks.</a></li>
    <ol type="A">
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/07-executing-post-upgrade-tasks.markdown#tuning-your-database-for-production">Remove the database tuning made for the upgrade process.</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/07-executing-post-upgrade-tasks.markdown#re-enabling-search-indexing-and-reindexing-search-indexes">Re-enable and re-index the search indexes.</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/07-executing-post-upgrade-tasks.markdown#enabling-web-content-view-permissions">Update web content permissions. (@product@ 7.0 and lower)</a></li>
      <li><a href="https://github.com/jhinkey/liferay-docs/blob/72-upgrading-liferay/deployment/articles/05-upgrading-to-liferay-7-2/02-planning-for-deprecated-apps.markdown">Address deprecated apps.</a></li>
      <li>Checkpoint: Completed the post-upgrade tasks</li>
    </ol>
</ol>
