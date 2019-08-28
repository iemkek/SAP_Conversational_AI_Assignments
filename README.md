# Assignment 2 - Chatbot with a response from webhook

![Chatbot simple response result]()

## Webhook

#### Step 1: Create a folder
Create a new folder for your webhook application

#### Step 2: Initialize the application
Open a command prompt window and navigate to your webhook application folder. Enter command the following command and press enter:

```
npm init
```
This will create a package.json file in the folder.

#### Step 3: Extend package.json
Open the package.json file and add "engines" and "scripts" section so it looks similar to this:

```json
{
  "name": "webhookrecast",
  "version": "1.0.0",
  "description": "Webhook application",
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

#### Step 4: The manifest file
In your webhook application folder, create a file called manifest.yml and open it. Add the code you see below and change <UNIQUE_APPLICATION_NAME> with your own unique name.

```yaml
---
applications:
- name: <UNIQUE_APPLICATION_NAME>
  path: .
  memory: 128M
```

#### Step 5: The script file
In your webhook application folder, create a file called webhook.js and open it. Add the code below to the file.

```javascript
'use strict';
const express = require('express');
const bodyParser = require('body-parser');
const storage = require('node-persist');

const app = express();

app.use(bodyParser.json());

app.post('/postFullName', async (req, res) => {
	console.log(req.body.nlp.entities);
	var sFullName = req.body.nlp.entities.person[0].fullname;
	console.log(sFullName);
	var aResults = sFullName.split(" ");
	var sFirstName = aResults[0];
	var sLastName = aResults[1];
	console.log(sFirstName);
	console.log(sLastName);

	// Store fullname for later use
	await 
  .init();
	await storage.setItem('sFullName',sFullName);
	await storage.setItem('sFirstName',sFirstName);
	await storage.setItem('sLastName',sLastName);
	
	// Send back response to chatbot
	res.send({
		replies: [
					{ type: 'text',
					  content: 'Thanks ' + sFirstName + '. Now please give me your postal code and house number.' }
				 ],
				 conversation: {
					memory: {}
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

#### Step 6: Install dependencies
In the command prompt, navigate to the webhook application folder and enter the following command:

```
npm install express body-parser
````
This will download and add the dependent libraries for the application.

#### Step 7: Push the application to Cloud Foundry
In the command prompt, navigate to the webhook application folder and enter the following command: 

```
cf push
```
The application will now be deployed to the SCP Cloud Foundry environment.

#### Step 8: Check your deployed application
Look for your deployed and started application on the SCP Cloud Foundry environment. Take note of the complete application url. It should be similar to this, but with your own unique application name:

https://webhookrecast.cfapps.eu10.hana.ondemand.com

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
