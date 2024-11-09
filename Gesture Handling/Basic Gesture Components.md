# Basic Gesture Components in React Native

In React Native, gestures are a critical part of building interactive and fluid user interfaces. The **Basic Gesture Components** allow developers to handle different types of gestures such as tapping, swiping, pinching, and dragging. React Native provides libraries such as **React Native Gesture Handler** and **React Native Reanimated** to manage gestures efficiently.

---

### 1. **Introduction to Gesture Handling**

Gestures in mobile applications are interactions where the user makes a specific motion on the screen, such as tapping, swiping, or pinching. Handling gestures effectively enhances the user experience and adds interactivity to your app. React Native Gesture Handler and React Native Reanimated work together to help handle gestures in an optimized manner.

---

### 2. **Installing React Native Gesture Handler**

To start using gesture components in React Native, you need to install the `react-native-gesture-handler` library.

```bash
npm install react-native-gesture-handler
```

After installing the package, ensure you follow the installation instructions specific to your platform (iOS/Android) for linking the package.

---

### 3. **Basic Gesture Handler Components**

React Native Gesture Handler offers a number of core components to handle gestures. The most common components include:

#### 3.1 **TouchableOpacity** (Basic Tap Gesture)
This component is used to detect tap gestures, and it automatically applies a fade effect on the target component when tapped.

```javascript
import React from 'react';
import { TouchableOpacity, Text } from 'react-native';

const App = () => {
  return (
    <TouchableOpacity
      onPress={() => console.log('Tapped!')}
      style={{ backgroundColor: 'blue', padding: 10 }}
    >
      <Text style={{ color: 'white' }}>Tap Me</Text>
    </TouchableOpacity>
  );
};

export default App;
```

#### 3.2 **TouchableWithoutFeedback** (Tap Gesture without Feedback)
This component detects taps but does not provide any visual feedback like changing opacity.

```javascript
import React from 'react';
import { TouchableWithoutFeedback, Text } from 'react-native';

const App = () => {
  return (
    <TouchableWithoutFeedback onPress={() => console.log('Tapped without feedback!')}>
      <Text>Tap Here</Text>
    </TouchableWithoutFeedback>
  );
};

export default App;
```

#### 3.3 **PanGestureHandler** (Drag Gesture)
The `PanGestureHandler` component detects dragging gestures, allowing you to track the movement of a finger across the screen. You can access the movement details, like position and velocity, to animate or adjust the UI accordingly.

```javascript
import React from 'react';
import { View } from 'react-native';
import { PanGestureHandler } from 'react-native-gesture-handler';

const App = () => {
  const onGestureEvent = (event) => {
    console.log(event.nativeEvent.translationX, event.nativeEvent.translationY);
  };

  return (
    <PanGestureHandler onGestureEvent={onGestureEvent}>
      <View style={{ width: 100, height: 100, backgroundColor: 'red' }} />
    </PanGestureHandler>
  );
};

export default App;
```

- `translationX` and `translationY` provide the movement distance along the X and Y axes, respectively.

---

#### 3.4 **TapGestureHandler** (Single and Double Tap Gestures)
This component detects tap gestures, including single taps and double taps.

```javascript
import React from 'react';
import { Text, View } from 'react-native';
import { TapGestureHandler } from 'react-native-gesture-handler';

const App = () => {
  const onSingleTap = () => console.log('Single Tap');
  const onDoubleTap = () => console.log('Double Tap');

  return (
    <View>
      <TapGestureHandler onHandlerStateChange={onSingleTap} numberOfTaps={1}>
        <Text>Tap Me</Text>
      </TapGestureHandler>

      <TapGestureHandler onHandlerStateChange={onDoubleTap} numberOfTaps={2}>
        <Text>Double Tap Me</Text>
      </TapGestureHandler>
    </View>
  );
};

export default App;
```

- **`numberOfTaps`**: Defines how many taps need to be detected for the gesture to be triggered (e.g., `1` for single tap, `2` for double tap).

---

#### 3.5 **LongPressGestureHandler** (Long Press Gesture)
The `LongPressGestureHandler` component detects when the user presses and holds a point on the screen for a specified duration.

```javascript
import React from 'react';
import { Text, View } from 'react-native';
import { LongPressGestureHandler } from 'react-native-gesture-handler';

const App = () => {
  const onLongPress = () => console.log('Long Press Detected!');

  return (
    <LongPressGestureHandler onHandlerStateChange={onLongPress} minDurationMs={800}>
      <Text style={{ padding: 20 }}>Long Press Me</Text>
    </LongPressGestureHandler>
  );
};

export default App;
```

- **`minDurationMs`**: Specifies the minimum duration (in milliseconds) for the long press gesture to be recognized.

---

#### 3.6 **RotationGestureHandler** (Rotation Gesture)
This component is used to handle rotation gestures, useful for actions like rotating an object on the screen.

```javascript
import React from 'react';
import { View } from 'react-native';
import { RotationGestureHandler } from 'react-native-gesture-handler';

const App = () => {
  const onGestureEvent = (event) => {
    console.log(event.nativeEvent.rotation); // Rotation angle
  };

  return (
    <RotationGestureHandler onGestureEvent={onGestureEvent}>
      <View style={{ width: 100, height: 100, backgroundColor: 'green' }} />
    </RotationGestureHandler>
  );
};

export default App;
```

- **`rotation`**: Provides the rotation angle in radians.

---

#### 3.7 **PinchGestureHandler** (Pinch Gesture)
This component is used to detect pinch gestures, often used for zooming.

```javascript
import React from 'react';
import { View } from 'react-native';
import { PinchGestureHandler } from 'react-native-gesture-handler';

const App = () => {
  const onGestureEvent = (event) => {
    console.log(event.nativeEvent.scale); // Pinch scale factor
  };

  return (
    <PinchGestureHandler onGestureEvent={onGestureEvent}>
      <View style={{ width: 100, height: 100, backgroundColor: 'blue' }} />
    </PinchGestureHandler>
  );
};

export default App;
```

- **`scale`**: Provides the scale factor for the pinch gesture.

---

### 4. **Combining Gesture Handlers**

You can combine multiple gestures on a single element. For example, you can use both a `PanGestureHandler` for dragging and a `TapGestureHandler` for tapping on the same component.

```javascript
import React from 'react';
import { View } from 'react-native';
import { TapGestureHandler, PanGestureHandler } from 'react-native-gesture-handler';

const App = () => {
  const onPanGestureEvent = (event) => console.log(event.nativeEvent.translationX);
  const onTapGestureEvent = (event) => console.log('Tapped!');

  return (
    <PanGestureHandler onGestureEvent={onPanGestureEvent}>
      <TapGestureHandler onHandlerStateChange={onTapGestureEvent}>
        <View style={{ width: 100, height: 100, backgroundColor: 'orange' }} />
      </TapGestureHandler>
    </PanGestureHandler>
  );
};

export default App;
```

---

### 5. **Conclusion**

Gesture handling is a vital part of creating a responsive and interactive app. React Native Gesture Handler provides a comprehensive set of tools to work with various types of gestures, such as taps, swipes, pinches, and drags. By combining these basic gesture components, you can implement complex interactions with minimal effort and ensure your app feels intuitive and responsive.