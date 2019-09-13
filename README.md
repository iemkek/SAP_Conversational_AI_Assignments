# Assignment 6 - Chatbot Channels
SAP Conversational AI allows you to make your chatbot available through a lot of different channels. By choosing the correct channels you can connect with your target audience. It is possible to integrate the chatbot in one of your own web applications, but also to connect it to existing services like Facebook (chat) or Amazon Alexa (voice). 

Let’s talk to our chatbot by leveraging the Amazon Alexa channel. 

## Amazon developer account
You will need to have an Amazon developer account to be able to access the Alexa service.

#### Step 1: Register
 Go to https://developer.amazon.com/settings/console/registration/nextstep to create a developer account.

## SAP Conversational AI
Once your Amazon developer account is set up, its time to connect SAP Conversational AI to Amazon Alexa.

#### Step 1: Log in
Go to https://cai.tools.sap/ and log in to your account. Now select the bot you have created.

#### Step 2: Connect
Select the Connect tab and open the Amazon Alexa line.
![Select Channel](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/ChatbotChannels1.png)

Click “Login with Amazon” and provide your Amazon developer credentials. 
![Login to Amazon](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/ChatbotChannels2.png)

#### Step 3: Create the channel
Once the connection is established, it’s time to choose an invocation name. This is the “magic” word that will let Alexa know that it needs to switch to SAP conversational AI. Feel free to personalize these two fields. Once you’re done click “Create Channel”.
![Create Channel](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/ChatbotChannels3.png)

Leave the fields “Vendor” and “Locales” as suggested and click “Deploy skill to Amazon Alexa” 
![Deploy skill](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/ChatbotChannels4.png)

SAP CAI has now created a skill on Amazon Alexa. But before we start talking to our chatbot we will need to make some adjustments to the reply’s of the webhook.

## Webhook
Remember how the webhook uses a “card” response containing a picture and a “buttons” response with YES/NO buttons? This is something we can’t convert to speech. Instead we will just need to reply with plain text.

#### Step 1: Change the webhook
The first response that needs to be changed is the “Get address success”. Replace the following code:

```javascript
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
	 ]
```

With:

```javascript
replies: [
		{ type: 'text',
		  content: sText }
	 ],
```

The second response that needs to be changed is the “Post BP success”. Replace the following code:

```javascript
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

```
With:

```javascript
replies: [
		{ type: 'text',
		  delay: 2,
		  content: 'You are now registered as business partner ' + sBusinessPartnerNr + '.' }
	 ],

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

## Amazon Alexa
It’s time to chat! 
Go to https://developer.amazon.com/alexa/console/ask and login with your credentials. You should find the skill that has been deployed by SAP conversational AI. Click the skill name.
![Alexa Skills](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/ChatbotChannels5.png)

On the next page, open the “Test” tab. Set the “Skill testing is enabled in:” drop down to “Development”, and we’re all set to start the chat!
![Alexa developer console](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/ChatbotChannels6.png)

To start chatting with Amazon Alexa click and hold the microphone icon. “Ask **[your invocation name]** to register business partner”

![Chat!](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/ChatbotChannels7.png)
 
If Alexa response with “Please give me your first and last name” you are talking to your chatbot in SAP conversational AI! Continue on chatting and see if you can register yourself through Amazon Alexa.

# No more assignments.
Thank you for participating in this session!
