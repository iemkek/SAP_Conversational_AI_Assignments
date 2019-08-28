# Assignment 2 - Chatbot with a response from webhook

![Chatbot simple response result]()

## Webhook

#### Step 1: Create a folder
Create a new folder for your webhook application

#### Step 2: Initialize the application
Open a command prompt window and navigate to your webhook application folder. Enter command npm init and press enter. This will create a package.json file in the folder.

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

## SAP Conversational AI

#### Step 1:

# Continue to the next assignment
[Assignment 3](https://github.com/iemkek/SAP_Conversational_AI_Assignments/tree/3_Address_lookup_and_user_interaction)
