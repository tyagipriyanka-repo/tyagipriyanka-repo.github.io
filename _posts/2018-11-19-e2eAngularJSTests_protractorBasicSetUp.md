---

title: Automation tests for AngularJS with protractor
author: priyanka tyagi
tags: []

---

## Protractor
### **Step 1 - Installing**

Install using the command,
`npm install protractor -g`

Protractor will be installed on your machine and ready to be run using the  _`protractor <config_file_path>`_  command.
Protractor's config file, guides protractor on how to execute tests and how it should be run.
There are several parameter options to run protractor with, e.g.
`--baseUrl`: to provide URL dynamically at the run-time
`--specs`:  Comma-separated list of files to test
`--exclude`:  Comma-separated list of files to exclude
All these options can be explored using the command,
`protractor --help`

### **Step 2 - Setting up**

There are many parameters that allow you to setup Protractor. Here are a few that are the most important in my opinion:

-   **SeleniumAddress**: This allows you provide a URL to the Selenium Server that Protractor will use to execute tests. In this case Selenium Server must be previously started to be able to run tests on Protractor.
-   **SeleniumServerJar**: This allows you provide the file path of the  _SeleniumServer.jar_ file. This parameter will be used by Protractor to control the Selenium life cycle. That way it is not necessary to start and stop the Selenium Server to run Protractor.
-   **SauceUser** and **SauceKey**: When this parameter is used, Protractor will ignore the SeleniumServerJar parameter and will run tests against the Selenium Server in [SauceLabs](https://saucelabs.com/).
-   **Specs**: An array of test files can be sent through the specs parameter for Protractor to execute. The path of the test files must be relative to the config file.
-   **seleniumArgs**: This allows you to pass a parameter to Selenium if SeleniumServerJar is being used.
-   **capabilities**: Parameters also can be passed to the WebDriver using the capabilities parameter. It should contain the browser against which Protractor should run the tests.
-   **baseURL**: A default URL may be passed to Protractor through the baseURL parameter. That way all calls by Protractor to the browser will use that URL.
-   **framework**: This can be used to set the test framework and assertions that should be used by Protractor. There are 3 options currently for this parameter: Jasmine, Cucumber and Mocha.
-   **allScriptsTimeout**: To set up a timeout for each test executed on Protractor use this parameter with a number value in milliseconds.

All of the parameters are encapsulated in a node object which should be named "config". That way Protractor will identify those parameters inside that object. Below is an example of a Protractor configuration file named _config.js_

exports.config =  { seleniumServerJar:  './node_modules/protractor/selenium/selenium-server-standalone-2.39.0.jar', specs:  [  'tests/hello_world.js'  ],  
 seleniumArgs:  ['-browserTimeout=60'],  'browserName':  'chrome'  }, baseUrl:  'http://localhost:8000', allScriptsTimeout:  30000  };

In this example the parameter seleniumServerJar is used to start the Selenium Server through Protractor. The folder  _tests_  will be executed. The file that contains all of the tests is _hello_world.js_ and it is inside the  _tests_  folder. These tests are going to run against the Chrome browser due the capabilities parameter that is setup with the browserName attribute as '_chrome_'. The timeout to run each test is 30 seconds.

To run the  _config_  file, simply run the command protractor passing  _config.js_  as the parameter. Protractor will run it following the instructions passed in the  _config_  file. However, we will get the following error message, as the  _hello_world.js_  file doesn't exist yet.
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc1OTc4MDQxNCwtMTUwMzQ4NjAyOSwtMj
k1MTY1Njk2LC0xNTAzNDg2MDI5LDczMDk5ODExNiw1ODM2MDYx
MzddfQ==
-->