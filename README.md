# Trellis Autofill Documentation

Contact: support@trellisconnect.com

## Basic Installation

```javascript
<script src="//cdn.trellisconnect.com/autofill/v1.0/autofill.js"></script>
<script>TrellisAutofill.configure({clientId: 'YOUR_ID_HERE'});</script>
```

## Advanced Features
`TrellistAutofill.open();`

Opens Trellis to allow the user to login or to initiate the redirect.

```javascript
<script src="//cdn.trellisconnect.com/autofill/v1.0/autofill.js"></script>
<button>Start the login process</button>
<script>
(() => {
  const button = document.querySelector('button');
  const handler =  TrellisAutofill.configure({clientId: 'YOUR_ID_HERE'});
  button.addEventListener('click', () => {
    handler.open();
  });
})();
</script>
```

`TrellisAutofill.isLoggedIn();`

Returns whether the user has successfully connected their insurance account using the login widget. Will return true once credentials have been accepted – whether or not policy data has loaded.

`TrellisAutofill.hasData();`

Returns whether the user has data available for their account.

`onLoad() callback`

Trellis Autofill calls onLoad when the app has finished loading and is ready to be interacted with. Use this callback to perform initial data checks such as `isLoggedIn` and `hasData`. 

```javascript
<script>
(() => {
  TrellisAutofill.configure({
    clientId: 'YOUR_ID_HERE',
    onLoad: () => {
      console.log(Autofill has loaded’);
      if (TrellisAutofill.isLoggedIn()) {
        // custom logic
      }
    },
  });
})();
</script>
```

## onEvent() callbacks

Trellis Autofill calls onEvent() as events occur through the user journey. This can be used for user tracking/analytics or for page behavior responding to the user. onEvent receives two arguments: the name of the event, and an object of metadata regarding the event.

```javascript
<script>
TrellisAutofill.configure({
clientId: ‘my-client-id’,
// onEvent: (name, metadata)
// name:
// OPEN - Widget opened
// CLOSE - Widget closed
// SELECT_ISSUER - User selects current insurer
//   metadata.issuer - The name of the issuer
// SUBMIT_CREDENTIALS - User submits login credentials
// AUTH_COMPLETE - Credentials accepted by Trellis
// LOADING_COMPLETE - Autofill has finished waiting for policy data.
//  - metadata.is_data_available - Boolean. Whether any data successfully loaded.
// TRANSITION_VIEW - User has viewed a screen in the experience
//  - metadata.view_name - Enum representing the screen viewed:
//   - SELECT_ISSUER - We ask the user to choose their current insurer
//   - CONSENT - We ask the user to consent to the privacy policy
//   - HAVE_LOGIN - We ask the user if they have an online account for their insurance
//   - REMEMBER_LOGIN - We ask the user if they remember the login for their insurance
//   - CREDENTIAL - We ask the user for his or her account credentials
//   - MFA - The user is asked for additional, MFA authentication
// AUTOFILL_REQUESTED - The Autofill SDK has made the request to the Autofill API to the redirect URL for this user.
// REDIRECT_INITIATED - The Autofill SDK is redirecting to the destination sent by the Autofill API.
});
</script>
```

Example:

```javascript
<script>
TrellisAutofill.configure({
clientId: ‘my-client-id’,
onEvent: (name, metadata) => {
  switch(name) {
    case ‘OPEN’:
      runOpenAnalyticsFunction();
      break;
    case ‘CLOSE’:
      runCloseAnalyticsFunction();
      break;
  }
},
});
</script>
```
