# Liferay Upgrade Plan 

Here are the steps for upgrading Liferay and your custom plugins to @product-ver@. Note, you can upgrade Liferay (steps I and II) and your custom plugins (step III) simultaneously. 

| **Important:** Before executing the upgrade plan, read about upgrading a 
| cluster or upgrading a sharded environment, if that's your situation.
| **Notice:** <a href="/deployment/deployment/-/knowledge_base/6-2/upgrading-liferay">
| If you're on Liferay Portal 6.0.x, upgrade to Liferay Portal 6.2. If you're on Liferay 
| Portal 6.1.x, upgrade to @product@ 7.1.</a> 

## Upgrade Steps

<ol class="root">
	<li>
		Prepare for Upgrade
	</li>
	<ol>
		<li requirement="recommended">Check for Upgrade Planner Updates</li>
		<li requirement="recommended">Check Installed JDKs</li>
	</ol>
  <li>
  	<div class="title"><a href="/deployment/docs/upgrading-to-product-ver">Upgrade database and configuration</a></div>
  	<div class="description">
  		Upgrade your @product@ database and configuration.</a>Note, step <em>D. Prepare a new @product@ production server</em> can be done in parallel with steps <em>A. through C.</em>
  	</div>
  </li>
  <ol>
    <li>
    	<div class="title">
    		<a href="/deployment/docs/planning-for-deprecated-applications">Planning for deprecated applications</a>
    	</div>
    	<div class="description">
			Examine the deprecated applications: Remove unwanted applications from production and note ones to modify after upgrading to @product-ver@
    	</div>
    </li>
    <li>
    	<div class="title">
		    <a href="/deployment/docs/test-upgrading-a-product-backup-copy">Test upgrading a database copy</a>
    	</div>	
    </li>
    <ol>
      <li>Copy the production database and save the copy logs.</li>
      <li>Copy the production installation to a test server and configure it to use the database copy.</li>
      <li>Check the @product@ UI and database copy logs for unused objects (sites, users, pages, etc.)</li>
      <li>Use the script console or UI to remove the noted unused objects</li>
      <li>Test @product@ with its pruned database</li>
      <li>Install @product-ver@ on a test server and configure it to use the database copy</li>
      <li>Upgrade the database to @product-ver@ (see step <em>E. Upgrade the @product@ data</em>); then return here</li>
      <li>If the upgrade took too long, search the upgrade log to identify more unused objects. Then start back at step <em>1.</em> with a fresh copy of the production database.</li>
      <li>Test this upgraded portal and resolve any issues</li>
      <li>Checkpoint: Noted all unused objects and how to remove them</li>
    </ol>
    <li><a href="/deployment/docs/preparing-to-upgrade-the-product-database">Prepare to upgrade the production database</a></li>
    <ol>
      <li>Remove all noted unused objects</li>
      <li>Test @product@ with its pruned database</li>
      <li>Upgrade your Marketplace apps</li>
      <li>Publish all staged changes to production</li>
      <li>Synchronize a complete @product@ backup, including your pruned production database</li>
      <li>Checkpoint: Ready to upgrade the production database</li>
    </ol>
    <li><a href="/deployment/docs/preparing-a-new-product-server-for-data-upgrade">Prepare a new @product@ server for data upgrade</a></li>
    <ol>
      <li>Install @product-ver@</li>
      <li>Install the latest fix pack (DXP only)</li>
      <li>Migrate your OSGi configurations (@product@ 7.0+)</li>
      <li>Migrate your portal properties</li>
      <ol type="a">
        <li>Update your portal properties</li>
        <li>Convert applicable properties to OSGi configurations</li>
      </ol>
      <li>Configure your Documents and Media file store</li>
      <li>Disable indexing</li>
      <li>Checkpoint: Prepared the new @product@ production server</li>
    </ol>
    <li><a href="/deployment/docs/upgrading-the-product-data">Upgrade the @product@ data</a></li>
    <ol>
      <li><a href="/deployment/docs/tuning-your-database-for-the-upgrade">Tune your database for the upgrade</a></li>
      <li><a href="/deployment/docs/configuring-the-data-upgrade">Configure the data upgrade</a></li>
      <li><a href="/deployment/docs/upgrading-the-core-using-the-upgrade-tool">Upgrade the core</a></li>
      <ol>
        <li>Run the data upgrade tool</li>
        <li>Resolve any core upgrade issues</li>
        <li>If the issues were with upgrades to @product@ 7.0 or lower, restore the pruned production database backup</li>
        <li>Upgrade your resolved issues (step <em>a</em>); continue if there were no issues</li>
      </ol>
      <li><a href="/deployment/docs/upgrading-modules-using-gogo-shell">Upgrade the @product@ modules, if you didn't upgrade them automatically with the core</a></li>
      <ol>
        <li>Upgrade modules that are ready for upgrade</li>
        <li>Verify upgraded modules</li>
        <li>Resolve any module upgrade issues</li>
        <li>Upgrade your resolved module issues (step <em>a</em>); continue if there were no issues</li>
      </ol>
      <li>Checkpoint: Completed upgrading the database</li>
    </ol>
    <li><a href="/deployment/docs/executing-post-upgrade-tasks">Execute post-upgrade Tasks</a></li>
    <ol>
      <li>Remove the database tunning made for the upgrade process</li>
      <li>Re-enable and re-index the search indexes</li>
      <li>Update web content permissions (@product@ 7.0 and lower)</li>
      <li>Address deprecated apps</li>
      <li>Checkpoint: Completed post-upgrade tasks</li>
    </ol>
  </ol>
  <li>
  	<div class="title">
  		Upgrade Code
  	</div>
  </li>
  <ol>
  	<li>
  		<div class="title">Setup Liferay Workspace</div>
  	</li>
  	<ol>
  		<li icon="" req="required" commandId="#createNewWorkspaceWizard">
  			<div class="title"><a href="">Create New Liferay Workspace</a></div>
  			<div class="description">If you have an existing 7.x workspace, skip to the next step.</div>
  		</li>
  		<li icon="" req="required" commandId="#importExistingWorkspaceWizard">
  			<div class="title"><a href="">Import Existing Liferay Workspace</a></div>
  			<div class="description">If you do not have an existing 7.x workspace, go to the previous step.</div>
  		</li>
  	</ol
  	<li req="required">
  		Configure Liferay Workspace Settings
  	</li>
  	<ol>
  		<li req="required">Configure Bundle URL</li>
  		<li req="required">Configure Target Platform Version</li>
  	</ol>
  	<li req="required">
  		Initialize Server Bundle
  	</li>
  </ol>
  <li>Migrate Plugin SDK Projects</li>
  	<ol>
  		<li>Import Existing Plugins SDK Projects</li>
  		<li>Migrate Existing Plugins to Workspace</li>
  	</ol>
  <li>Upgrade Build Dependencies</li>
  <ol>
	<li>Update Repository URL</li>
	<li>Update Workspace Plugin Version</li>
	<li>Remove Dependency Versions</li>
  </ol>
  <li>Fix Upgrade Problems</li>
  	<ol>
  		<li>Auto-Correct Upgrade Problems</li>
  		<li>Find Upgrade Problems</li>
  		<li>Resolve Upgrade Problems</li>
  		<li>Remove Problem Markers</li>
  	</ol>
	<li>Upgrade Liferay Services</li>
  	<ol>
  		<li>Remove Legacy Files</li>
  		<li>Upgrade to Declarative Services</li>
  		<li>Rebuild Services</li>
  	</ol>
	<li>Upgrade Plugins to OSGi Modules</li>
	<ol>
		<li>Override/extension (module)</li>
		<li>Liferay core JSP hook (WAR)</li>
		<li>Liferay portlet JSP hook (WAR)</li>
		<li>Service wrapper hook (WAR)</li>
		<li>Language key hook (WAR)</li>
		<li>Model listener hook (WAR)</li>
		<li>Event actions hook (WAR)</li>
		<li>Servlet filter hook (WAR)</li>
		<li>Portal properties hook (WAR)</li>
		<li>Struts action hook (WAR)</li>
	</ol>
	<li>Upgrading Themes</li>
	<li>Layout Templates</li>
	<li>Framework/feature upgrades</li>
      <ol type="1">
          <li>Using JNDI data source</li>
          <li>Service Builder service invocation</li>
          <li>Service Builder</li>
          <li>Velociy templates (deprecated)</li>
      </ol>
      <li>Portlets</li>
      <ol type="1">
          <li>JSF Portlet</li>
          <li>Spring Portlet MVC</li>
          <li>Liferay MVC Portlet</li>
          <li>GenericPortlet</li>
          <li>Servlet-based Portlet</li>
          <li>Struts 1 Portlet</li>
      </ol>
      <li>Web plugins</li>
      <li>Ext plugins</li>
  </ol>
</ol>
 
## Related Topics

[Upgrading to @product-ver@](/deployment/deployment/-/knowledge_base/7-2/upgrading-to-liferay-72)

[Upgrading a Sharded Environment](/deployment/deployment/-/knowledge_base/7-2/upgrading-sharded-environment)

[Updating a Cluster](/deployment/deployment/-/knowledge_base/7-2/updating-a-cluster)

[From Liferay 6 to 7](/develop/tutorials/-/knowledge_base/7-2/from-liferay-6-to-liferay-7)

[From @product@ 7.x to 7.2](/develop/tutorials/-/knowledge_base/7-2/from-liferay-7-1-to-7-2)

[Implementing Application Data Upgrades](/develop/tutorials/-/knowledge_base/7-2/data-upgrades)
