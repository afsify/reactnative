# **React Native Bridge: Notes**

The **React Native Bridge** is the communication layer that connects JavaScript code (written in React Native) with native code (written in Java, Objective-C, Swift, or Kotlin). This allows React Native apps to access platform-specific functionality and use native device APIs, which is essential when JavaScript alone cannot provide the required functionality.

---

### 1. **Overview of the React Native Bridge**

The **React Native Bridge** is responsible for enabling communication between the **JavaScript thread** and the **Native thread**. React Native works in two threads:

- **JavaScript Thread**: This is where your React Native code runs. It is responsible for rendering UI and business logic.
- **Native Thread**: This is where platform-specific code (iOS/Android) runs. Native modules are executed in this thread.

The bridge enables asynchronous communication between these threads, making it possible for the JavaScript code to interact with native code and vice versa.

---

### 2. **How the React Native Bridge Works**

The React Native Bridge works in two main directions:

- **JavaScript to Native**: JavaScript code calls native APIs or methods through the bridge.
- **Native to JavaScript**: Native code sends data, events, or responses to the JavaScript side.

The communication happens in an asynchronous manner, ensuring that the UI thread remains unblocked.

#### 2.1 **JavaScript to Native Communication**
- JavaScript can call methods on native modules through the **React Native Bridge**.
- The JavaScript thread sends a message to the native thread with the method name and arguments.
- The native thread processes the request and sends back the result.

#### 2.2 **Native to JavaScript Communication**
- Native code can send data or events back to JavaScript through **callback functions** or **Promises**.
- Events can be emitted by native modules using the bridge to notify the JavaScript side of certain changes or data updates.

---

### 3. **Components of the React Native Bridge**

#### 3.1 **Native Modules**
- Native modules are written in Java (Android) or Objective-C/Swift (iOS) and expose functionality to JavaScript.
- These modules are registered and accessible in JavaScript via the **NativeModules** object.

Example (Java):

```java
package com.example;

import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.bridge.ReactContextBaseJavaModule;
import com.facebook.react.bridge.ReactMethod;

public class MyNativeModule extends ReactContextBaseJavaModule {
  
  public MyNativeModule(ReactApplicationContext reactContext) {
    super(reactContext);
  }

  @Override
  public String getName() {
    return "MyNativeModule";
  }

  @ReactMethod
  public void showToast(String message) {
    Toast.makeText(getReactApplicationContext(), message, Toast.LENGTH_SHORT).show();
  }
}
```

JavaScript Usage:

```javascript
import { NativeModules } from 'react-native';

const { MyNativeModule } = NativeModules;
MyNativeModule.showToast("Hello from Native!");
```

#### 3.2 **Native UI Components**
- React Native allows you to create custom native UI components for specific platform designs or performance requirements.
- These components are created and managed by a **View Manager** on the native side.

Example (Android):

```java
public class MyCustomView extends View {
  public MyCustomView(Context context) {
    super(context);
    setBackgroundColor(Color.BLUE);
  }
}
```

---

### 4. **Data Types and Serialization**

The data passed through the bridge must be serialized to ensure compatibility between JavaScript and native code. React Native supports various data types, and the bridge automatically serializes them.

- **Primitive Data Types**: Strings, numbers, booleans are passed without modification.
- **Complex Data Types**: Arrays, objects, and arrays of objects are serialized to JSON format.

Custom data types need to be serialized and deserialized in the native module code.

---

### 5. **Asynchronous Nature of the Bridge**

Communication through the bridge is **asynchronous**, meaning that JavaScript does not block the UI thread while waiting for responses from the native side. This asynchronous design ensures that the UI remains responsive.

- **Callbacks**: Native modules often use callbacks to send data back to JavaScript.
- **Promises**: Native modules can return promises to handle asynchronous operations.

---

### 6. **Threading and Performance Considerations**

Since the JavaScript thread and the native thread are separate, proper management of the bridge's communication is critical for app performance:

- **Thread Blocking**: Heavy computation on the bridge thread can block communication, affecting performance. It’s important to offload heavy work to separate threads where possible.
- **UI Thread**: Keep UI updates on the UI thread and avoid blocking it with heavy native operations.

---

### 7. **Creating a Native Module with the Bridge**

To create a custom native module, follow these steps:

1. **Create a Native Module**: Write the code in Java (Android) or Objective-C/Swift (iOS).
2. **Expose Methods to JavaScript**: Use annotations (`@ReactMethod` in Android) to expose the methods you want to call from JavaScript.
3. **Register the Native Module**: Register your native module in the package.
4. **Use the Module in JavaScript**: Import and use the module in your JavaScript code.

Example (iOS - Objective-C):

```objc
#import <React/RCTBridgeModule.h>

@interface MyNativeModule : NSObject <RCTBridgeModule>
@end

@implementation MyNativeModule

RCT_EXPORT_MODULE();

RCT_EXPORT_METHOD(showToast:(NSString *)message)
{
  UIAlertController *alert = [UIAlertController alertControllerWithTitle:@"Native Toast"
                                                             message:message
                                                      preferredStyle:UIAlertControllerStyleAlert];
  
  UIViewController *rootViewController = [UIApplication sharedApplication].keyWindow.rootViewController;
  [rootViewController presentViewController:alert animated:YES completion:nil];
}

@end
```

---

### 8. **Communication with JavaScript Using Events**

Native modules can also send events back to JavaScript using the bridge:

1. **Emit Events from Native**: Use `RCTDeviceEventEmitter` on Android or `RCTEventEmitter` on iOS to send events to JavaScript.
2. **Listen for Events in JavaScript**: JavaScript can subscribe to events and handle them when emitted.

Example (Android):

```java
import com.facebook.react.modules.core.DeviceEventManagerModule;

public class MyNativeModule extends ReactContextBaseJavaModule {

  @ReactMethod
  public void sendEventToJS(String message) {
    getReactApplicationContext()
        .getJSModule(DeviceEventManagerModule.RCTDeviceEventEmitter.class)
        .emit("EventFromNative", message);
  }
}
```

In JavaScript:

```javascript
import { DeviceEventEmitter } from 'react-native';

DeviceEventEmitter.addListener('EventFromNative', (message) => {
  console.log(message);
});
```

---

### 9. **Best Practices for Using the Bridge**

- **Avoid Blocking the UI**: Use background threads for long-running native tasks.
- **Minimize Bridge Overhead**: Minimize the number of calls between JavaScript and native code to avoid performance bottlenecks.
- **Use Promises**: For handling asynchronous tasks, use promises to make the flow more manageable and error handling easier.
- **Efficient Data Serialization**: Ensure proper serialization of complex objects to minimize performance overhead.
- **Error Handling**: Implement robust error handling to ensure smooth communication between JavaScript and native code.

---

### 10. **Debugging the Bridge**

You can debug bridge communication by:

- **Console Logging**: Log messages on both the JavaScript and native side.
- **Using Xcode/Android Studio**: Use the native debuggers to inspect native module behavior.
- **Remote Debugging**: Enable remote debugging for JavaScript to inspect issues in communication or performance.

---

### 11. **Conclusion**

The **React Native Bridge** is a fundamental part of React Native’s architecture, enabling seamless communication between JavaScript and native code. Understanding how to efficiently use the bridge for communication, native modules, and native UI components can significantly enhance the functionality and performance of your React Native apps. 

By following best practices, managing thread performance, and ensuring proper serialization, you can create robust and performant React Native applications that leverage platform-specific capabilities.