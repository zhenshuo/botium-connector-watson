# Botium Connector for IBM Watson Assistant

[![NPM](https://nodei.co/npm/botium-connector-watson.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/botium-connector-watson/)

[ ![Codeship Status for codeforequity-at/botium-connector-watson](https://app.codeship.com/projects/6075bd10-b02e-0136-4bcc-2eae8ef75d66/status?branch=master)](https://app.codeship.com/projects/310383)
[![npm version](https://badge.fury.io/js/botium-connector-watson.svg)](https://badge.fury.io/js/botium-connector-watson)
[![license](https://img.shields.io/github/license/mashape/apistatus.svg)]()

This is a [Botium](https://github.com/codeforequity-at/botium-core) connector for testing your IBM Watson Assistant chatbots.

__Did you read the [Botium in a Nutshell](https://medium.com/@floriantreml/botium-in-a-nutshell-part-1-overview-f8d0ceaf8fb4) articles ? Be warned, without prior knowledge of Botium you won't be able to properly use this library!__

## How it works ?
Botium uses the IBM Watson Assistant API to run conversations.

It can be used as any other Botium connector with all Botium Stack components:
* [Botium CLI](https://github.com/codeforequity-at/botium-cli/)
* [Botium Bindings](https://github.com/codeforequity-at/botium-bindings/)
* [Botium Box](https://www.botium.at)

This connector processes info about NLP. So Intent/Entity asserters can be used.

## Requirements

* __Node.js and NPM__
* a __IBM Watson Assistant__ chatbot, and user account with administrative rights
* a __project directory__ on your workstation to hold test cases and Botium configuration

## Install Botium and Watson Connector

When using __Botium CLI__:

```
> npm install -g botium-cli
> npm install -g botium-connector-watson
> botium-cli init
> botium-cli run
```

When using __Botium Bindings__:

```
> npm install -g botium-bindings
> npm install -g botium-connector-watson
> botium-bindings init mocha
> npm install && npm run mocha
```

When using __Botium Box__:

_Already integrated into Botium Box, no setup required_

## Connecting IBM Watson Assistant to Botium
You need IBM Cloud credentials (Username/Password or API Key) - see [this article](https://chatbotsmagazine.com/10-minutes-codeless-test-automation-for-ibm-watson-chatbots-d71eac9626d7) on how to get it.

Open the file _botium.json_ in your working directory and add the secret:

```
{
  "botium": {
    "Capabilities": {
      "PROJECTNAME": "<whatever>",
      "CONTAINERMODE": "watson",
      "WATSON_WORKSPACE_ID": "<watson workspace id>",
      "WATSON_APIKEY": "<ibm cloud api key>"
    }
  }
}
```

To check the configuration, run the emulator (Botium CLI required) to bring up a chat interface in your terminal window:

```
> botium-cli emulator
```

Botium setup is ready, you can begin to write your [BotiumScript](https://github.com/codeforequity-at/botium-core/wiki/Botium-Scripting) files.

## Using the botium-connector-watson-cli

This connector provides a CLI interface for importing convos and utterances from your Watson workspace and convert it to BotiumScript.

* Intents and Utterances are converted to BotiumScript utterances files (using the _watson-intents_ option)
* User Conversations are downloaded and converted to BotiumScript convos or just a plain list for analytics (using the _watson-logs_ option)

You can either run the CLI with botium-cli (it is integrated there), or directly from this connector (see samples/convoV1/package.json for some examples):

    > botium-connector-watson-cli watsonimport watson-intents
    > botium-connector-watson-cli watsonimport watson-logs --watsonformat convo
    > botium-connector-watson-cli watsonimport watson-logs --watsonformat intent

_Please note that a botium-core installation is required_

For getting help on the available CLI options and switches, run:

    > botium-connector-watson-cli watsonimport --help

## Supported Capabilities

Set the capability __CONTAINERMODE__ to __watson__ to activate this connector.

### WATSON_ASSISTANT_VERSION
_Default: V1_

Watson supports two Assistant SDK versions, V1 and V2.
* With **V1**, Botium accesses a workspace (or _Skill_) directly
* With **V2**, Botium accesses an assistant wrapping a versioned skill

### WATSON_URL
_Default: "https://gateway.watsonplatform.net/assistant/api"_

### WATSON_VERSION
_Default: "2018-09-20"_

### WATSON_APIKEY *
IAM API Key for IBM Watson - see [here](https://cloud.ibm.com/docs/services/watson/getting-started-iam.html) how to create it for your IBM Watson account. Either the IAM API Key or the Service credentials (see below) are required.

### WATSON_USER * and WATSON_PASSWORD *
Service credentials for your IBM Watson instance - see [here](https://console.bluemix.net/docs/services/watson/getting-started-credentials.html#service-credentials-for-watson-services) how to create them for your IBM Watson account.

### WATSON_WORKSPACE_ID *
The Workspace ID to use. You can find it in the IBM Watson Assistant Dashboard when clicking on "View API Details" in the popup menu of a workspace.

_This is only supported for Assistant SDK V1_

### WATSON_ASSISTANT_ID *
The Assistant ID to use. You can find it in the IBM Watson Assistant Dashboard when clicking on "View API Details" in the popup menu of an assistant.

_This is only supported for Assistant SDK V2_

### WATSON_FORCE_INTENT_RESOLUTION
_Default: true_
If this capability is enabled, then a response will be dropped if the connector does not recognizes any component like text or button in it. But the dropped message has NLP recognition info like intent and entities, which could be checked.

### WATSON_COPY_WORKSPACE
_Default: false_

This capability will copy the Watson workspace and run the Botium script on the new workspace (and delete it afterwards). Typically, when running a large amount of tests on production conversation service, the Watson workspace should not get "polluted" with test data - enabling this capability will prevent that. 

_This is only supported for Assistant SDK V1_

_Attention: as the copied workspace will run through Watson training session, it could take some time until the copied workspace is available. Botium will only continue after training is complete_
