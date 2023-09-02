# Notifications API

> Note: This feature is available in Web Workers

> Secure context: This feature is available only in secure contexts (HTTPS), in some or all supporting browsers.

The Notifications API allows web pages to control the display of system notifications to the end user. These are outside the top-level browsing context viewport, so therefore __can be displayed even when the user has switched tabs or moved to a different app__.

## Initializing

First, the user needs to grant the current origin permission to display system notifications, which is generally done when the app or site initializes, using the `Notification.requestPermission()` method. This should be done in response to a user gesture, such as clicking a button, for example:

```js
function askNotificationPermission() {
  // function to actually ask the permissions
  function handlePermission(permission) {
    // set the button to shown or hidden, depending on what the user answers
    notificationBtn.style.display =
      Notification.permission === "granted" ? "none" : "block";
  }

  // Let's check if the browser supports notifications
  if (!("Notification" in window)) {
    console.log("This browser does not support notifications.");
  } else {
    Notification.requestPermission().then((permission) => {
      handlePermission(permission);
    });
  }
}
```

This is not only best practice — __you should not be spamming users with notifications they didn't agree to__ — but going forward browsers will explicitly disallow notifications not triggered in response to a user gesture. Firefox is already doing this from version 72, for example.

> Note: As of Firefox 44, the permissions for __Notifications__ and __Push__ have been merged. __If permission is granted for notifications, push will also be enabled.__

Next, a new notification is created using the `Notification()` constructor. This must be passed a title argument, and can optionally be passed an options object to specify options, such as text direction, body text, icon to display, notification sound to play, and more.

## Replacing existing notifications

__It is usually undesirable for a user to receive a lot of notifications in a short space of time — for example, what if a messenger application notified a user for each incoming message?__ To avoid spamming the user with too many notifications, it's possible to modify the pending notifications queue, replacing single or multiple pending notifications with a new one.

To do this, it's possible to add a tag to any new notification. If a notification already has the same tag and has not been displayed yet, the new notification replaces that previous notification. If the notification with the same tag has already been displayed, the previous notification is closed and the new one is displayed.