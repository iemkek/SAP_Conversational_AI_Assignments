# Innov8ion SAP Conversational AI meeting
All assignments for the Innov8ion SAP Conversational AI meeting are covered in this Github repository. Every branch is one assignment. You can navigate to the next assignment by clicking the link at the end of each assignment or by choosing the corresponding branch.

The scenario we will be developing is about creating a business partner on an S/4HANA system. We will start by setting up a simple chatbot that will respond to a user input and work our way to the actual business partner registration.

Please read through this page for some information regarding prerequisites and debugging possibilities. Then continue to the first assignment to get started. If you get stuck during the assignments, try to find out what is going wrong by using the tips and tricks on this page. 

[Assignment 1](https://github.com/iemkek/SAP_Conversational_AI_Assignments/tree/1_Chatbot_with_simple_response)

#### Prerequisites
You should have already completed the prerequisites for these assignments. If you have not done so already, you can find them [here](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/772b45ce6c46492b908d4c985add932a.html). If you complete the complete turorial, you are ready to go. It is a short SAP tutorial. 

#### Debugging the chatbot
There are two debug options in the SAP Conversational AI tool. You can access these options by pressing the TEST and CHAT WITH YOUR BOT buttons. These buttons are always visible on every page when you have created a bot.

![Debugging the chatbot](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/chatbotDebug.png)

To give you an insight into what the chatbot is doing when you are testing it (by givings text inputs), you can use the debug functionality of the chatbot. To activate debugging, use the CHAT WITH YOUR BOT button and press the debug button on the top of the message window.

![Debugging the chatbot](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/chatbotDebug1.png)

This screen (right side) will show you information regarding the skill that was triggered by your test input and what the response the skill was. In the above screenshot you can see that the createbp skill was triggered. The result of this skill was a message with content "Please give me your first and last name".

An additional tool that you can use is the expression analyzer. This tool accepts text strings and will show you which intent the expression is reffering to and what entities are found. It can be accessed to pressing the TEST button.

![Debugging the chatbot](https://github.com/iemkek/SAP_Conversational_AI_Assignments/blob/master/img/chatbotDebug2.png)

In the above screenshot the input test expression is "Iemke Kooijman" and the tool has determined that the corresponding intent for this expression is the createbp intent. It has also discovered that entity PERSON is in the expression.

#### Debugging the webhook
It can be pretty hard to debug a server side script. There are lengthy tutorials on how to do this ([this](https://blogs.sap.com/2019/08/02/cloudfoundryfun-7-connect-vs-code-to-deployed-cloud-applications) tutorial for example). An easy way to get some basic information from your script is to use the logging functionality of Cloud Foundry applications. Use the following code in your script to check any variables in your script on a certain point:

```javascript
console.log("<ANY_STRING_OR_VARIABLE>");
```

After the script has run, use the following command in the command prompt to show the recent logging for your own application:

```
cf logs <APPLICATION_NAME> --recent
```

# Continue to the first assignment
[Assignment 1](https://github.com/iemkek/SAP_Conversational_AI_Assignments/tree/1_Chatbot_with_simple_response)
