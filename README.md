# Assignment 6 - Chatbot Channels

Let’s talk to our chatbot by leveraging the Amazon Alexa channel. 

At the end of this exercise we want to achieve the following functionality:
![Chatbot channels]()

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
![Select Channel]()

Click “Login with Amazon” and provide your Amazon developer credentials. 
![Login to Amazon]()

#### Step 3: Create the channel
Once the connection is established, it’s time to choose an invocation name. This is the “magic” word that will let Alexa know that it needs to switch to SAP conversational AI. Feel free to personalize these two fields. Once you’re done click “Create Channel”.
![Create Channel]()

Leave the fields “Vendor” and “Locales” as suggested and click “Deploy skill to Amazon Alexa” 
![Deploy skill]()

SAP CAI has now created a skill on Amazon Alexa. But before we start talking to our chatbot we will need to make some adjustments to the reply’s of the webhook.

## Webhook
Remember how the webhook uses a “card” response containing a picture and a “buttons” response with YES/NO buttons? This is something we can’t convert to speech. Instead we will just need to reply with plain text.

#### Step 1: Change the webhook
The first response that needs to be changed is the “Get address success”. Replace the following code:




# No more assignments.
Thank you for participating in this session!
