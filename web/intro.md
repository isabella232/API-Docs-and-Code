# Introduction 
Welcome to the DataTiger APIs! 

Our APIs are split into two main APIs:

1. **Events API:** get your data into DataTiger
2. **Management API:** the setup and configuration of all components, i.e. user journeys and segments, is handled by this API

    
Our APIs are organized around REST. All requests should be made over SSL. All request and response bodies, including errors, are encoded in JSON. We also have some specific language bindings to make integration easier. You can switch the programming language of the examples with the tabs in the top right of the API docs.

To play around with a few examples, we recommend Postman. Simply tap the button below to import our API spec.

[![Postman](https://run.pstmn.io/button.svg)](https://github.com/DataTigerGitHub/API-Docs-and-Code/blob/master/web/postman.md)


**Example event**

Below you find an example event that triggers a "Welcome email". Events can contain event properties and user properties. The user properties will be stored with the associated user. Further properties can be specified via the API.

![Events](https://raw.githubusercontent.com/DataTigerGitHub/API-Docs-and-Code/master/web/DataTigerEvents.png)

Event properties:

| Event Property | Description |
|:------------- |:-------------|
| UserId     | A unique id of the user |
| AppId      | The id of the application the workflow you want to trigger |
| EventType | The event type that triggers the workflow |



User properties:

| User Property | Description |
|:------------- |:-------------|
| UserId | Required again to make the event processing more efficient |
| AppId | Required again to make the event processing more efficient |
| Email | The email address of the user, for sending the welcome email |
| FirstName | Used to personalize the welcome email |




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
