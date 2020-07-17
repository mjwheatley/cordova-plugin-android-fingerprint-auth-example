```
cordova create cordova-plugin-android-fingerprint-auth-example com.mawhea.fingerprintauth.example AndroidFingerprintAuthExample
cd cordova-plugin-android-fingerprint-auth-example
cordova platform add android
cordova plugin add cordova-plugin-android-fingerprint-auth
```

Add the following snippet to the `onDeviceReady` method in `www/js/index.js`

```javascript
// Check if device supports fingerprint
/**
 * @return Object {
 *      isAvailable:boolean,
 *      isHardwareDetected:boolean,
 *      hasEnrolledFingerprints:boolean
 *   }
 */
FingerprintAuth.isAvailable(function (result) {

  console.log("FingerprintAuth available: " + JSON.stringify(result));

  // If has fingerprint device and has fingerprints registered
  if (result.isAvailable == true && result.hasEnrolledFingerprints == true) {

    // Check the docs to know more about the encryptConfig object :)
    var encryptConfig = {
      clientId: "myAppName",
      username: "currentUser",
      password: "currentUserPassword",
      maxAttempts: 5,
      locale: "en_US"
    }; // See config object for required parameters

    // Set config and success callback
    FingerprintAuth.encrypt(encryptConfig, function (_fingerResult) {
      console.log("successCallback(): " + JSON.stringify(_fingerResult));
      if (_fingerResult.withFingerprint) {
        console.log("Successfully encrypted credentials.");
        console.log("Encrypted credentials: " + result.token);
      } else if (_fingerResult.withBackup) {
        console.log("Authenticated with backup password");
      }
      // Error callback
    }, function (err) {
      if (err === "Cancelled") {
        console.log("FingerprintAuth Dialog Cancelled!");
      } else {
        console.log("FingerprintAuth Error: " + err);
      }
    });
  }

  /**
   * @return Object {
   *      isAvailable:boolean,
   *      isHardwareDetected:boolean,
   *      hasEnrolledFingerprints:boolean
   *   }
   */
}, function (message) {
  console.log("isAvailableError(): " + message);
});
```

Test the app
```
cordova emulate android
```

Create a User Interface
* Create inputs for username, password, and token
* Create buttons for Encrypt and Decrypt
* Add a text box to display the encrypted token
* Add a text box to display the decrypted password