#Integrate Sauce Labs testing with Bluemix DevOps Services

You can integrate automated functional tests that run on Sauce Labs into pipelines in your IBM&reg; Bluemix&trade; DevOps Services projects. When a Sauce Labs test suite is configured as a test job in a pipeline, the test suite can run Java or JavaScript tests against your web or mobile app as part of your continuous delivery process. These tests can provide valuable flow control for your projects, acting as gates to prevent the deployment of bad code.

In this article, a Node.js app is the test target, but the steps are generally the same for other supported languages. If you have a project that already includes Sauce Labs tests, you can configure your pipeline to integrate with Sauce Labs in just a few steps.

##Table of Contents 
<!-- LWH: Is this heading from a template? Just curious, as I know we've been using the Summary of steps in the tutorials... -->
* [Before you begin](#prereqs)
* [Verifying your Sauce Labs account information](#verify)
* [Ensuring that your project has the required components](#ensure)
* [Configuring the pipeline](#config)
* [Running and reviewing the tests](#run)

<a name="prereqs"></a>
##Before you begin

* Make sure that you have a working pipeline that has stages to build and deploy your app.
  * Want to learn the basics of configuring a DevOps Services pipeline? [See this tutorial][8].
  * Need to sign up for IBM&reg; Bluemix&trade; and DevOps Services? [Register today][9]!
* Your project must include tests to run on Sauce Labs; for example, Selenium browser tests.
* Your project must have dependencies needed to run the tests.
  * For example, a Node.js project might require these dependencies:
    * [Grunt][4], a task runner for JavaScript
    * [Mocha][5], a test framework for Node.js and web browsers
    * [WebDriver.js][6], a Node.js and browser webdriver for Selenium
    * [The Sauce Labs REST API wrapper][7]
* Register for a Sauce Labs account.
  * Sauce Labs offers 14-day free trials for new accounts. [Register for an account if you don't already have one][2].
  * If you want to learn how to use Sauce Labs, [see the official Sauce Labs documentation][1].

<a name="verify"></a>
##Verifying your Sauce Labs account information

Save a couple of pieces of information from your Sauce Labs account for use in the pipeline.

1. Locate and save your Sauce Labs user name. [It's in the welcome message at the top of your Sauce Labs account page][3].
2. Locate and save your Sauce Labs Access Key. [It's in the bottom-left corner of your Sauce Labs account page][3].

<a name="ensure"></a>
##Ensuring that your project has the required components
<!-- LWH: This heading implies that task info might be in this section, but this seems to contain mostly concept info. What do you think of changing this heading to something like "Required components"? -->

###Dependencies

Dependencies can be kept in your project's source control repository or installed when a job is run in the pipeline. In a Node.js project, installing dependencies might be as simple as running the `npm install` command in a build job. 
 
**Note**: Each job is run in its own clean environment. Dependencies that are installed in one job are not available to other jobs in its stage. However, jobs that consume the output of another job can access its dependencies. 

Task runners and test frameworks can differ between projects and languages, but for automated browser-based testing on Sauce Labs, you must include WebDriver for Selenium. In a Node.js project, your package list might include these dependencies:

```
"devDependencies": {
    "grunt-env": "latest",
    "grunt-contrib-jshint": "latest",
    "grunt-simple-mocha": "latest",
    "grunt-concurrent": "latest",
    "grunt-contrib-watch": "latest",
    "selenium-webdriver": "latest",
    "grunt": "~0.4.5",
    "saucelabs": "latest"
  }
``` 
To learn about required components for various languages, [see the official Sauce Labs documentation][1].

###Tests

Store your Sauce Labs tests in an appropriate subdirectory; typically `/test/sauce/` for Node.js. As with dependencies, the tests can be kept in your project's source control repository or be installed when a job is run in the pipeline.

One key difference between running Sauce Labs tests without the DevOps Services pipeline and with it is that you provide your Sauce Labs user name and access key in the pipeline itself. Your user name and access key are externalized as the `SAUCE_USERNAME` and `SAUCE_ACCESS_KEY` environment properties. To connect to the Sauce Lab servers, you will need to refer to those environment properties in your test scripts. 

<a name="config"></a>
##Configuring the pipeline

You can configure a Sauce Labs test job in a pipeline or in a stand-alone stage.

###Configuring a Sauce Labs test job in a pipeline

1. If you don't already have a stage that deploys a test version of your app, create one. 
2. In the stage, add a test job after the deploy job. By placing these jobs in the same stage, they can access the same set of environment properties.
![A stage that deploys and tests][11]
3. Configure the stage:
  1. On the **ENVIRONMENT PROPERTIES** tab, create two text properties: `CF_APP_NAME` and `APP_URL`. Leave `CF_APP_NAME` empty. If you like, you can set `APP_URL` to a known URL. You can also set it elsewhere in this stage or in your tests.
4. Configure the deploy job:
  1. In the **Deploy Script** field, include this command: `export CF_APP_NAME="$CF_APP"`. That command exports the app name as an environment property.
![Deploy script with added code][14]   
5. Configure the test job:
![A configured test job][10]
  a. For the tester type, select **Sauce Labs**.

  b. Enter your Bluemix target, organization, and space information.
  
  c. Enter your Sauce Labs user name and access key. Your user name and access key are externalized as the `SAUCE_USERNAME` and `SAUCE_ACCESS_KEY` environment properties, for use in your tests.
  
  d. Select the test execution command that best fits your project. For a hypothetical Node.js project, you might select **npm test** or **grunt test**. 
  
  e. Choose whether to download Selenium logs and job videos as artifacts. You can always access these on Sauce Labs.
  
  f. If you want to see your test reports in the test job logs, select the **Enable Test Report** check box.
  
  **Tip:** If you're testing a Node.js app, configure Mocha to use `mocha-jenkins-reporter` to generate correctly formatted xUnit output for the reporter. Standard JUnit formatting also works for Java.
  
6. Click **SAVE**.

For a live example of this pipeline, click the **Deploy to Bluemix** button:

[![Deploy To Bluemix](https://bluemix.net/deploy/button.png)](https://hub.jazz.net/deploy/index.html?repository=https://github.com/eLobeto/sauceLabsExample.git)
 
###Configuring a Sauce Labs test job in a stand-alone stage

3. Configure the stage. On the **ENVIRONMENT PROPERTIES** tab, be sure to create a text property: `APP_URL`. Set `APP_URL` to the known URL.
5. Configure the test job:
![A configured test job][10]

  a. For the tester type, select **Sauce Labs**.
  
  b. Enter your Bluemix target, organization, and space information.
  
  c. Enter your Sauce Labs user name and access key. Your user name and access key are externalized as the `SAUCE_USERNAME` and `SAUCE_ACCESS_KEY` environment properties, for use in your tests.
  
  d. Select the test execution command that best fits your project. For a hypothetical Node.js project, you might select **npm test** or **grunt test**. 
  
  e. Choose whether to download Selenium logs and job videos as artifacts. You can always access these on Sauce Labs.
  
  f. If you want to see your test reports in the test job logs, select the **Enable Test Report** check box.
  
  **Tip:** If you're testing a Node.js app, configure Mocha to use `mocha-jenkins-reporter` to generate correctly formatted xUnit output for the reporter. Standard JUnit formatting also works for Java.
  
6. Click **SAVE**.

For a live example of this pipeline, click the **Deploy to Bluemix** button:

[![Deploy To Bluemix](https://bluemix.net/deploy/button.png)](https://hub.jazz.net/deploy/index.html?repository=https://github.com/eLobeto/exampleSauceApp.git)

<a name="run"></a>
##Running the pipeline and reviewing the tests

Now, whenever your pipeline runs, your Sauce Labs tests run.

1. To manually trigger your pipeline, click the **Run Stage** icon.
2. When the test job is completed, click it to view its logs.
![A test log in DevOps Services][12]
3. To view your tests on the Sauce Labs website, click the links in the logs.
![A test log on Sauce labs][13]

**Note:** In a Node.js app that uses Mocha tests, each `describe(xxxx, function() { //tests here });` block is treated as a single job on Sauce Labs. If you want a single test to be considered as a single job, ensure that each `describe()` block contains only one test. <!--LWH: In the first sentence in this note, I wonder whether "on Sauce Labs" is correct. Should that be "by Sauce Labs"? -->

<!--LWH: Are there next steps? Or is this an obvious stopping point? -->


[1]: https://docs.saucelabs.com/
[2]: https://saucelabs.com
[3]: https://saucelabs.com/account
[4]: http://gruntjs.com/
[5]: http://mochajs.org/
[6]: http://admc.io/wd/
[7]: https://www.npmjs.com/package/saucelabs
[8]: https://hub.jazz.net/tutorials/basicbuild
[9]: https://login.jazz.net/psso/proxy/jazzregister?redirect_uri=https%3A%2F%2Fhub.jazz.net%2F
[10]: images/test1.png
[11]: images/deployandtest.png
[12]: images/log1.png
[13]: images/log2.png
[14]: images/deploycode.png
