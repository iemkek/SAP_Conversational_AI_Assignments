# Assignment 1 - Chatbot with a simple response

![Chatbot simple response result]()

## SAP Conversational AI

#### Step 1: Create bot
Create a new bot in the SAP Conversational AI environment by clicking the NEW BOT button.

#### Step 2: Fill bot details
Fill atleast the following details:
- Bot name
- Type of data (Non-personal)
- End user (Non-vulnerable)

#### Step 3: Create intent
Now that the bot is created, the first intent can also be created. An intent can be recognized by the bot when a user sends it a message and is the heart of the bot's understanding. Once an intent is recognized, the bot can perform one or more actions based this.

Name your intent: createbp

#### Step 4: Add expressions to intent
Drill down into the intent you just created. Now add an expression to your intent. If a user types this expression, the bot can recognize it and connect it to the corresponding intent.

Create expression: Register as business partner

#### Step 5: Create skill
Go to the Build tab and press the Create skill button. A skill can be executed after the bot has detected a certain intent from the user input.

Name your skill: createbp

#### Step 6: Skill trigger
Drill down into the skill you just created. Now go to the Triggers tab and create the following trigger:

If @createbp is-present

#### Step 7: Define actions
Go to the Actions tab. Press ADD NEW MESSAGE GROUP and then select SEND MESSAGE. In the message input field, enter:

Please give me your first and last name

#### Step 8: Test it
Test your chatbot by pressing the CHAT WITH YOUR BOT button. Type the expression "Register as business partner" and look at the bot response.

#### Summary

A user gives an input to the chatbot. If the bot can recognize this input through expressions, it can detect the corresponding intent. When an intent is detected, a skill gets executed and the bot can give a response to the user.

#### Questions

1.	How intelligent is the AI part in SAP Conversational AI?
2.	This was a simple response. What are you missing?
3.	Is this what you expected from building a chatbot?

# Continue to the next assignment
[Assignment 2](https://github.com/iemkek/SAP_Conversational_AI_Assignments/tree/2_Chatbot_with_response_from_webhook)
