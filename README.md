# Assignment 4 - Business Partner registration on S4/HANA
A more common use case for all of us would probably be in the area of communicating with an SAP system. We can easily do this by calling an OData service. In this assignment we will use the Innov8ion/iQibt S/4HANA system. Using the name and address data a call to a custom OData service will be performed. On the S/4HANA system a Business Partner will be created and the resulting Business Partner number will be returned. The number will than be communicated back to the user using a 'card' response.

At the end of this exercise we want to achieve the following functionality:
![Business Partner registration result](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/BusinessPartnerRegistrationOnS4HANATest.png)

## Webhook
To create a Business Partner on the S/4HANA system, an additional function is needed. This function will first fetch a CSRF token from the S/4HANA system, after which the actual create call will take place. We will send all the data that is collected in the chatbot memory in this call.

#### Step 1: Add the create business partner function to the webhook.js file
Open the webhook.js file in your text/code editor and add the "postCreateBusinessPartner" function so the file looks similar to this:

**Note: make sure to replace the XXXXXX username and XXXXXX password in this script by the credentials supplied to you in the session**

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

app.post('/postCreateBusinessPartner', (req, res) => {	
	// Get all variables from memory
	var sFirstName = req.body.conversation.memory.user.firstName;
	console.log(sFirstName);
	var sLastName = req.body.conversation.memory.user.lastName
	console.log(sLastName);
	var sStreet = req.body.conversation.memory.address.street;
	console.log(sStreet);
	var sHouseNumber = req.body.conversation.memory.address.houseNumber;
	console.log(sHouseNumber);
	var sPostalCode = req.body.conversation.memory.address.postalCode;
	console.log(sPostalCode);
	var sCity = req.body.conversation.memory.address.city;
	console.log(sCity);
	
	var sBaseUrl = 'http://iqibt-s4hana.sabaas.nl:8001';
	var sUrl = '/sap/opu/odata/sap/ZGW_BUPA_SRV/BusinessPartnerSet';
	axios({
	  method: 'get',
	  url: sUrl,
	  baseURL: sBaseUrl,
	  auth: {
		username: 'XXXXXXX',
		password: 'XXXXXXX'
	  },
	  headers: {'X-CSRF-Token': 'Fetch'}
	})
	.then(function (response) {
		console.log("****************************** GET CSRF SUCCESS *****************************");
		var sVarNameCSRF = 'x-csrf-token';
		var sCSRFToken = response.headers[sVarNameCSRF];
		console.log('csrf: ' + sCSRFToken);
		var sVarNameCookie = 'set-cookie';
		var sCookie = response.headers[sVarNameCookie][1];
		console.log('cookie: ' + sCookie);
		
		var sBaseUrl = 'http://iqibt-s4hana.sabaas.nl:8001';
		var sUrl = '/sap/opu/odata/sap/ZGW_BUPA_SRV/BusinessPartnerSet';
		var data = { Businesspartner : '0000000', 
					 Firstname : sFirstName,
					 Lastname: sLastName,
					 Streetname: sStreet,
					 Housenumber: sHouseNumber,
					 Postalcode: sPostalCode,
					 Cityname: sCity
				   };
		var dataString = JSON.stringify(data);
		
		axios({
		  method: 'post',
		  url: sUrl,
		  baseURL: sBaseUrl,
		  auth: {
			username: 'XXXXXXX',
			password: 'XXXXXXX'
		  },
		  headers: {
			'Content-Type': 'application/json',
			'x-csrf-token': sCSRFToken,
			'cookie': sCookie
		  },
		  data: dataString,
		})
		.then(function (response) {
			console.log("****************************** POST BP SUCCESS *****************************");
			var sBusinessPartnerNr = response.data.d.Businesspartner;
			console.log(sBusinessPartnerNr);
			
			// Send back response to chatbot
			res.send({
				replies: [
							{ type: 'text',
							  delay: 2,
							  content: 'You are now registered as a business partner.' },
							{
							  type: 'card',
							  content: {
								title: sFirstName + ' ' + sLastName + ' - ' + sBusinessPartnerNr,
								subtitle: sStreet + ' ' + sHouseNumber + ', ' + sPostalCode + ' ' + sCity,
								imageUrl: 'https://media.licdn.com/dms/image/C4E0BAQH9lwnKDWtBew/company-logo_400_400/0?e=1572480000&v=beta&t=wYK8bopvEmZQdhFnLnwq9okBQROfCqkVGA95UCFmlmA',
								buttons: []
							  }
							}	
						 ]
			})
		  })
		.catch(function (error) {
			console.log("****************************** POST BP ERROR *****************************");
			console.log(error);
		  });
		
		
	  })
	.catch(function (error) {
		console.log("****************************** GET CSRF ERROR *****************************");
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

#### Step 2: Push the application to Cloud Foundry
In the command prompt, navigate to the webhook application folder and login to the Cloud Foundry environment with your SCP credentials by entering the following command:

```
cf login
```

To deploy your application to the Cloud Foundry environment, enter the following command:

```
cf push
```

## SAP Conversational AI
SAP Conversational AI can recognize the sentiment of a message. In this assignment the chatbot should respond on the message 'yes' and it can recognize the sentiment in this to be 'positive' or 'very positive'. This will be the condition for the new message group we are going to create.

> **Sentiment** - 
> Sentiment detection is an important part of analyzing an user’s input. This allows you to treat different levels of positive, and negative inputs.

#### Step 1: Log in
Go to https://cai.tools.sap/ and log in to your account. Now select the bot you have created.

#### Step 2: Call the webhook
Drill down into the @createbp intent. Go to the Build tab and drill down into the skill you created in the first assignment. Now go to the ACTIONS tab and add another message group by pressing the ADD A NEW MESSAGE GROUP button. Now select the CALL WEBHOOK option.

![Call webhook](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/BusinessPartnerLookup2.png)

Enter the complete application url in the input box followed by:

```
/postCreateBusinessPartner
```

Add the following IF statement to the message group by pressing the ADD CONDITION button and typing:

```
IF _sentiment is positive OR _sentiment is very positive
```

The chatbot has the ability to detect the sentiment of a user input. In our case the chatbot classifies the input "Yes" as positive or very positive. We can use this standard functionality in our IF statement.

![Add new message group](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/BusinessPartnerRegistrationOnS4HANA1.png)

#### Step 3: Test it
Test your chatbot by pressing the CHAT WITH YOUR BOT button.
- Type the expression "Register as business partner"
- Now type your first and last name (make sure you use capitals)
- Now type your postal code together with your house number in the correct format (1234 AA 1)
- Now select yes

If you do not get the correct results, try finding out what went wrong by using the tips and tricks on the master branch page [here](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/README.md#debugging-the-chatbot).

![Business Partner registration result](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/BusinessPartnerRegistrationOnS4HANATest.png)

# Continue to the next assignment
[Assignment 5 - Business partner lookup on S/4HANA](https://github.com/iemkek/SAP_Conversational_AI_Assignments/tree/5_Business_Partner_lookup_on_S4HANA)
