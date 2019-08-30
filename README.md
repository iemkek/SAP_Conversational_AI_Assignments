# Assignment 2 - Chatbot with a response from webhook

![Chatbot simple response result]()

## Webhook
Make sure you have all prerequisites set up before starting with this assignment. So before starting, you should have already succesfully deployed the "hello world" app from the prerequisites tutorial to the SCP Cloud Foundry environment. The prerequisites tutorial can be found [here](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/772b45ce6c46492b908d4c985add932a.html).

#### Step 1: Create a folder
Create a new folder for your webhook application

#### Step 2: Initialize the application
Open a command prompt window and navigate to your webhook application folder. Enter the following command and press enter:

```
npm init
```

You will be prompted to answer a few questions. Enter the following:

```
package name: (webhookRecast) <ENTER>
version: (1.0.0) <ENTER>
description: <ENTER>
entry point: (index.js) webhook.js <ENTER>
test command: <ENTER>
git repository: <ENTER>
keywords: <ENTER>
author: <ENTER>
license: (ISC) <ENTER>
Is this OK? (yes) <ENTER>
```

This will create a package.json file in the folder.

#### Step 3: Extend package.json
Open the package.json file in your favourite text/code editor (like [Notepad++](https://notepad-plus-plus.org/download/v7.7.1.html)) and add the "engines" and "scripts" sections so it looks similar to this:

```json
{
  "name": "webhookrecast",
  "version": "1.0.0",
  "description": "",
  "main": "webhook.js",
  "author": "",
  "license": "ISC",
  "engines": {
    "node": "10.x.x"
  },  
  "scripts": {
    "start": "node webhook.js"
  },
}
```

#### Step 4: Install dependencies
In the command prompt, navigate to the webhook application folder and enter the following command:

```
npm install express body-parser
````

This will download and add the dependent libraries for the application to the webhook application folder and to the package.json file.

#### Step 5: The manifest file
In your webhook application folder, create a file called manifest.yml and open it in a text/code editor. Add the code you see below and change <UNIQUE_APPLICATION_NAME> with your own unique name (Hint: use your first or last name to make it unique).

```yaml
---
applications:
- name: <UNIQUE_APPLICATION_NAME>
  path: .
  memory: 128M
```

#### Step 6: The script file
In your webhook application folder, create a file called webhook.js and open it in a text/code editor. Add the code below to the file.

```javascript
'use strict';
const express = require('express');
const bodyParser = require('body-parser');

const app = express();
app.use(bodyParser.json());

app.post('/postFullName', (req, res) => {
	console.log(req.body.nlp.entities);
	var sFullName = req.body.nlp.entities.person[0].fullname;
	console.log(sFullName);
	var aResults = sFullName.split(" ");
	var sFirstName = aResults[0];
	var sLastName = aResults[1];
	console.log(sFirstName);
	console.log(sLastName);
	
	// Send back response to chatbot
	res.send({
		replies: [
					{ type: 'text',
					  content: 'Thanks ' + sFirstName + '. Now please give me your postal code and house number.' }
				 ],
				 conversation: {
					memory: {
						user: { 
							firstName: sFirstName,
							lastName: sLastName
						}
					}
				 }
	})
	
});

// Set up webserver so we can receive HTTP requests from chatbot
const PORT = process.env.PORT || 8088;
var server = app.listen(PORT, function () {

    const host = server.address().address;
    const port = server.address().port;

    console.log('Webhook app listening at http://' + host + ':' + port);

});
```

#### Step 7: Push the application to Cloud Foundry
In the command prompt, navigate to the webhook application folder and login to the Cloud Foundry environment with your SCP credentials by entering the following command:

```
cf login
```

To deploy your application to the Cloud Foundry environment, enter the following command:

```
cf push
```

#### Step 8: Check your deployed application
Look for your deployed and started application on the SCP Cloud Foundry environment. Take note of the complete application url. It should be similar to this, but with your own unique application name:

https://webhookrecast.cfapps.eu10.hana.ondemand.com

You can find this url by logging into your SCP trial account and navigating to the Cloud Foundry environment via the tile. Then go to your trail subbaccount. In the left menu click 'Spaces', and click your space, for and find your application in the list. Click it to see the details and find your application URL. Right click the url and use "Copy link address" to capture the entire URL.

You need this url in the next step.

## SAP Conversational AI

#### Step 1: Call the webhook
Drill down into the skill you created in the first assignment. Now go to the ACTIONS tab and add another message group by pressing the ADD A NEW MESSAGE GROUP button. Now select the CALL WEBHOOK option.

Enter the complete application url in the input box followed by:
```
/postFullName
```
Add the following if statement to the message group:
```
IF #person.fullname is-present
```

#### Step 2: Test it
Test your chatbot by pressing the CHAT WITH YOUR BOT button. Type the expression "Register as business partner". Now enter your first and last name (make sure you use capitals) and look at the bot response.

# Continue to the next assignment
[Assignment 3](https://github.com/iemkek/SAP_Conversational_AI_Assignments/tree/3_Address_lookup_and_user_interaction)
