# Permissions API

[MDN](https://developer.mozilla.org/en-US/docs/Web/API/Permissions_API)

Permissions API can be used to determine if permission to access a particular API has been granted or denied, or requires specific user permission.

Historically different APIs handle their own permissions inconsistently — for example the Notifications API provided its own methods for requesting permissions and checking permission status, whereas the Geolocation API did not. The Permissions API provides the tools to allow developers to implement a consistent and better user experience for working with permissions.

The permissions property has been made available on the `Navigator` object, both in the standard browsing context and the worker context (`WorkerNavigator` — so permission checks are available inside workers), and returns a `Permissions` object that provides access to the Permissions API functionality.

## Permission-aware APIs

- __Background Synchronization API__: `background-sync` (should always be granted)

- __Clipboard API__: `clipboard-read`, `clipboard-write`

- __Geolocation API__: `geolocation`

- __Local Font Access API__

- __Media Capture and Streams API__: `microphone`, `camera`

- __Notifications API__: `notifications`

- __Payment Handler API__: `payment-handler`

- __Push API__: `push`

- __Sensor APIs__: `accelerometer`, `gyroscope`, `magnetometer`, `ambient-light-sensor`

- __Storage Access API__: `storage-access`

- __Storage API__: `persistent-storage`

- __Web Audio Output Devices API__: `speaker-selection`

- __Web MIDI API__: `midi`
