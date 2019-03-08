---

title: Automation tests for AngularJS with protractor
author: priyanka tyagi
tags: []

---

## Protractor
### **Step 1 - Installing**
Install using the command, `npm install protractor -g`

Protractor will be installed on your machine and ready to be run using the  _`protractor <config_file_path>`_  command.
Protractor's config file, guides protractor on how to execute tests and how it should be run.
There are several parameter options to run protractor with, e.g.
`--baseUrl`: to provide URL dynamically at the run-time
`--specs`:  Comma-separated list of files to test
`--exclude`:  Comma-separated list of files to exclude
All these options can be explored using the command, `protractor --help`

You also may need to install `webdriver-manager` in order to provide the headless browser for protractor to run with, or this step can be by-passed using the parameter `directConnect`

### **Step 2 - Setting up**

There are many parameters that allow you to setup Protractor. Here are a few that are the most important in my opinion:

-   **SeleniumAddress**: This allows you provide a URL to the Selenium Server that Protractor will use to execute tests. In this case Selenium Server must be previously started to be able to run tests on Protractor.
-   **directConnect**: If set this parameter is set to true, then, this way it is not necessary to start and stop the Selenium Web-driver Server to run Protractor.
-   **Specs**: An array of test files can be sent through the specs parameter for Protractor to execute. The path of the test files must be relative to the config file.
-   **capabilities**: Parameters also can be passed to the WebDriver using the capabilities parameter. It should contain the browser against which Protractor should run the tests.
-  **multiCapabilities**: Similar to capabilities, this provides the option of running your tests on multiple browsers
-   **baseURL**: A default URL may be passed to Protractor through the baseURL parameter. That way all calls by Protractor to the browser will use that URL.
-   **framework**: This can be used to set the test framework and assertions that should be used by Protractor. There are 3 options currently for this parameter: Jasmine, Cucumber and Mocha.
-   **allScriptsTimeout**: To set up a timeout for each test executed on Protractor. This parameter is used with a number value in milliseconds.

Similar to these you can incorporate functions like **`onPrepare`**, to perform certain actions before executing the tests

All of the parameters are encapsulated in a node object which should be named "config". That way Protractor will identify those parameters inside that object. Below is an example of a Protractor configuration file named _config.js_

```
exports.config =  
{ 
	directConnect:  false,
	onPrepare:  function () {
		setTimeout(function () {
		var  x  =  0, y  =  0;
		browser.driver.manage().window().setPosition(x, y);
		browser.driver.executeScript(function () {
			return {
				width:  window.screen.availWidth,
				height:  window.screen.availHeight
			};
		}).then(function (result) {
			browser.driver.manage().window().setSize(result.width, result.height);
		});
	});
},
	specs:  [  'tests/hello_world.js'  ],  
	framework:  'jasmine2',
	params: {
	//
	},
    suites: {
    //
    },
	multiCapabilities: [
	{
		'browserName':  'chrome',
		'chromeOptions': {
		'args': ['--no-sandbox', '--disable-web-security']
	}
	// },
	// {
	// 'browserName': 'firefox'
	// },
	// {
	// 'browserName': 'safari'
	}
	], 
	baseUrl:  'http://localhost:8000', 
	allScriptsTimeout:  30000  
	};
```
In this example the folder  _tests_  will be executed. The file that contains all of the tests is _hello_world.js_ and it is inside the  _tests_  folder. These tests are going to run against the Chrome browser due the capabilities parameter that is setup with the browserName attribute as '_chrome_'. The timeout to run each test is 30 seconds.

To run the  _config_  file, simply run the command protractor passing  _config.js_  as the parameter. Protractor will run it following the instructions passed in the  _config_  file. 

### **Step 3 - Creating tests**
Protractor runs on top of Selenium and because of that it provides all of the benefits Selenium already provides. However what makes Protractor the best test automation framework for AngularJS apps is the customisation that can be done to Protractor directives.

The commands customised by Protractor, aims to catch the elements of the application’s interface through AngularJS directives. AngularJS uses some specific techniques to manipulate the Document Object Model (DOM), by inserting or extracting information from the HTML. For instance, _`ng-model`_ is used to input data, _`ng-binding`_ is used to show data in the DOM and _`ng-repeat`_ is used to show the information from the HTML of the javascript list in your AngularJS code.

To capture different elements in the UI the following commands can be used,
`ng-model`: by.model("name");
`ng-binding`: by.binding("lastName");
`ng-repeat`: by.repeater("list");
Using these commands the framework gets a WebElement that contains the `ng-model` directive with the value "_name_" and so on.





> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0MzI0MTA3MzksLTE1MDM0ODYwMjksLT
I5NTE2NTY5NiwtMTUwMzQ4NjAyOSw3MzA5OTgxMTYsNTgzNjA2
MTM3XX0=
-->