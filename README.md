# Assignment 1 - Chatbot with a simple response
In this first assignment we will be setting up the chatbot on the SAP Conversational AI webpage and make it reply to a user input.

At the end of this exercise we want to achieve the following functionality:
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

```
Intent

An intent is a box of expressions that mean the same thing but are constructed in different ways. Intents are the heart of your bot’s understanding. Each one of your intents represents an idea your bot is able to understand.
```

#### Step 5: Add expressions to intent
Drill down into the intent you just created by clicking the @createbp intent. Now add an expression to your intent by typing the expression and pressing enter. If a user types this expression when interacting with the bot, it can recognize the expression and connect it to the corresponding intent.

Enter expression: Register as business partner

![Add expressions](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/simpleResponse3.png)

```
Expression

An expression is a sentence that your bot can understand –- it’s basically something that a user might say to your bot. Expressions are organized into intents and constitute the entire knowledge of your bot. The more expressions you have, the more precisely your bot can understand its users.
``` 

#### Step 6: Create skill
Go to the Build tab and press the Create skill button. A skill can be executed after the bot has detected a certain intent from the user input. Call your skill "createbp". The skill should now be visible on the canvas.

![Create skill](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/simpleResponse4.png)

```
Skill

A skill is a block of conversation that has a clear purpose and that your bot can execute to achieve a goal. It can be as simple as the ability to greet someone, but it can also be more complex, like giving movie suggestions based on information provided by the user.
```

#### Step 7: Skill trigger
This skill should only be executed when the user refers to the createbp intent. Triggers can be added to a skill. To do this, drill down into the skill you just created by clicking the skill in the canvas. Now go to the Triggers tab and enter the following trigger:

```
If @createbp is-present
```

![Skill trigger](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/simpleResponse5.png)

#### Step 8: Define actions
Go to the Actions tab. Press ADD NEW MESSAGE GROUP and then select SEND MESSAGE. In the message input field, enter:

```
Please give me your first and last name
```

Add the following IF statement to the message group:

```
IF #job is-present
```

The #job entity is standard predefined entity. In the expression "Register as business partner", the chatbot can automatically determine that "business partner" is a #job.

![Define actions](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/simpleResponse6.png)

#### Step 9: Test it
Test your chatbot by pressing the CHAT WITH YOUR BOT button (bottom right side of the screen) and looking at the bot response.
- Type the expression "Register as business partner" 

If you do not get the correct results, try finding out what went wrong by using the tips and tricks on the master branch page [here](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/README.md#debugging-the-chatbot).

![Chatbot simple response result](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/simpleResponseTest.png)

#### Summary

A user gives an input to the chatbot. The bot can recognize this input through **expressions**, it can detect the **intent** corresponding to the expression. When an intent is detected, a **skill** can get executed and the bot can give a response to the user.

# Continue to the next assignment
[Assignment 2](https://github.com/iemkek/SAP_Conversational_AI_Assignments/tree/2_Chatbot_with_response_from_webhook)
