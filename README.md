# Assignment 3 - Address lookup and user interaction

The webhook is a simple script running on a server. In our specific case it is a Node.js script running on SCP Cloud Foundry. This script can be extended to perform any action you want. One of the possibilities is to perform calls on third party services like an address lookup. This is exactly what we will do in this assignment. After that, we want to make the user confirm his address to the bot using a "buttons" reply. This allows the user to simply click an option instead of typing his response.

At the end of this exercise we want to achieve the following functionality:
![Address lookup result](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/addressLookupTest.png)

## Webhook
To perform the address lookup, an additional function is needed in the webhook.js file. This function will call a simple third-party service that takes a postal code in the url and returns the corresponsing street and city. 

> **postcode-api.nl** -
> This is a free dutch API. It can be upsed to perform simple address lookups based on a postal code. Try it out [here](http://postcode-api.nl).

The result can then be saved using the chatbot memory functionality so it can be used at a later stage.

> **Conversation memory** -
> Each conversation with a unique user has a memory object from the beginning to the end of the conversation. This memory persists during the entire conversation; you can update it at any time or clear it whenever you want.

#### Step 1: Add the address lookup function to the webhook.js file
Open the webhook.js file in your text/code editor and add the "postAddressLookup" function so the file looks similar to this:

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

const axios = require('axios');
app.post('/postAddressLookup', (req, res) => {
	var sEntityName = 'postcode-housenumber';
	var sPostalCodeHouseNumber = req.body.nlp.entities[sEntityName][0].raw;
	var aResults = sPostalCodeHouseNumber.split(" ");
	var sPostalCode = aResults[0] + " " + aResults[1];
	var sHouseNumber = aResults[2];
	console.log(sPostalCode);
	console.log(sHouseNumber);
	var sGetUrl = "http://postcode-api.nl/adres/" + sPostalCode;
	console.log(sGetUrl);
	
	//Send HTTP Request
	axios({
	  method: 'get',
	  url: sGetUrl,
	})
	.then(function (response) {
		console.log("****************************** GET ADDRESS SUCCESS *****************************");
		
		var sStreet = response.data[0].straat;
		var sCity = response.data[0].plaats;
		var sPostalCode = response.data[0].postcode;
		
		// Store address data for later use
		var oMemory = req.body.conversation.memory;
		oMemory.address = {
			postalCode: sPostalCode,
			houseNumber: sHouseNumber,
			street: sStreet,
			city: sCity
			
		}	
		
		var sText = 'Is your address: ' + 
					sStreet + ' ' + sHouseNumber + ', ' + 
					sPostalCode + ' ' +
					sCity + '?';
		
		//Send back response to chatbot
		res.send({
			replies: [
						{ type: 'buttons',
						  content: {
							  "title": sText,
							  "buttons": [
								{
								  "title": "Yes",
								  "type": "BUTTON_TYPE",
								  "value": "YES"
								},
								{
								  "title": "No",
								  "type": "BUTTON_TYPE",
								  "value": "NO"
								}
							  ]
							}}
					 ],
					 conversation: {
						memory: oMemory
				 }
		})
	  })
	.catch(function (error) {
		console.log("****************************** GET ADDRESS ERROR *****************************");
		console.log(error);
	  });
	
});

// Set up webserver so we can receive HTTP requests from chatbot
const PORT = process.env.PORT || 8088;
var server = app.listen(PORT, function () {

    const host = server.address().address;
    const port = server.address().port;

    console.log('Webhook app listening at http://' + host + ':' + port);

});
```

#### Step 2: Install dependencies
In the command prompt, navigate to the webhook application folder and enter the following command:

```
npm install axios
````

This will download and add the dependent axios library for the application to the webhook application folder and to the package.json file.

#### Step 3: Push the application to Cloud Foundry
In the command prompt, navigate to the webhook application folder and login to the Cloud Foundry environment with your SCP credentials by entering the following command:

```
cf login
```

To deploy your application to the Cloud Foundry environment, enter the following command:

```
cf push
```

## SAP Conversational AI
An additional entity is needed in the chatbot to store the postal code and house number input from the user. Before the chatbot will recognize the pattern of the postal code and house number, we need to train it by feeding it some more expressions. In these expressions we need to specify what is our entity. Only then will the chatbot recognize the postal code and house number pattern and pass it along correctly to a new message group. In this message group a call to the webhook needs to be performed to the corresponding webhook function.

#### Step 1: Log in
Go to https://cai.tools.sap/ and log in to your account. Now select the bot you have created.

#### Step 2: Create new entity
Go to the Entities tab and press the CREATE button.

![Create new entity](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/addressLookup1.png)

#### Step 3: Enter entity details
Enter the entity name 'postcode-housenumber' and choose the 'Free entity' option. Now press the CREATE button.

![Enter entity details](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/addressLookup2.png)

#### Step 4: Add expressions for this entity
Go to the @createbp intent and add two more expressions:
- 1234 AA 1
- 9999 XY 123

![Add expressions for this entity](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/addressLookup3.png)

#### Step 5: Recognizing the entity in the expression
Select the text of the first newly added expression completely. Now select the entity 'postcode-housenumber'.

Do the same for the second expression.

![Recognizing the entity in the expression](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/addressLookup4.png)

#### Step 6: Add new message group
Go to the Build tab and select the Actions tab. Choose ADD A NEW MESSAGE GROUP and select CALL WEBHOOK.

![Call webhook](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/BusinessPartnerLookup2.png)

Enter the complete application url in the input box followed by:

```
/postAddressLookup
```

Add the following IF statement to the message group: 

```
IF #postcode-housenumber is-present.
```

![Add new message group](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/addressLookup5.png)

#### Step 7: Test it
Test your chatbot by pressing the CHAT WITH YOUR BOT button.
- Type the expression "Register as business partner"
- Now type your first and last name (make sure you use capitals)
- Now type your postal code together with your house number in the correct format (1234 AA 1)

If you do not get the correct results, try finding out what went wrong by using the tips and tricks on the master branch page [here](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/README.md#debugging-the-chatbot).

![Address lookup result](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/addressLookupTest.png)

## Continue to the next assignment
[Assignment 4](https://github.com/iemkek/SAP_Conversational_AI_Assignments/tree/4_Business_Partner_registration_on_S4HANA)
