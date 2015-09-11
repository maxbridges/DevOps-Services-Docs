#Integrating Speedcurve performance testing with Bluemix DevOps Services pipelines
<!--*Max Bridges*-->
You can integrate SpeedCurve with your IBM Bluemix DevOps Services projects to include automated web application performance tests in your deployment process. SpeedCurve offers continuous performance tests of your apps' front ends to ensure responsive, satisfying experiences for your users.

<!--{{#template name="yourconcept_tool_yourtitle"}}-->

<!-- template name should match the filename, for example code_tool_track_and_plan -->

##Table of Contents
* [Prerequisites](#prereqs)
* [Steps to configure](#config)
* [Validate integration](#validate)


<a name="prereqs"></a>
##Prerequisites

You'll need the following things to integrate SpeedCurve tests into your DevOps Services Build & Deploy pipeline:

* A working pipeline with stages that build and deploy your app
  * Want to learn the basics of configuring a DevOps Services pipeline? [See this tutorial][8].
  * Need to sign up for IBM Bluemix and DevOps Services? [Register today][9].
* A SpeedCurve account
  * To integrate SpeedCurve into your pipeline, you'll need your account's SpeedCurve API key. [Register for an account if you don't already have one][3].
  * If you want to learn more about SpeedCurve, [see the official SpeedCurve learning resources][2].
  * If you want to learn how to use the SpeedCurve API, [see the official SpeedCurve API documentation][1].

<a name="config"></a>
##Steps to configure

To get up and running with SpeedCurve and DevOps Services as quickly as possible, add a dedicated SpeedCurve test job after an existing deploy job:
![A deploy and test tile in a pipeline][10]

1. On the Build & Deploy Overview page, click the Stage Configuration icon on the stage that contains the deployment job you want to test.
2. Click **JOBS**.
3. Click **ADD JOB** and select **Test**. 
4. Under Test Command, add this command: `curl "https://api.speedcurve.com/v1/deploy" -u [your-API-key-here]:x --request POST --data note="${CF_APP}-${BUILD_NUMBER}"`
  * The `--data note=="${CF_APP}-${BUILD_NUMBER}"` argument will add the app name and the test number to your SpeedCurve dashboard.
![A configured test job][11]
5. Click on the existing deploy job.
6. Under **Deploy Script**, add the command `export CF_APP_NAME="$CF_APP"`. This externalizes the app name from the deploy job.<br>
![A configured deploy job][14]
7. Click the **ENVIRONMENT PROPERTIES** tab.
8. Add a text property named `CF_APP_NAME`. This allows the exported app name from the deploy job to be used in the SpeedCurve test job.
6. Click **SAVE**. 

**Note**: You specify what you are testing on the SpeedCurve website, not in the pipeline. In this exmaple, the test job only triggers a test after a new deployment. Before you run the pipeline, be sure that you add the URL that you want to test to your SpeedCurve account.


<a name="validate"></a>
##Validate integration with DevOps Services

1. Run the pipeline.
2. When the new SpeedCurve stage completes, open your SpeedCurve dashboard. You'll see that a new set of tests have been requested.<br>
![Part of a SpeedCurve performance report][13]


<!--{{/template}}-->

[1]: https://api.speedcurve.com/
[2]: https://speedcurve.com/learn/
[3]: https://speedcurve.com/pricing/
[4]: http://gruntjs.com/
[5]: http://mochajs.org/
[6]: http://admc.io/wd/
[7]: https://www.npmjs.com/package/saucelabs
[8]: https://hub.jazz.net/tutorials/basicbuild
[9]: https://login.jazz.net/psso/proxy/jazzregister?redirect_uri=https%3A%2F%2Fhub.jazz.net%2F
[10]: images/pipeline_tile.png
[11]: images/testjob.png
[12]: images/props.png
[13]: images/speedcurve.png
[14]: images/deployjob.png
