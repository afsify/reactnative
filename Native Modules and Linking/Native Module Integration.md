# Native Module Integration in React Native

Native modules allow you to access platform-specific functionality that is not available through React Native’s JavaScript API. This is useful when you need to interact with device features, such as the camera, GPS, file system, or custom native libraries, that are not part of the React Native ecosystem.

React Native provides the ability to write custom **native modules** using **Java** (for Android) or **Objective-C/Swift** (for iOS), and then expose them to JavaScript. This allows you to extend React Native's functionality with native code and interact with native APIs directly.

---

### 1. **What are Native Modules?**

A **Native Module** is a bridge between React Native (JavaScript) and native platform code (Java/Objective-C/Swift). It enables React Native apps to use native features that are unavailable through the standard React Native APIs.

#### Key Characteristics:
- Native modules are written in platform-specific languages (Java or Objective-C/Swift).
- They provide access to device features or libraries that aren't part of the core React Native library.
- Native modules are accessed from JavaScript, but the implementation is handled on the native side.

---

### 2. **Why Use Native Modules?**

While React Native provides access to many platform features out-of-the-box, there are cases where you may need to implement functionality that React Native doesn't support natively. This is where native modules come into play. Some common use cases include:

- Accessing native device APIs (e.g., camera, GPS, Bluetooth).
- Integrating third-party native libraries not available in React Native.
- Optimizing performance for complex computations or custom UI components.
- Writing custom native code for specific platform needs.

---

### 3. **Creating a Native Module in React Native**

To create a native module, you need to write some code for both iOS and Android, exposing functionality to JavaScript.

#### 3.1 **Creating a Native Module for Android**

1. **Create the Java Class**: Create a Java class that extends `ReactContextBaseJavaModule` and overrides the `getName` method to return the module's name.

   ```java
   package com.myapp;

   import com.facebook.react.bridge.ReactApplicationContext;
   import com.facebook.react.bridge.ReactContextBaseJavaModule;
   import com.facebook.react.bridge.ReactMethod;

   public class MyNativeModule extends ReactContextBaseJavaModule {
       MyNativeModule(ReactApplicationContext reactContext) {
           super(reactContext);
       }

       @Override
       public String getName() {
           return "MyNativeModule"; // This name will be used to access the module in JavaScript
       }

       @ReactMethod
       public void showMessage(String message) {
           System.out.println(message);  // Example function
       }
   }
   ```

2. **Register the Native Module**: Register the module in the `MainApplication.java` file.

   ```java
   import com.myapp.MyNativeModule;

   @Override
   protected List<ReactPackage> getPackages() {
       return Arrays.<ReactPackage>asList(
               new MainReactPackage(),
               new MyNativeModule()  // Add the module to the package list
       );
   }
   ```

#### 3.2 **Creating a Native Module for iOS**

1. **Create the Objective-C or Swift Class**: For iOS, you can use Objective-C or Swift to implement the native module. Below is an example in Objective-C.

   ```objective-c
   #import <React/RCTBridgeModule.h>

   @interface MyNativeModule : NSObject <RCTBridgeModule>
   @end

   @implementation MyNativeModule

   RCT_EXPORT_MODULE(); // Register the module

   RCT_EXPORT_METHOD(showMessage:(NSString *)message)
   {
       NSLog(@"%@", message);  // Example function
   }

   @end
   ```

2. **Link the Module in `AppDelegate.m`** (for Objective-C) or `AppDelegate.swift` (for Swift):

   ```objective-c
   #import "MyNativeModule.h"
   
   - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
   {
       RCTBridge *bridge = [[RCTBridge alloc] initWithDelegate:self launchOptions:launchOptions];
       return YES;
   }
   ```

---

### 4. **Calling Native Modules from JavaScript**

Once you’ve implemented and registered your native module, you can access it in your JavaScript code.

1. **Import and Call Native Module in JavaScript**: Use the `NativeModules` object from React Native to access the native module.

   ```javascript
   import { NativeModules } from 'react-native';

   const { MyNativeModule } = NativeModules;

   // Calling the native method
   MyNativeModule.showMessage("Hello from Native Module!");
   ```

2. **Handling Asynchronous Methods**: If the native module methods involve asynchronous operations (e.g., network requests, file I/O), you can use Promises or callbacks to handle the results.

   Example using Promises:

   ```javascript
   import { NativeModules } from 'react-native';

   const { MyNativeModule } = NativeModules;

   // Example of a method that returns a Promise
   MyNativeModule.getDataFromNative().then(response => {
     console.log(response);  // Handle the response from native code
   }).catch(error => {
     console.error(error);  // Handle errors
   });
   ```

---

### 5. **Exposing Callbacks to Native Modules**

You can also pass JavaScript functions as callbacks to native code, allowing native code to execute functions when certain tasks complete.

1. **In Java (Android)**:

   ```java
   @ReactMethod
   public void fetchData(Callback successCallback, Callback errorCallback) {
       try {
           // Simulate data fetching
           String data = "Fetched Data";
           successCallback.invoke(data);  // Invoke success callback
       } catch (Exception e) {
           errorCallback.invoke(e.getMessage());  // Invoke error callback
       }
   }
   ```

2. **In JavaScript (React Native)**:

   ```javascript
   import { NativeModules } from 'react-native';

   const { MyNativeModule } = NativeModules;

   // Calling the native method with callbacks
   MyNativeModule.fetchData(
     (data) => {
       console.log("Data fetched successfully: ", data);
     },
     (error) => {
       console.error("Error fetching data: ", error);
     }
   );
   ```

---

### 6. **Handling UI from Native Modules**

In some cases, you may need to update the UI in the React Native app after calling native code (e.g., showing a Toast message, updating a view). You can achieve this by sending events from the native module to JavaScript.

1. **Android**:

   ```java
   @ReactMethod
   public void sendEventToJS(String message) {
       ReactApplicationContext context = getReactApplicationContext();
       context.getJSModule(DeviceEventManagerModule.RCTDeviceEventEmitter.class)
              .emit("onMessageReceived", message);  // Send event to JS
   }
   ```

2. **iOS**:

   ```objective-c
   #import <React/RCTEventEmitter.h>

   @interface MyNativeModule : RCTEventEmitter
   @end

   @implementation MyNativeModule

   RCT_EXPORT_MODULE();

   - (NSArray<NSString *> *)supportedEvents {
       return @[@"onMessageReceived"];  // Declare supported events
   }

   RCT_EXPORT_METHOD(sendEventToJS:(NSString *)message)
   {
       [self sendEventWithName:@"onMessageReceived" body:@{@"message": message}];  // Send event to JS
   }

   @end
   ```

3. **JavaScript** (React Native):

   ```javascript
   import { DeviceEventEmitter } from 'react-native';

   // Listening for the event from native
   DeviceEventEmitter.addListener('onMessageReceived', (event) => {
     console.log(event.message);  // Handle event data
   });

   // Calling native method to send event
   MyNativeModule.sendEventToJS("Hello from Native");
   ```

---

### 7. **Linking Native Modules**

React Native allows you to automatically link native modules in your project using **autolinking** (React Native 0.60+). However, for older versions (pre-0.60), you may need to link native modules manually.

#### 7.1 **Autolinking (React Native 0.60 and above)**

React Native automatically links native modules with `react-native link`. It also automatically adds necessary files to both Android and iOS projects, so you don’t need to manually link them.

#### 7.2 **Manual Linking**

For versions below 0.60 or when autolinking fails, you may need to manually link your native modules in the Android and iOS build configurations.

---

### 8. **Conclusion**

Native module integration in React Native allows you to extend the platform capabilities and access device-specific features. By using Java for Android and Objective-C/Swift for iOS, you can implement platform-specific logic and expose it to JavaScript, enabling richer functionality in your React Native applications.

Key points:
- **Platform-Specific Code**: Write native modules in Java (Android) or Objective-C/Swift (iOS).
- **JavaScript Integration**: Call native modules from JavaScript using `NativeModules`.
- **Callbacks & Promises**: Handle asynchronous native module methods.
- **UI Updates**: Send events from native modules to JavaScript for UI updates.
- **Autolinking**: React Native automatically links native modules (0.60+).

Native module integration is a powerful feature that unlocks many possibilities for building feature-rich cross-platform applications.