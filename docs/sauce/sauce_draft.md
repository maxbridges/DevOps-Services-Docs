#Integrate Sauce Labs Testing with DevOps Services

You can integrate automated functional tests run on Sauce Labs into IBM Bluemix DevOps Services project pipelines. When configured as a test job in a pipeline, a Sauce Labs test suite can run Java or JavaScript tests against your web or mobile application as part of your continuous delivery process. These tests can provide valuable flow control for your projects, acting as gates to prevent the deployment of bad code.

In this article, a Node.js app is the test target, but the steps are generally the same for other supported languages. If you have a project that already includes Sauce Labs tests, you can configure your pipeline to integrate with Sauce Labs in just a few steps.

##Table of Contents
* [Prerequisites](#prereqs)
* [Verify your Sauce Labs account information](#verify)
* [Ensure that required components are included in your project](#ensure)
* [Configure the pipeline](#config)
* [Run and review the tests](#run)

<a name="prereqs"></a>
##Prerequisites

You'll need the following things to run Sauce Labs tests against a pipeline app:

* A working pipeline with stages to build and deploy your app
  * Want to learn the basics of configuring a DevOps Services pipeline? [See this tutorial][8].
  * Need to sign up for IBM Bluemix and DevOps Services? [Register today][9]!
* A project that includes tests to run on Sauce Labs, e.g. Selenium browser tests
* Dependencies needed to run those tests
  * For example, a Node.js project might require:
    * [Grunt][4], a task runner for JavaScript
    * [Mocha][5], a test framework for Node.js and web browsers
    * [WebDriver.js][6], a Node.js and browser webdriver for Selenium
    * [The Sauce Labs REST API wrapper][7]
* A Sauce Labs account
  * Sauce Labs offers 14-day free trials for new accounts. [Register for an account if you don't already have one][2].
  * If you want to learn how to use Sauce Labs, [see the official Sauce Labs documentation][1].

<a name="verify"></a>
##Verify your Sauce Labs account information

You need to save a couple of pieces of information from your Sauce Labs account for use in the pipeline.

1. Locate and save your Sauce Labs username. [It's in the welcome message at the top of your Sauce Labs account page][3].
2. Locate and save your Sauce Labs Access Key. [It's at the bottom left corner of your Sauce Labs account page][3].

<a name="ensure"></a>
##Ensure that required components are included in your project

###Dependencies

Next, ensure that the dependencies required for testing are present in your project. Dependencies can be kept in your project's source control repository, or installed when a job is run in the pipeline. In a Node.js project, this might be as simple as running the `npm install` command in a build job.  

**Note**: Each job is run in its own clean environment. Dependencies installed in one job will not be available to other jobs in its stage. Jobs that consume the output of another job, however, will also have access to those dependencies. 

Task runners and test frameworks can differ between projects and languages, but for automated browser-based testing on Sauce Labs, you must include WebDriver for Selenium. In a Node.js project, your package list might therefore include the following dependencies:

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
To learn about required components for a variety of languages, [see the official Sauce Labs documentation][1].

###Tests

Your Sauce Labs tests should be located in the subdirectory`/tests/sauce/`. As with dependencies, these can be kept in your project's source control repository, or be installed when a job is run in the pipeline.

One key difference between running Sauce Labs tests without the DevOps Services pipeline and with it is that you provide your Sauce Labs username and access key in the pipeline itself. These are externalized as the environment properties `SAUCE_USERNAME` and `SAUCE_ACCESS_KEY`, respectively. You can refer to them in your test scripts. 

<a name="config"></a>
##Configure the pipeline

Configure a Sauce Labs test job in a pipeline:

1. If you don't already have a stage that deploys a test version of your app, create one. 
2. Add a test job after the deploy job in that stage. By placing these jobs in the same stage, they'll have access to the same set of environment properties.
![A stage that deploys and tests][11]
3. Configure the stage:
  1. In the **ENVIRONMENT PROPERTIES** tab, create two text properties: `CF_APP_NAME` and `TEST_URL`. Leave `CF_APP_NAME` empty. You can set `TEST_URL` to a known URL if you like. You can also set it elsewhere in this stage or in your tests.
4. Configure the deploy job:
  1. In the **Deploy Script** field, include the command `export CF_APP_NAME="$CF_APP"`. This will export the app name as an environment property. 
5. Configure the test job:
![A configured test job][10]
  1. Select **Sauce Labs** as the Tester Type.
  2. Enter your Bluemix **Target**, **Organization**, and **Space** information.
  3. Enter your Sauce Labs **Username** and **Access Key**. These are externalized as the environment properties `SAUCE_USERNAME` and `SAUCE_ACCESS_KEY`, respectively, for use in your tests.
  4. Select the **Test Execution Command** that best fits your project. For a hypothetical Node.js project, that would probably be **npm install**. 
  5. Choose whether or not to **Download Selenium logs and job videos** as artifacts. You can always access these on Sauce Labs, as well.
  6. Check the **Enable Test Report** box if you want to see your test reports in the test job logs.
    * If you're testing a Node.js app, configure Mocha to use `mocha-jenkins-reporter` to generate correctly formatted xUnit output for the reporter. Standard JUnit     formatting will also work for Java.
6. Click **SAVE**. 

<a name="run"></a>
##Run the pipeline and review the tests

Now, whenever your pipeline runs, your Sauce Labs tests will run, too.

1. Click the **Run Stage** icon to manually trigger your pipeline.
2. When the test job completes, click on it to view its logs.
![A test log in DevOps Services][12]
3. Alternatively, view your tests on Sauce Labs' site, too.
![A test log on Sauce labs][13]

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