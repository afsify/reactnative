# Using Third-Party Native Modules in React Native

React Native allows you to write mobile applications using JavaScript and React. However, there may be situations where you need to use native functionality that is not available through the standard React Native APIs. In these cases, you can leverage **third-party native modules** to extend your app's capabilities.

A **native module** is a piece of code written in Java (for Android) or Objective-C/Swift (for iOS) that exposes native functionality to the React Native JavaScript runtime. Third-party native modules are libraries created by the community or third-party companies to access device features like camera, GPS, accelerometer, push notifications, etc., that React Native’s core APIs do not cover out-of-the-box.

---

### 1. **What is a Third-Party Native Module?**

A **third-party native module** is a pre-built library or component that allows you to access native functionality in your React Native app. These modules are written in Java or Objective-C (or Swift for iOS) and are typically available as npm packages. You can use these modules to integrate additional platform-specific functionality into your app without having to write your own native code.

Examples of common third-party native modules include:
- **React Native Camera** (for camera functionality)
- **React Native Push Notifications** (for push notifications)
- **React Native Maps** (for map integration)

---

### 2. **Installing a Third-Party Native Module**

To use a third-party native module in your React Native project, you need to install it through npm or yarn.

Example using npm:
```bash
npm install <module-name> --save
```

Example using yarn:
```bash
yarn add <module-name>
```

Once the module is installed, you may need to link the native module (in older versions of React Native) or run a command to ensure the module is properly integrated with your project.

For **React Native 0.60+** (autolinking):
- Native modules are automatically linked to the project during the build process. No need for manual linking.

For **React Native 0.59 and below** (manual linking):
- You need to manually link the native module:
  ```bash
  react-native link <module-name>
  ```

---

### 3. **Using a Third-Party Native Module**

After installing and linking the third-party module, you can use it just like any other React Native component or API. You will usually import the module into your JavaScript code and call its methods to interact with the native functionality.

#### Example: Using React Native Camera

Install the `react-native-camera` module:
```bash
npm install react-native-camera --save
```

Use the module in your code:

```javascript
import React, { useState } from 'react';
import { View, Button } from 'react-native';
import { RNCamera } from 'react-native-camera';

const CameraScreen = () => {
  const [isRecording, setIsRecording] = useState(false);

  const startRecording = async (camera) => {
    if (camera) {
      const options = { quality: 'high', base64: true, maxDuration: 30 };
      const data = await camera.recordAsync(options);
      console.log(data.uri); // URI of the recorded video
    }
  };

  return (
    <View style={{ flex: 1 }}>
      <RNCamera
        style={{ flex: 1 }}
        type={RNCamera.Constants.Type.back}
        flashMode={RNCamera.Constants.FlashMode.on}
        captureAudio={true}
      />
      <Button
        title={isRecording ? "Stop Recording" : "Start Recording"}
        onPress={() => setIsRecording(!isRecording)}
      />
    </View>
  );
};

export default CameraScreen;
```

In this example:
- The `RNCamera` component is used to display the camera view.
- The `recordAsync()` method is used to start and stop video recording.

---

### 4. **Platform-Specific Code**

Sometimes, you may need to write platform-specific code to handle differences between iOS and Android or to use different native modules for each platform. You can use the `Platform` module from React Native to handle this.

```javascript
import { Platform, Text } from 'react-native';
import { Camera } from 'react-native-camera';

const CameraComponent = () => {
  if (Platform.OS === 'ios') {
    return <Text>Using iOS Camera</Text>;
  } else if (Platform.OS === 'android') {
    return <Text>Using Android Camera</Text>;
  }
  return <Text>Using Web Camera</Text>;
};
```

You can conditionally load different modules or handle platform-specific behavior by checking the platform type.

---

### 5. **Permissions for Native Modules**

Many third-party native modules require permissions to access native features, such as the camera, microphone, or location. React Native has built-in APIs like `PermissionsAndroid` (for Android) and `react-native-permissions` to manage permissions.

Example (requesting camera permission on Android):
```javascript
import { PermissionsAndroid, Platform } from 'react-native';

const requestCameraPermission = async () => {
  if (Platform.OS === 'android') {
    try {
      const granted = await PermissionsAndroid.request(
        PermissionsAndroid.PERMISSIONS.CAMERA,
        {
          title: 'Camera Permission',
          message: 'We need access to your camera to take pictures',
          buttonNeutral: 'Ask Me Later',
          buttonNegative: 'Cancel',
          buttonPositive: 'OK',
        }
      );
      if (granted === PermissionsAndroid.RESULTS.GRANTED) {
        console.log('Camera permission granted');
      } else {
        console.log('Camera permission denied');
      }
    } catch (err) {
      console.warn(err);
    }
  }
};
```

For iOS, permissions are configured in the `Info.plist` file, and you can request permissions using `react-native-permissions` or manually using native code.

---

### 6. **Common Third-Party Native Modules**

Here are some commonly used third-party native modules in React Native:

- **`react-native-camera`**: Provides access to the device's camera.
- **`react-native-location`**: For accessing the device's location.
- **`react-native-maps`**: A library for embedding maps in your React Native app.
- **`react-native-push-notification`**: Handles local and push notifications.
- **`react-native-fs`**: Provides file system APIs for reading and writing files.
- **`react-native-video`**: For playing video files.
- **`react-native-contacts`**: For accessing contacts stored on the device.

---

### 7. **Troubleshooting Third-Party Native Modules**

- **Linking Issues**: If the module does not work as expected, try to manually link it using `react-native link` (if using older React Native versions).
- **Compatibility**: Make sure the third-party module is compatible with your version of React Native. Check the module's documentation for the correct version or any known issues.
- **Native Code Changes**: Some modules may require changes to the native code (e.g., iOS's `Info.plist` or Android's `AndroidManifest.xml`). Always follow the installation instructions carefully to ensure correct setup.

---

### 8. **Best Practices**

- **Modularization**: Keep your codebase modular by using third-party modules only when necessary and only for features you can't achieve with React Native's built-in APIs.
- **Use Autolinking (RN 0.60+)**: React Native now supports autolinking for most third-party native modules. If you are using React Native 0.60 or above, autolinking should handle the native linking process automatically.
- **Stay Updated**: Always ensure the third-party modules are actively maintained and compatible with the latest React Native versions.
- **Error Handling**: Ensure proper error handling, especially when dealing with native code that might not work across all devices or platforms.

---

### 9. **Conclusion**

Third-party native modules provide a powerful way to access native functionality in React Native apps. By using these modules, you can easily integrate platform-specific features like camera access, push notifications, and more. Make sure to follow installation instructions carefully, handle permissions, and write platform-specific code where necessary. By using these modules, you can significantly enhance the capabilities of your React Native app while keeping the majority of the development in JavaScript.