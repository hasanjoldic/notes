# WebOTP API

WebOTP API allows to autofil OTP that the user receives via SMS.

The verification is done via a two-step process:

  1. The app client requests a one-time password (OTP), which is obtained from a specially-formatted SMS message sent by the app server.

  2. JavaScript is used to enter the OTP into a validation form on the app client and it is submitted back to the server to verify that it matches what was originally sent in the SMS.

> SMSes aren't that secure. Attackers can spoof an SMS and hijack a person's phone number. Carriers can recycle phone numbers to new users after an account is closed.

## SMS message format

```txt
Your verification code is 123456.

@www.example.com #123456
```

- The first line and second blank line are optional and are for human readability.

- The last line is mandatory. It must be the last line if there are others present, and must consist of:

  - The domain part of the URL of the website that invoked the API, preceded by a `@`. Domain value must not include a URL schema, port, or other URL features not shown above.
  - Followed by a space.
  - Followed by the OTP, preceded by a pound sign (`#`).

The availability of WebOTP can be controlled using a __Permissions Policy__ specifying a `otp-credentials` directive. This directive has a default allowlist value of `"self"`, meaning that by default, these methods can be used in top-level document contexts.

## Example

```html
<input 
  type="text"
  autocomplete="one-time-code"
  inputmode="numeric"
/>
```

```js
// Detect feature support via OTPCredential availability
if ("OTPCredential" in window) {
  window.addEventListener("DOMContentLoaded", (e) => {
    const input = document.querySelector('input[autocomplete="one-time-code"]');
    if (!input) return;
    // Set up an AbortController to use with the OTP request
    const ac = new AbortController();
    const form = input.closest("form");
    if (form) {
      // Abort the OTP request if the user attempts to submit the form manually
      form.addEventListener("submit", (e) => {
        ac.abort();
      });
    }
    // Request the OTP via get()
    navigator.credentials
      .get({
        otp: { transport: ["sms"] },
        signal: ac.signal,
      })
      .then((otp) => {
        // When the OTP is received by the app client, enter it into the form
        // input and submit the form automatically
        input.value = otp.code;
        if (form) form.submit();
      })
      .catch((err) => {
        console.error(err);
      });
  });
}
```