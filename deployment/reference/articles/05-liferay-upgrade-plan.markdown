# Liferay Upgrade Plan 

Here are the steps for upgrading Liferay and your custom plugins to @product-ver@. Note, you can upgrade Liferay (steps I and II) and your custom plugins (step III) simultaneously. 

| **Important:** Before executing the upgrade plan, read about upgrading a 
| cluster or upgrading a sharded environment, if that's your situation. 

## Upgrade Steps

<ol type="I">
  <li><a href="/deployment/deployment/-/knowledge_base/6-2/upgrading-liferay">If you're on Liferay Portal 6.1 or lower, upgrade to to Liferay Portal 6.2</a></li>
  <li><a href="https://dev.liferay.com/home">Upgrade your @product@ database and configuration</a></li>
  <ol type="A">
    <li><a href="https://dev.liferay.com/home">Prepare for upgrade</a></li>
    <ol type="1">
      <li><a href="https://dev.liferay.com/home">Examine the deprecated applications</a></li>
      <li><a href="https://dev.liferay.com/home">Prune your database</a></li>
      <ol type="a">
          <li><a href="https://dev.liferay.com/home">Use the UI to remove unused entities (sites, groups, pages, content, etc.). Note these entities.</a></li>
          <li><a href="https://dev.liferay.com/home">On another server, prune a copy of the database</a></li>
          <ol type="i">
            <li><a href="https://dev.liferay.com/home">Copy the complete production backup to a test server</a></li>
            <li><a href="https://dev.liferay.com/home">Note unused entities in tables more populated than expected.</a></li>
            <li><a href="https://dev.liferay.com/home">Use the UI or script console to remove the unused entities.</a></li>
            <li><a href="https://dev.liferay.com/home">Test the portal accordingly after these changes</a></li>
          </ol>
          <li><a href="https://dev.liferay.com/home">Use UI or script console to remove the noted unused entities from the production database</a></li>
      </ol>
      <li>Upgrade production marketplace apps</li>
      <li>Publish all staged changes to production</li>
      <li>Synchronize a complete @product@ backup</li>
    </ol>
    <li>Prepare a new @product@ production server</li>
    <ol type="1">
      <li>Install @product-ver@</li>
      <li>Install the latest fix pack (DXP only)</li>
      <li>Migrate your OSGi configurations (@product@ 7.0+)</li>
      <li>Migrate your portal properties</li>
      <ol type="a">
        <li>Update your portal properties</li>
        <li>Convert applicable properties to OSGi configurations</li>
      </ol>
      <li>Disable indexing</li>
    </ol>
    <li>Tune your database for the upgrade process, point to the future article https://issues.liferay.com/browse/LRDOCS-5880</li>
    <li>Upgrade the database to @product-ver@</li>
    <ol type="1">
      <li>Configure the database upgrade</li>
      <li>Upgrade the core</li>
      <ol type="a">
        <li>Run the data upgrade tool</li>
        <li>Resolve any core upgrade issues</li>
        <li>If the issues were with upgrades to @product@ 7.0 or lower, restore the pruned database backup</li>
        <li>Upgrade your resolved issues (step <em>a</em>); continue if there were no issues</li>
      </ol>
      <li>Upgrade the @product@ modules, if you didn't upgrade them automatically with the core</li>
      <ol type="a">
        <li>Upgrade modules that are ready for upgrade</li>
        <li>Verify upgraded modules</li>
        <li>Resolve any module upgrade issues</li>
        <li>Upgrade your resolved module issues (step <em>a</em>); continue if there were no issues</li>
      </ol>
      <li>Analyze the upgrade log in depth</li>
      <ol type="a">
        <li>If parts of the upgrade takes longer than you like, start again at step <em>1</em> to identify and remove more unused entities or change the database tunning at step <em>3</em>; otherwise, continue.</a></li>
    </ol>
    <li>Execute post-upgrade Tasks</li>
    <ol type="1">
      <li>Remove the database tunning made for the upgrade process</li>
      <li>Re-enable and re-index the search indexes</li>
      <li>Update web content permissions (@product@ 7.0 and lower)</li>
      <li>Address deprecated apps</li>
    </ol>
  </ol>
  <li>Upgrade your custom plugins to @product-ver@</li>
  <ol type="A">
      <ol type="1">
      </ol>
      <li>Hooks/Overrides/Extensions</li>
      <ol type="1">
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
      <li>Themes</li>
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
      <li>Ext</li>
  </ol>
</ol>
 
## Related Topics

[Upgrading to @product-ver@](/deployment/deployment/-/knowledge_base/7-2/upgrading-to-liferay-72)

[Upgrading a Sharded Environment](/deployment/deployment/-/knowledge_base/7-2/upgrading-sharded-environment)

[Updating a Cluster](/deployment/deployment/-/knowledge_base/7-2/updating-a-cluster)

[From Liferay 6 to 7](/develop/tutorials/-/knowledge_base/7-2/from-liferay-6-to-liferay-7)

[From @product@ 7.x to 7.2](/develop/tutorials/-/knowledge_base/7-2/from-liferay-7-1-to-7-2)

[Implementing Application Data Upgrades](/develop/tutorials/-/knowledge_base/7-2/data-upgrades)
