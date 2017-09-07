# How to get started 
This is a how to get started example

The purpose of this guide is to walk you through one of the ways to get started with DataTiger. You can either use our SDK or directly use the REST end points. Please see this guide for more details. [![Postman](https://run.pstmn.io/button.svg)](https://github.com/DataTigerGitHub/API-Docs-and-Code/blob/master/web/postman.md)


## Setup an Application

As each DataTiger domain has an application with the ID 10000 created by default, we will skip this step to make this guide quicker. Infos on how to setup an application can be found in the Management API docs (management.html). 

## Setup a Workflow / User journey

The key for each workflow is the "trigger". It determines the event which triggers a particular workflow. A trigger can for example be the registration of a new user, the passing of a level or the validation of an email address. 

### Using an API token

The code below shows an example of a demo workflow that emails the user a welcome email after a user "welcome" event type has been sent. Please replace the <ApiToken> in the HTTP headers with your own DataTiger Management API key. 

```js
var request = require("request");

var options = { method: 'POST',
  url: 'https://api.datatiger.com/applications/10000/workflows',
  headers: 
   { authorisation: 'Bearer - <ApiToken>',
     'content-type': 'application/json',
     accept: 'application/json' },
  body: 
   { name: 'Example Email Workflow',
     applicationId: 10000,
     version: 1,
     status: 'ACTIVE',
     triggerExpression: 'event.EventType == \'DemoEmailSend\'',
     steps: 
      { START: 
         { name: 'Start',
           type: 'START',
           transitions: { OK: 'SEND_EMAIL', ERROR: 'FIN' } },
        SEND_EMAIL: 
         { name: 'WELCOME_EMAIL',
           type: 'SENDGRID_EMAIL_ACTION',
           transitions: { OK: 'FIN', ERROR: 'FIN_ERR' },
           templateId: 'sendGrid ID',
           apiKey: 'sendGrid API Key',
           emailTo: '${user.Email}',
           emailToName: '${user.FirstName}',
           replyTo: 'your@email.com',
           replyToName: 'Your Name',
           from: 'your@email.com',
           fromName: 'DataTiger',
           subject: 'Welcome to DataTiger',
           trackOpen: true,
           trackClick: true,
           substitutions: [ { token: '%firstName%', value: '${user.FirstName}' } ],
           sandboxed: false },
        FIN: { name: 'FIN', type: 'TERMINAL' },
        FIN_ERR: { name: 'FIN_ERR', type: 'TERMINAL' } } },
  json: true };

request(options, function (error, response, body) {
  if (error) throw new Error(error);

  console.log(body);
});
```



### UI frontend components

(coming soon)


## Sending of events 

At this point you should have a workflow setup and we can you send the first event. It will be a straightforward user registration / welcome event. 

In the workflow in section 1.) we chose "DemoEmailSend" as the event type, which will be used to trigger the demo workflow.
 

The key components of an event are (we are only listing the required ones in this guide) below.

**Please customize them**

* UserId: a unique id of the user that is associated with this particular event. For the purpose of this guide we will use an email address.  
* AppID: the id of the application the workflow you want to trigger resides in. For the purpose of this guide we will use the default application "10000"
* EventType: the event type that triggers the workflow you want to run. In the case of this demo we use "DemoEmailSend" as the trigger. 
    "Version": "1",
    "CreatedOn": 0,
* User: here all the user properties required for this event are being passed. In the case of this guide, we will pass the following:
* UserId: required again to make the event processing more efficient
* AppId: also required again to make the event processing more efficient
* Email: the email address of the user, for sending the welcome email. Best to use your own email address. 
* FirstName: used to customize the welcome email


The code below shows an example of a welcome event. Please customize the parameters above and replace the x-api-key value with your own DataTiger Events API key. 

```js
var request = require("request");

var options = { method: 'POST',
  url: 'https://events.datatiger.com/LATEST/events',
  headers: 
   { 'x-api-key': 'Your DataTiger Events API key',
     'content-type': 'application/json',
     accept: 'application/json' },
  body: 
   { userId: 'your@email.com',
     events: 
      [ { UserId: 'your@email.com',
          AppId: '10000',
          EventType: 'DemoEmailSend',
          Version: '1',
          CreatedOn: 0,
          User: 
           { UserId: 'your@email.com',
             AppId: '10000',
             Version: '1',
             Email: 'your@email.com',
             FirstName: 'Your first name' } } ] },
  json: true };

request(options, function (error, response, body) {
  if (error) throw new Error(error);

  console.log(body);
});
```

If you have used your own email address you should have received a welcome email. Congratulations, you are now a DataTiger!

If not, please feel free to get in touch with use at help@datatiger.com so that we can help you debug. 




