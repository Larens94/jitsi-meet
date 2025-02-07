# <p align="center">Jitsi Meet React Native SDK</p>


## Installation
Inside your project, run;
```console
npm i @jitsi/react-native-sdk
```
<br/><br/>Additionally, if not already installed, some dependencies will need to be added.

This can be done by running the following script:
```console
node node_modules/@jitsi/react-native-sdk/update_dependencies.js
```
This will check and update all your dependencies.<br/><br/>

 [comment]: # (These deps definitely need to be added manually, more could be neccesary)

Because of SVG use in react native, you need to update metro.config your project's file:

```javascript
const { getDefaultConfig } = require('metro-config');

module.exports = (async () => {
  const {
    resolver: {
      sourceExts,
      assetExts
    }
  } = await getDefaultConfig();

  return {
    transformer: {
      babelTransformerPath: require.resolve('react-native-svg-transformer'),
      getTransformOptions: async () => ({
        transform: {
          experimentalImportSupport: false,
          inlineRequires: true,
        },
      }),
    },
    resolver: {
      assetExts: assetExts.filter(ext => ext !== 'svg'),
      sourceExts: [...sourceExts, 'svg']
    }
  }
})();
```

### iOS

#### Project Info.plist
- Add a *Privacy - Camera Usage Description*
- Add a *Privacy - Microphone Usage Description*

#### General
- Signing & capabilites:
    - Add Background modes
        - Audio
        - Voice over IP
        - Background fetch

Run;
```console
cd ios && pod install && cd ..
```

### Android

- In your build.gradle have at least `minSdkVersion = 24`
- In `android/app/src/debug/AndroidManifest.xml` and `android/app/src/main/AndroidManifest.xml`, under the `</application>` tag, include
  ```xml
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.CAMERA" />
  ```

If you want to test all the steps before applying them to your app, you can check our React Native SDK sample app here:
https://github.com/jitsi/jitsi-meet-sdk-samples/tree/master/react-native


### Using JWT tokens
- If you are planning to use tokens you can update your domain and room name inside meeting options.
- For example:
```javascript
  const meetingOptions = {
     domain: 'YOUR_DOMAIN',
     roomName: 'ROOM_NAME?jwt=JWT_TOKEN'
  }
```
