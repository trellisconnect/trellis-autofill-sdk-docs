# Trellis Autofill Documentation

Contact: support@trellisconnect.com

## Basic Installation

```html
<script src="//cdn.trellisconnect.com/autofill/v1.0/autofill.js"></script>
<script>
  const handler = TrellisAutofill.configure({ client_id: 'YOUR_ID_HERE' });
</script>
```

## Test Mode

You can enable test mode by adding `?t=1` to the URL of the page where you're including Autofill. This will allow you to sign in with the `Trellis Demo` issuer and use the demo account for testing.

### Disable Sticky State

If you'd like to disable sticky state without using test mode, you can add `?features=nostickystate` to the URL of the page where you're including Autofill.

## Configuration Options

| Option      | Description                                                                           |
| ----------- | ------------------------------------------------------------------------------------- |
| `client_id` | Your Trellis client ID                                                                |
| `onLoad`    | A function that is called when the TrellisAutofill loads                              |
| `onEvent`   | A function that is called when the user reaches a certain point in the Autofill flow. |

## open() function

Opens Trellis to allow the user to login or to initiate the redirect.

```html
<script src="//cdn.trellisconnect.com/autofill/v1.0/autofill.js"></script>
<button>Start the login process</button>
<script>
  (() => {
    const button = document.querySelector('button');
    const handler = TrellisAutofill.configure({ client_id: 'YOUR_ID_HERE' });
    button.addEventListener('click', () => {
      handler.open();
    });
  })();
</script>
```

## destroy() function

```js
const handler = TrellisAutofill.configure({ client_id: 'YOUR_ID_HERE' });
// when you're ready to unmount the Autofill widget...
handler.destroy();
```

## onLoad() callback

Trellis Autofill calls onLoad when the app has finished loading and is ready to be interacted with.

```javascript
TrellisAutofill.configure({
  client_id: 'YOUR_ID_HERE',
  onLoad: () => {
    console.log(Autofill has loaded’);
  },
});
```

## onEvent() callback

Trellis Autofill calls onEvent() as events occur through the user journey. This can be used for user tracking/analytics or for page behavior responding to the user. onEvent receives two arguments: the name of the event, and an object of metadata regarding the event.

```javascript
TrellisAutofill.configure({
  client_id: ‘my-client-id’,
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
```

Example:

```js
TrellisAutofill.configure({
  client_id: ‘my-client-id’,
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
```
