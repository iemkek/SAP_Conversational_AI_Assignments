# Innov8ion SAP Conversational AI meeting
All assignments for the Innov8ion SAP Conversational AI meeting are covered in this Github repository. Every branch is one assignment. You can navigate to the next assignment by clicking the link at the end of each assignment.

Please read through this page and then continue to the first assignment to get started.

[Assignment 1](https://github.com/iemkek/SAP_Conversational_AI_Assignments/tree/1_Chatbot_with_simple_response)

#### Prerequisites
You should have already completed the prerequisites for these assignments. If you have not done so already, you can find them [here](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/772b45ce6c46492b908d4c985add932a.html). It is a short SAP tutorial. After you follow the prerequisites-tutorial you are ready to go! 

#### Debugging the chatbot


#### Debugging the webhook
It can be pretty hard to debug a server side script. There are lengthy tutorials on how to do this ([this](https://blogs.sap.com/2019/08/02/cloudfoundryfun-7-connect-vs-code-to-deployed-cloud-applications) tutorial for example). An easy way to get some basic information from your script is to use the logging functionality of Cloud Foundry applications. Use the following code in your script to check any variables in your script on a certain point:

```javascript
console.log("<ANY_STRING_OR_VARIABLE>");
```

After the script has run, use the following command in the command prompt to show the recent logging for your own application:

```
cf logs <APPLICATION_NAME> --recent
```

