# Assignment 1 - Chatbot with a simple response

![Chatbot simple response result](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/simpleResponseTest.png)

## SAP Conversational AI

#### Step 1: Log in
Go to https://cai.tools.sap/ and create an account or log in to your account.

#### Step 2: Create bot
Create a new bot in the SAP Conversational AI environment by clicking the NEW BOT button.

![Create bot](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/simpleResponse1.png)

#### Step 3: Enter bot details
Fill atleast the following details:
- Bot name (createbp)
- Type of data (Non-personal)
- End user (Non-vulnerable)

#### Step 4: Create intent
Now that the bot is created, the first intent can also be created. An intent can be recognized by the bot when a user sends it a message and is the heart of the bot's understanding. Once an intent is recognized, the bot can perform one or more actions based this.

Name your intent: createbp

![Create intent](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/simpleResponse2.png)

#### Step 5: Add expressions to intent
Drill down into the intent you just created by clicking the @createbp intent. Now add an expression to your intent by typing the expression and pressing enter. If a user types this expression when interacting with the bot, it can recognize the expression and connect it to the corresponding intent.

Enter expression: Register as business partner

![Add expressions](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/simpleResponse3.png)

#### Step 6: Create skill
Go to the Build tab and press the Create skill button. A skill can be executed after the bot has detected a certain intent from the user input. Call your skill "createbp". The skill should now be visible on the canvas.

![Create skill](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/simpleResponse4.png)

#### Step 7: Skill trigger
Drill down into the skill you just created by clicking the skill in the canvas. Now go to the Triggers tab and enter the following trigger:

```
If @createbp is-present
```

![Skill trigger](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/simpleResponse5.png)

#### Step 8: Define actions
Go to the Actions tab. Press ADD NEW MESSAGE GROUP and then select SEND MESSAGE. In the message input field, enter:

```
Please give me your first and last name
```

![Define actions](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/simpleResponse6.png)

#### Step 9: Test it
Test your chatbot by pressing the CHAT WITH YOUR BOT button (bottom right corner of the screen). Type the expression "Register as business partner" and look at the bot response.

![Chatbot simple response result](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/simpleResponseTest.png)

#### Summary

A user gives an input to the chatbot. The bot can recognize this input through **expressions**, it can detect the **intent** corresponding to the expression. When an intent is detected, a **skill** can get executed and the bot can give a response to the user.

# Continue to the next assignment
[Assignment 2](https://github.com/iemkek/SAP_Conversational_AI_Assignments/tree/2_Chatbot_with_response_from_webhook)
