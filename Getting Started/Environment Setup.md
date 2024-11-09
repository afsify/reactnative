# Environment Setup for React Native

Setting up a development environment for React Native involves installing various tools and dependencies. This ensures you can create, run, and test applications seamlessly on both Android and iOS.

---

### Key Steps and Requirements for React Native Environment Setup

1. **Node.js and npm (Node Package Manager) Installation**

   - **Node.js**: React Native relies on Node.js, a JavaScript runtime, to run various scripts and manage dependencies.
   - **npm**: It is bundled with Node.js and is used for installing React Native CLI and other dependencies.

   **Installation:**
   - Download Node.js from [nodejs.org](https://nodejs.org/).
   - Verify installation:
     ```bash
     node -v
     npm -v
     ```

2. **React Native CLI vs. Expo CLI**

   - **React Native CLI**: Provides full access to native modules, recommended for developing native modules and more complex applications.
   - **Expo CLI**: A simpler setup with pre-configured environments; ideal for rapid prototyping but limited access to native modules.

   **Installation:**
   - React Native CLI:
     ```bash
     npm install -g react-native-cli
     ```
   - Expo CLI:
     ```bash
     npm install -g expo-cli
     ```

3. **Watchman (for macOS)**

   - **Watchman**: A file-watching service by Facebook, used by React Native to track file changes.
   - **Installation**:
     - Install with Homebrew (macOS):
       ```bash
       brew install watchman
       ```

4. **Java Development Kit (JDK)**

   - Required to build and run React Native projects on Android. The recommended JDK version for React Native is OpenJDK 11.
   - **Installation**:
     - Download from [AdoptOpenJDK](https://adoptopenjdk.net/).
     - Verify installation:
       ```bash
       java -version
       ```

5. **Android Studio**

   - **Android Studio**: The official IDE for Android development, needed to set up an Android emulator and manage Android SDK.
   - **Installation**:
     - Download from [developer.android.com](https://developer.android.com/studio).
     - Select the Android Virtual Device (AVD) and Android SDK options during installation.

   - **Environment Variables for Android**:
     Add the Android SDK paths to your `PATH` environment variable.

     ```bash
     # Add the following lines to your profile file (e.g., ~/.bashrc or ~/.zshrc)
     export ANDROID_HOME=$HOME/Library/Android/sdk
     export PATH=$PATH:$ANDROID_HOME/emulator
     export PATH=$PATH:$ANDROID_HOME/tools
     export PATH=$PATH:$ANDROID_HOME/tools/bin
     export PATH=$PATH:$ANDROID_HOME/platform-tools
     ```

   - **Android Virtual Device (AVD)**:
     - Open Android Studio → `AVD Manager` → `Create Virtual Device…`
     - Choose a device model and a system image.

6. **Xcode (for iOS Development)**

   - Required to build and run React Native projects on iOS.
   - **Installation**:
     - Download Xcode from the Mac App Store.
     - Open Xcode and agree to the license agreement.
   - **Command Line Tools**:
     ```bash
     xcode-select --install
     ```
   - **Simulator**:
     - Open Xcode → Preferences → Components → Install iOS Simulator.

---

### Setting Up Your First React Native Project

1. **Creating a New Project**

   - **Using React Native CLI**:
     ```bash
     npx react-native init ProjectName
     ```
   - **Using Expo CLI**:
     ```bash
     expo init ProjectName
     ```

2. **Running the Project**

   - **Android (React Native CLI)**:
     ```bash
     npx react-native run-android
     ```
   - **iOS (React Native CLI)**:
     ```bash
     npx react-native run-ios
     ```
   - **Expo**:
     ```bash
     cd ProjectName
     expo start
     ```
     Open the Expo Go app on your device and scan the QR code to run the app.

3. **Debugging Tools**

   - **React Native Debugger**: A standalone debugger with React DevTools and Redux DevTools integration.
     - Download from [GitHub](https://github.com/jhen0409/react-native-debugger).
   - **React Developer Tools**:
     ```bash
     npm install -g react-devtools
     ```

4. **Testing the App**

   - **On Physical Devices**:
     - Android: Enable Developer Mode and USB Debugging.
     - iOS: Connect your device via Xcode and configure the necessary provisioning profiles.

5. **Hot Reloading**

   - React Native supports **Fast Refresh**, allowing you to see changes instantly as you save files.

---

### Example: Running a Basic App

1. **Open `App.js`** in your React Native project.
2. Replace the content with:

   ```javascript
   import React from 'react';
   import { View, Text } from 'react-native';

   const App = () => {
     return (
       <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
         <Text>Hello, React Native!</Text>
       </View>
     );
   };

   export default App;
   ```

3. Save the file. The emulator or connected device should reflect the changes immediately.

---

### Troubleshooting Common Issues

- **“Command not found” Errors**: Ensure all environment variables are set up correctly.
- **AVD not starting**: Check if the Android SDK path is correctly added.
- **Cannot find module 'react-native'**: Verify that the project dependencies are installed (`npm install`).

---

### Summary of Key Concepts

- **CLI Options**: React Native CLI for native module access; Expo CLI for simplicity.
- **Environment Setup**: Install Node.js, Android Studio, and Xcode.
- **Running and Debugging**: Use emulators, physical devices, and debugging tools.
- **Code Example**: Sample code to display “Hello, React Native!” on the screen.

This guide ensures you have all the tools to start building applications with React Native on both Android and iOS.