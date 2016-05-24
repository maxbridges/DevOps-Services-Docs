#Extending the capabilities of your Build & Deploy pipeline

###### Last updated: 19 May 2016

You can extend the capabilities of your Build & Deploy pipeline by configuring your jobs to use supported services. For example,  test jobs can run static code scans and build jobs can globalize strings.

* [Running static code scans by using the pipeline](#scan)
* [Running dynamic app scans by using the pipeline](#appscan)
* [Globalizing strings by using the pipeline](#globalize)
* [Creating Slack notifications for builds in the pipeline](#slack)
* [Creating HipChat notifications for builds in the Delivery Pipeline](#hipchat)
* [Using Active Deploy for zero downtime deployment in the pipeline](#activedeploy)

<a name="scan"></a>
##Running static code scans by using the pipeline
Want to find security issues in your code before you deploy it? When you use the IBM&reg; Static Analyzer for Bluemix&reg; as part of your pipeline, you can run automated checks against your Java&trade; app's static `.war`, `.ear`, `.jar`, or `.class` build binary files.

A pipeline that uses the Static Analyzer service typically includes these stages:
  
* A build stage to build the source files

* A processing stage that includes these jobs:

  * A build job to run the Static Analyzer service
  
  * A build job to run a container build
  
* A deploy stage to deploy the container

<iframe width="640" height="360" src="https://www.youtube.com/embed/38DgKxT189s?feature=player_embedded" frameborder="0" allowfullscreen></iframe>

###Creating a static code scan

Before you begin, [review the Terms of Use for the service][4].

1. Create a processing stage.

  a. Click **ADD STAGE**.
  
  b. Name the stage; for example, `Processing`.
  
  c. For the input type, select **Build Artifacts**.
  
  d. For the stage and job, verify the values and update them if needed.
  
2. In the processing stage, add a build job to run the code scan.

  a. On the **JOBS** tab, click **ADD JOB**.
  
  b. For the job type, select the **Test**.
  
  c. For the tester type, select **IBM Security Static Analyzer**.
  
  d. For the organization and space, verify the values and update them if needed.
  
  e. Select or clear the **Set up service and space for me** check box as needed.
  
    * If you want the pipeline to check your Bluemix space for the service and an app that binds the service to the container, select the check box. If the service or bound app does not exist, the pipeline adds the free plan of the service to your space. The bound app that is created is named `pipeline_bridge_app`. Then, the pipeline uses the credentials from pipeline_bridge_app to access the bound services.
    
    * If you already configured the service and bound app in your Bluemix space already, or if you want to [configure these requirements manually][5], leave the check box cleared.
  
  f. In the **Minutes to wait for analysis to complete** field, type a value of 0 - 59 minutes. The default value is 5 minutes. A URL to the Static Analyzer dashboard is in the console logs at the end of the job. 
  
     If the Static Analyzer scan is not complete before the time that you specified, the job fails. However, the scan analysis continues to run and you can view it on the Static Analyzer dashboard. After the Static Analyzer scan is complete, if you rerun the job, the scan request is not resubmitted and the pipeline job can be completed. Alternatively, you can configure the pipeline not to be blocked on a successful scan result. For instructions, see the next step.
 
  g. Depending on what you want to happen if this job fails or times out, select or clear the **Stop stage execution on job failure** check box. Jobs can fail when vulnerabilities are high. 

    * If you select the check box and the job fails, later jobs in the stage and later stages do not run.
  
    * If you clear the check box and the job fails, the stage continues without blocking later jobs and stages. For example, if you know that the report includes many issues to process, you might configure the stage to continue because the scan can take a long time. In this scenario, you might not want the rest of your jobs and stages to stop only because the scan is taking too long.

  h. Click **SAVE**.

3. When the job finishes, view the results by clicking **View logs and history**. If the analysis succeeds or times out, a URL is shown in the scan results. If the scan status is pending, wait until the scan is complete to see the full results.

4. If you need to run the processing stage again before the analysis is finished, you can. In the following situations, a new analysis is not resubmitted and the previous results are used:
  * The processing stage was still running when you started a new analysis
  * A scan was already submitted for the build
  * A new source build hasn't run yet
  
5. To start a new analysis, complete one of these steps:
  * Run the build stage that inputs into the processing stage, and then run the processing stage again.
  * Open the URL for the scan results and click the **Trash** icon. Then, run the processing stage again.

###Console output examples:

#####Successful scan
![Example successful scan][7]

####Pending scan
![Example pending scan][8]

For more information about using the Static Analyzer service from the Bluemix Dashboard, [see the Static Analyzer service docs][6].


<a name="appscan"></a>
## Running dynamic app scans in the pipeline

Want to ensure security in your web applications? When you integrate IBM® AppScan Dynamic Analyzer for Bluemix® with your pipeline, you can automate security testing and identify issues in your apps before they become a problem.

A pipeline that uses the Dynamic Analyzer service typically includes these stages:

  - A **Build** stage to build the source files
  - A **Deploy** stage where you deploy your Cloud Foundry Application
  - A **Test** stage where you run the Dynamic Analyzer

### Creating a Dynamic AppScan

Before you begin:

  - You will need a running Bluemix application.
  - Review the [Terms of Use](http://www-03.ibm.com/software/sla/sladb.nsf/sla/bm-7235-01) for IBM Application Security on Cloud.

To set up the extension, configure the test stage of your pipeline.

1. Click **ADD STAGE** and name the stage **Dynamic Scan**.
2. Go to the **JOBS** tab and click **ADD JOB**. Select **Test** as the job type.
3. For **Tester Type**, select **IBM AppScan Dynamic Analyzer** from the drop down menu.
4. Select your **Target**, **Organization**, and **Space**.
5. If you do not already have the service in your space, check the **Set up service and space for me** box.
6. Specify a **Target URL** for the scan. This URL must be fully qualified.
7. (Optional) Provide the name of the **CF App** that the service and target route is bound too. This is used as a verification and if skipped, you will need to provide an alternative method to scan the URL.
8. Select a **Scan Type** from the drop down.
  - The staging scan will search for more issues and as a result, is more likely to be destructive. For a complete list of security vulnerabilities the scan searches for, [see the AppScan Dynamic Analyzer docs](https://new-console.ng.bluemix.net/docs/services/AppScanDynamicAnalyzer/index.html).
9. In the **Minutes to wait for analysis to complete** field, specify the maximum amount of time an analysis should have to complete. The value must be a whole number between 0 and 59.
  - When the analysis is running the pipeline will block other jobs. If the Dynamic Analyzer is not complete before the time you specified, the job fails. However, the scan continues to run and you can view updates on the Dynamic AppScan dashboard. After the scan is complete, if you rerun the job again and the scan request is not resubmitted and the pipeline job can be completed. Alternatively, you can configure the pipeline to continue even if the job is unsuccessful.
10. Select or clear the **Stop stage execution on job failure** check box depending on what you want to happen if this job times out or fails.
11. Click **Save**.

Viewing logs:

Click on **View logs and history** when the job finishes to view the results. If the analysis succeeds or times out, a URL is shown in the scan results. If the scan is pending, wait until the scan is complete to see the full results.


    
<a name="globalize"></a>
##Globalizing strings by using the pipeline

You can translate strings automatically into other languages when you use the IBM Globalization Pipeline service with your pipeline. IBM Globalization Pipeline uses machine translation to translate your source files as part of the pipeline's build and deployment process.

You can also update the machine-translated strings within the globalization project. A translator or native speaker of the language can then review the machine-translated strings to ensure that they are of a high quality.

To see an example of a typical pipeline that uses the Globalization Pipeline service, watch this video:

<iframe width="640" height="360" src="https://www.youtube.com/embed/UToj7FIomCg?feature=player_embedded" frameborder="0" allowfullscreen></iframe>

###Creating a globalization stage and job
Before you begin:

1. Include all English-translatable strings in one or more `filename_en.properties` or `filename_en.json` files that all use the same name. For example: `messages_en.properties`.

2. If your messages are in `.json` files, remove the depth from the structure by removing any subkeys. To remove the subkeys, change instances of `{key: {subkey: value, subkey:value}}` to `{key:value, key:value}`. 

To create the globalization stage and job:

1. Create a globalization stage.

  a. Click **ADD STAGE**.
  
  b. Name the stage; for example, `Globalization`.
  
  c. For the input type, select **SCM repository**.
  
2. In the globalization stage, add a job to translate the source files.

  a. On the **JOBS** tab, click **ADD JOB**.

  b. For the job type, select **Build**.
  
  c. For the builder type, select **IBM Globalization Pipeline**.
  
  d. For the organization and space, verify the values and update them if needed.
  
  e. In the **Source file name** field, type the name and extension of the `.properties` or `.json` input file. If you have files in different subdirectories, but they all have the same name, you need to type the file name only once. For example, if you have a `messages_en.properties` file in three directories, type `messages_en.properties` for the source file name, and all files with that name will be translated.
  
  f. Determine whether to select the **Set up service and space for me** check box.
  
    * If you want the pipeline to check your Bluemix space for the service and an app that binds the service to the container, select this check box. If the service or bound app does not exist, the pipeline adds the free plan of the service to your space for you. The bound app that is created is named `pipeline_bridge_app`. Then, the pipeline uses the credentials from pipeline_bridge_app to access the bound services.
    
    * If you configured the service and bound app in your Bluemix space already or if you want to [configure these requirements manually][9], leave this check box cleared.
    
  g. For the Globalization bundle prefix, enter a prefix for the bundle name, which is structured in this format: `<globalization_bundle_prefix>.path.to.source.file`. The pipeline job creates this Globalization bundle for you in the Globalization Pipeline service.
  
    **Tip:** Use the DevOps Services project name in the prefix so that the project can be identified easily in the Globalization Pipeline service.
  
  h. Click **SAVE**. 
  
3. Create another stage to package your app. For the input of the job in this stage, use the IBM Globalization Pipeline job from the previous stage. Do not use the source as the input. The Globalization Pipeline job augments the source files with the machine-translated strings.

4. To ensure that the machine-translated content is included in the packaged app, create another stage to package the app in. For the input to that stage, include the Globalization Pipeline job. 

The machine translated files are placed in the same directory as the source `.properties` or `.json` file. To view the files, click **Job > Artifacts**.

After the stage is completed, you can review the translated files from the console output. You can also direct translators to the files so that they can review the machine-translation output and provide revisions to improve quality. The revisions are stored in a Cloudant™ database and take precedence over any future machine translations of the same strings.

For more information about using the Globalization Pipeline service from the Bluemix Dashboard, [see the Globalization Pipeline service documentation][10].


<a name="slack"></a>
## Creating Slack notifications for builds in the pipeline

You can send notifications about IBM Container Service, IBM Security Static Analyzer, and IBM Globalization build results from your Delivery Pipeline to your Slack channels.

Before you begin, create or copy a Slack WebHook URL:

1. Open the Slack Integration page for your team: https://_project_name_.slack.com/services
2. In the list of integrations, locate **Incoming WebHooks** and click **Add**.
3. Select a channel and click **Add Incoming WebHooks Integration**.
4. Add a **WebHook URL** or copy an existing one.

For more information, [see Incoming WebHooks in the Slack documentation](https://api.slack.com/incoming-webhooks).

To create Slack notifications:

1. In the pipeline, open the configuration for a stage.
2. In the **ENVIRONMENT PROPERTIES** tab, click **ADD PROPERTY**.
3. Select **Text property**.
4. Enter the name and a value for the environment property. Repeat to create multiple environment properties.
  
  _Table 1. Environment properties for configuring Slack notifications_
  
  <table>
  <tr>
  <th>Name</th>
  <th>Value</th>
  <th>Description</th>
  <tr/>
  <tr>
    <td><code>SLACK_WEBHOOK_PATH</code></td>
    <td>A URL</td>
    <td>Required. The WebHook URL that is saved in the settings for your Slack Project.</td>
  </tr>
  <tr>
    <td><code>SLACK_COLOR</code></td>
    <td>You can enter one of the following values:
      <ul><li><code>good</code></li>
      <li><code>warning</code></li>
      <li><code>danger</code></li>
      <li>Any hexadecimal color, such as #439FEO</li></ul></td>
    <td>Optional. The color of the border that is displayed along the left side of the message in Slack. The default colors are green for good messages, red for bad messages, and gray for informational messages.</td>
  </tr>
  <tr>
    <td><code>NOTIFY_FILTER</code></td>
    <td>To receive only a subset of the message types, enter one of the following values:
      <ul>
      <li><code>good</code>: Get unknown, good, and info messages only. Bad messages are not sent.</li>
      <li><code>bad</code>: Get all messages.</li>
      <li><code>info</code>: Get info messages only. Good, bad, and unknown messages are not sent.</li>
      <li><code>unknown</code>: Get all messages.</li></ul>
      Example: If you set <code>NOTIFY_FILTER = bad</code>, error notifications are displayed on the Slack channel only.</td>
    <td>Optional. Decide which type of messages to send notifications for. By default, good, and bad messages are sent, but not informational messages.
      <ul><li><code>good</code>: Successful build results.</li>
      <li><code>bad</code>: Unsuccessful build results.</li>
      <li><code>info</code>: Informational messages about the build process.</li>
      <li><code>unknown</code>: Unknown messages are not assigned a type.</li></ul></td>
   </table>
  
5. Click **Save**.

6. Repeat these steps to send Slack notifications for other stages that include IBM Container Service, IBM Security Analyzer, and IBM Globalization jobs.

The build notification that is displayed in Slack includes a link to the DevOps Services project and sometimes to the project's dashboard. For a Slack user to open these links, the user must be registered with DevOps Services and be a member of the project that the pipeline is configured in.


<a name="hipchat"></a>
## Creating HipChat notifications for builds in the pipeline

You can send notifications about IBM Container Service, IBM Security Static Analyzer, and IBM Globalization build results from your Delivery Pipeline to your HipChat rooms.

Before you begin, create or copy and existing HipChat token:

1. Go to the HipChat Account page for your team: https://_project_name_.hipchat.com/account/api
2. Create a token or use an existing one.

To create HipChat notifications:

1. In the pipeline, open the configuration for a stage.
2. In the **ENVIRONMENT PROPERTIES** tab, click **ADD PROPERTY**.
3. Select **Text Property**.
4. Enter the name and a value for the environment property. Repeat to create multiple environment properties.
  
  _Table 2. Environment Properties for configuring HipChat notifications_
  <table>
  <tr>
  <th>Name</th>
  <th>Value</th>
  <th>Description</th>
  </tr>
  <tr>
    <td><code>HIP_CHAT_TOKEN</code></td>
    <td>Alphanumeric String</td>
    <td>Required. See the "Before you begin" section for instructions to create or copy a HipChat token.</td>
  </tr>
  <tr>
    <td><code>HIP_CHAT_ROOM_NAME</code></td>
    <td>Room name</td>
    <td>Required.</td>
  </tr>
  <tr>
    <td><code>HIP_CHAT_COLOR</code></td>
    <td>Enter one of the following values:
      <ul><li><code>yellow</code></li>
      <li><code>red</code></li>
      <li><code>green</code></li>
      <li><code>purple</code></li>
      <li><code>gray</code></li>
      <li><code>random</code></li></ul>
    </td>
    <td>Optional: Specify the background color and the left-side border color of HipChat notifications. If you set <code>HIP_CHAT_COLOR</code>, you do not need to specify when you call the following script.
     <p><code>-l notification_level</code></p> </td>
  </tr>
  <tr>
    <td><code>NOTIFICATION_COLOR</code></td>
    <td>Enter one of the following values:
      <ul><li><code>good</code></li>
      <li><code>danger</code></li>
      <li><code>info</code></li></ul>
    This variable applies to both HipChat and Slack notification colors. If you specify <code>NOTIFICATION_COLOR</code>, you do not need to specify <code>HIP_CHAT_COLOR</code> or <code>SLACK_COLOR</code>.</td>
    <td>Optional: Specify the background color and the left-side border color of both HipChat and Slack notifications. If you set <code>NOTIFICATION_COLOR</code>, you do not need to specify when you call the following script.
     <p><code>-l notification_level</code></p> </td>
  </tr>
  <tr>
    <td><code>NOTIFICATION_LEVEL</code></td>
    <td>Enter one of the following values:
      <ul><li><code>good</code></li>
      <li><code>info</code></li>
      <li><code>bad</code></li></ul></td>
    <td>Optional: Specify the notification level. See <code>NOTIFICATION_FILTER</code> for more detail on what triggers the notification.</td>
  </tr>
  <tr>
    <td><code>NOTIFICATION_FILTER</code></td>
    <td>Enter one of the following values:
      <ul><li><code>good</code></li>
      <li><code>info</code></li>
      <li><code>bad</code></li></ul>
    <td>Optional: Specify the notification filter level. Notifications are sent when the following parameters are met:
      <ul><li><code>NOTIFICATION_FILTER = good</code> and <code>NOTIFICATION_LEVEL = bad</code>, <code>good</code>, or <code>unknown</code></li>
      <li><code>NOTIFICATION_FILTER = info</code> and <code>NOTIFICATION_LEVEL = bad</code>, <code>good</code>, <code>info</code>, or <code>unknown</code></li>
      <li><code>NOTIFICATION_FILTER = bad</code> and <code>NOTIFICATION_LEVEL = bad</code> or <code>unknown</code></li>
      <li><code>NOTIFICATION_FILTER = unknown</code> and <code>NOTIFICATION_LEVEL = bad</code>, <code>good</code>, or <code>unknown</code></li></ul></td>
    </tr>
  </table>
  
5. Click **Save**.

6. Repeat these steps to send HipChat notifications for other stages that include IBM Container Service, IBM Security Static Analyzer, and IBM Globalization jobs.

<a name="activedeploy"></a>
## Using Active Deploy for zero-downtime deployment in the pipeline

You can update running apps with zero downtime when you use the IBM® Active Deploy service in your pipeline. The service provides you an update process where the new version of your app is finalized only when it proves to work properly in production. You can automate Active Deploy and enable faster continuous delivery by integrating the service into your pipeline.

The service is built into two separate jobs that you create in the same stage of your pipeline. This gives you the ability to insert test jobs that perform automated testing of the new release while roll-back is still possible. The **Active Deploy - Begin** job contains a script that starts the deployment process to increase instances of your new app until both versions of your app are live in production. **Active Deploy - Complete** ends that deployment process and decreases the original version of the app if the test phase was successful. After the Complete job finishes, it will be impossible to rollback to the previous release.

### Creating the Active Deploy stage of the pipeline
To set up Active Deploy in your pipeline, configure the jobs and environmental variables of the **Deploy** stage.

Before you begin:
- You will need a running application with an existing pipeline. For information about getting started with the pipeline, [see the Build &amp; Deploy docs](https://hub.jazz.net/docs/deploy/).

To add Jobs:

1. From your pipeline dashboard, click **ADD STAGE** and name the stage **Active Deploy**.
2. Go to the **JOBS** tab and click **ADD JOB**. Select **Deploy** as the **Job Type** and name it **Deploy Single Instance**.
  - You must edit the default command script to export *NAME*, *CF_APP_NAME*, or *CONTAINER_NAME* and deploy as a single instance with no mapped routes. *NAME* should be the equivalent to the name of the deployed app and should be unique each time the job is run. For example:
  ```
    #!/bin/bash
    NAME="${CF_APP}_${BUILD_NUMBER}"
    cf push "${NAME}" --no-route -i 1
    export NAME
  ```
3. Add a **Deploy** job and select **Active Deploy - Begin** from the **Deployer type** menu.
4. Click **ADD JOB** and select **Test**.
5. In the **Test Command** section, insert the code for any tests you want to run.
 - In order for the test to complete successfully the result must be 0. A return code of anything else causes the job to fail and a rollback occurs.
6. Uncheck **Stop running this stage if this job fails** in all test jobs to allow the **Active Deploy - Complete** job to run. If the **Active Deploy - Begin** job is successful, the **Active Deploy - Complete** job must run, or the update will remain in progress and additional updates won't be possible.
7. Click **ADD JOB** and select **Active Deploy - Complete** from the **Deployer type** menu.


To configure your environmental variables:

1. In the **ENVIRONMENT PROPERTIES** tab, click **ADD PROPERTY**.
2. Select **TEXT PROPERTY**.
3. Enter the name and value for each of the variables below. Repeat to add more variables and then click **SAVE** to complete your stage.


<table border="1" cellpadding="5" cellspacing="5">
<tr>
<th>Name</th>
<th>Required</th>
<th>Default</th>
<th>Description</th>
</tr>
<tr>
<td>NAME,  CF_APP_NAME, or CONTAINER_NAME</td>
<td>Yes</td>
<td>Leave blank, this must be set and exported in the <b>Deploy Single Instance</b> job.</td>
<td>The name of the new version of the app. Takes the form <i>AppName_BuildNumber</i>, with the build number increasing.</td>
</tr>
<tr>
<td>GROUP_SIZE</td>
<td>Yes</td>
<td>1</td>
<td>The desired number of instances of your app.</td>
</tr>
<tr>
<td>TEST_RESULT_FOR_AD</td>
<td>Yes</td>
<td>Leave blank, this must be set in the code for your <b>Test</b> jobs.</td>
<td>All of your test jobs need to be set to return a 0 to be successful.</td>
</tr>
<tr>
<td>TARGET_PLATFORM</td>
<td>Yes</td>
<td>Cloud Foundry</td>
<td>The default target platform is Cloud Foundry. To use Containers, you will need to set this variable to 'Container'.
</tr>
<tr>
<td>ROUTE_HOSTNAME</td>
<td>No</td>
<td>The name of your app.</td>
<td>As a default, the <code>check_and_set_env.sh</code> script assigns the variable <i>NAME</i>. You can override this behavior by setting it now.</td>
</tr>
<tr>
<td>ROUTE_DOMAIN</td>
<td>No</td>
<td> *.mybluemix.net</td>
<td>The domain that will be used if one isn't specified.</td>
</tr>
<tr>
<td>CONCURRENT_VERSIONS</td>
<td>No</td>
<td>2</td>
<td>The number of versions, including at least 1 successful deploy, of the app that are kept.</td>
</table>


Important:
- The first time the pipeline is run, the Active Deploy service will not be invoked. When the pipeline runs, the **Deploy Single Instance** job exports the *NAME* of the new version of the app. The **Active Deploy - Begin** job uses the *NAME* to find the *App_Name* and then searches the space for any earlier versions of the app with a route. If an original app can't be found, **Active Deploy - Begin** will scale the app to *GROUP_SIZE* instances and map the route to *ROUTE_HOSTNAME.ROUTE_DOMAIN*.


View updates:

While the pipeline is running, you can view real-time updates in several ways:

 * To see the code as it is updated, click **View logs and history**.
 * To track progress by using the activity log on your app's dashboard, click below the **LAST EXECUTION RESULT** heading.
 * To see a history of your updates, including the current deployment, go to the Active Deploy dashboard.


For more information about the Active Deploy service, see the [Bluemix docs](https://www.ng.bluemix.net/docs/services/ActiveDeploy/index.html).





[1]: https://hub.jazz.net/docs/deploy
[2]: https://www.ng.bluemix.net/docs/containers/container_single_pipeline_ov.html#container_single_pipeline_ov
[3]: https://www.ng.bluemix.net/docs/containers/container_group_pipeline_ov.html#container_group_pipeline_ov 
[4]: http://www-03.ibm.com/software/sla/sladb.nsf/sla/bm-6814-01
[5]: https://www.ng.bluemix.net/docs/containers/container_group_pipeline_ov.html#container_binding_pipeline
[6]: https://www.ng.bluemix.net/docs/services/StaticAnalyzer/index.html
[7]: ./images/analyzer_success.png
[8]: ./images/analyzer_pending.png
[9]: https://www.ng.bluemix.net/docs/containers/container_group_pipeline_ov.html#container_binding_pipeline
[10]: https://www.ng.bluemix.net/docs/services/GlobalizationPipeline/index.html
