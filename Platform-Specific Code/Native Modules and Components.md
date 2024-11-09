# **Native Modules and Components in React Native**

React Native provides the ability to bridge the gap between JavaScript code and native platform code, allowing you to use native components and functionality within your React Native applications. This is achieved through **Native Modules** and **Native Components**, which allow you to access platform-specific APIs and create custom components that directly interact with native code.

---

### 1. **Overview of Native Modules and Components**

- **Native Modules**: These are Java or Objective-C/Swift classes that expose native functionality to JavaScript code. Native modules allow you to call platform-specific APIs or access native features such as device sensors, camera, geolocation, and other services.
  
- **Native Components**: These are UI components written in native code (Android/iOS) that can be used within your React Native app, providing better performance or native platform look and feel.

Both **Native Modules** and **Native Components** allow you to bridge JavaScript with native code to access and use functionality that cannot be achieved using the standard React Native APIs.

---

### 2. **Creating a Native Module**

Native modules are typically created in Java or Objective-C (for iOS) or Java (for Android). Below is a simple example of creating a native module in React Native.

#### 2.1 **Create the Native Module (Android Example)**

For Android, native modules are written in Java. You can follow these steps:

1. **Create a Java class for the module**:

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

2. **Register the Module**:
   - In your `MainApplication.java`, register the native module:

```java
import com.example.MyNativeModule;  // Import your module
import com.facebook.react.bridge.NativeModule;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.bridge.ReactContextBaseJavaModule;
import com.facebook.react.bridge.ReactPackage;

import java.util.Collections;
import java.util.List;

public class MainApplication extends Application implements ReactApplication {
  @Override
  public List<ReactPackage> getPackages() {
    return Arrays.asList(
      new MainReactPackage(),
      new MyNativeModule()  // Add your module here
    );
  }
}
```

#### 2.2 **Using the Native Module in JavaScript**

Once the native module is created, you can use it in your JavaScript code as follows:

```javascript
import { NativeModules } from 'react-native';

// Access the native module
const { MyNativeModule } = NativeModules;

// Call the method from the native module
MyNativeModule.showToast("Hello from Native!");
```

#### 2.3 **Creating a Native Module for iOS (Objective-C Example)**

For iOS, native modules are written in Objective-C or Swift.

1. **Create an Objective-C class for the module**:

```objc
#import <React/RCTBridgeModule.h>
#import <UIKit/UIKit.h>

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

2. **Access and use the native module in JavaScript**:

```javascript
import { NativeModules } from 'react-native';

const { MyNativeModule } = NativeModules;
MyNativeModule.showToast("Hello from Native!");
```

---

### 3. **Creating Native Components**

Native components are UI components written in native code, allowing you to implement platform-specific designs or performance optimizations. Native components can be written for both Android (Java/Kotlin) and iOS (Objective-C/Swift).

#### 3.1 **Creating a Native Component (Android Example)**

1. **Create a custom view (Android)**:

```java
package com.example;

import android.content.Context;
import android.graphics.Color;
import android.view.View;

public class MyCustomView extends View {
  
  public MyCustomView(Context context) {
    super(context);
    setBackgroundColor(Color.BLUE);
  }
}
```

2. **Create a view manager to integrate with React Native**:

```java
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.uimanager.SimpleViewManager;
import com.facebook.react.uimanager.ThemedReactContext;

public class MyCustomViewManager extends SimpleViewManager<View> {

  @Override
  public String getName() {
    return "MyCustomView";
  }

  @Override
  public View createViewInstance(ThemedReactContext reactContext) {
    return new MyCustomView(reactContext);
  }
}
```

3. **Register the View Manager**:

```java
import com.facebook.react.bridge.ReactPackage;
import com.facebook.react.bridge.NativeModule;
import com.facebook.react.uimanager.ViewManager;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class MyReactPackage implements ReactPackage {

  @Override
  public List<NativeModule> createNativeModules(ReactApplicationContext reactContext) {
    return Collections.emptyList();
  }

  @Override
  public List<ViewManager> createViewManagers(ReactApplicationContext reactContext) {
    List<ViewManager> viewManagers = new ArrayList<>();
    viewManagers.add(new MyCustomViewManager());
    return viewManagers;
  }
}
```

#### 3.2 **Use the Native Component in JavaScript**

Once the native component is created and registered, you can use it in your React Native app like any other component.

```javascript
import { requireNativeComponent } from 'react-native';

const MyCustomView = requireNativeComponent('MyCustomView');

// Usage in JSX
<MyCustomView style={{ width: 200, height: 200 }} />
```

---

### 4. **Communication Between JavaScript and Native Code**

#### 4.1 **Sending Data from JavaScript to Native**

In the previous examples, the JavaScript code calls methods on the native module. You can also send data from JavaScript to native code through **method calls**.

For instance:

```javascript
MyNativeModule.showToast("Message from JS");
```

#### 4.2 **Sending Data from Native to JavaScript**

To send data back from native code to JavaScript, you use **Promises** or **Callback functions**. For example:

1. **Using Promises**:

```java
@ReactMethod
public Promise fetchDataFromNative() {
  // Fetch data asynchronously and return a promise
  return Promise.resolve("Data from native");
}
```

In JavaScript:

```javascript
MyNativeModule.fetchDataFromNative()
  .then(data => console.log(data))
  .catch(error => console.log(error));
```

2. **Using Callbacks**:

```java
@ReactMethod
public void fetchDataFromNative(Callback callback) {
  // Fetch data and pass it back to JS
  callback.invoke("Data from native");
}
```

In JavaScript:

```javascript
MyNativeModule.fetchDataFromNative((response) => {
  console.log(response);
});
```

---

### 5. **Debugging Native Modules and Components**

To debug native modules and components:
- **Android**: Use Android Studio’s logcat to see logs.
- **iOS**: Use Xcode’s console for debugging.
- Enable **remote debugging** for JavaScript, and use the native debugger to inspect native code execution.

---

### 6. **Best Practices**

- **Modularize**: Split your native modules into small, reusable components.
- **Error Handling**: Implement proper error handling in your native modules and communicate errors to JavaScript via callbacks or promises.
- **Cross-Platform Code**: Ensure that your native modules work on both Android and iOS. Use platform-specific checks when necessary.
- **Test with Native Apps**: Test native modules independently in the platform’s native app before integrating them into React Native.

---

### 7. **Conclusion**

Native modules and components are essential when you need to access native device features or implement platform-specific UI elements that are not available in React Native's built-in API. By understanding how to create and use native modules and components, you can greatly extend the functionality and performance of your React Native applications, bridging the gap between JavaScript and native code for a seamless experience.
