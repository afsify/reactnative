# Passing Parameters Between Screens in React Native

React Navigation is a powerful routing and navigation library that allows you to pass parameters between screens easily. Parameters are passed from one screen to another through **navigation props**.

---

### Key Concepts

1. **React Navigation**:
   - React Navigation is a popular library used for managing the navigation in React Native apps.
   - It allows you to move between screens and pass data (parameters) to different screens.

2. **Types of Navigators**:
   - **Stack Navigator**: This is the most common type, where each screen is placed on top of a stack. The most recently visited screen is the top one.
   - **Tab Navigator**: Used for bottom or top tab navigation between screens.
   - **Drawer Navigator**: For side navigation drawers.

   You can pass parameters through these navigators.

---

### Steps for Passing Parameters Between Screens

1. **Setting up React Navigation**
   - Install necessary dependencies:
     ```bash
     npm install @react-navigation/native @react-navigation/stack react-native-screens react-native-safe-area-context
     ```
   - If you're using expo, you can also run:
     ```bash
     expo install react-native-gesture-handler react-native-reanimated
     ```

2. **Create Stack Navigator**
   - Import necessary modules:
     ```javascript
     import * as React from 'react';
     import { Button, View, Text } from 'react-native';
     import { NavigationContainer } from '@react-navigation/native';
     import { createStackNavigator } from '@react-navigation/stack';
     ```

   - Define Stack Navigator:
     ```javascript
     const Stack = createStackNavigator();
     ```

---

### Passing Parameters from One Screen to Another

1. **Passing Parameters to the Next Screen**

   - **Passing parameters when navigating**:
     You can pass parameters when navigating to another screen by passing a second argument in the `navigate` function.

     Example:
     ```javascript
     function HomeScreen({ navigation }) {
       return (
         <View>
           <Button
             title="Go to Details"
             onPress={() => {
               // Passing parameters while navigating
               navigation.navigate('Details', {
                 itemId: 86,
                 otherParam: 'Anything you want here',
               });
             }}
           />
         </View>
       );
     }
     ```

2. **Receiving Parameters on the Target Screen**

   On the target screen, you can use the `route` prop to access the parameters.

   Example:
   ```javascript
   function DetailsScreen({ route }) {
     // Accessing the parameters passed to this screen
     const { itemId, otherParam } = route.params;

     return (
       <View>
         <Text>Item ID: {itemId}</Text>
         <Text>Other Param: {otherParam}</Text>
       </View>
     );
   }
   ```

---

### Example Code for Passing Parameters Between Screens

```javascript
import * as React from 'react';
import { Button, View, Text } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';

// Define the Stack Navigator
const Stack = createStackNavigator();

// Home Screen Component
function HomeScreen({ navigation }) {
  return (
    <View>
      <Button
        title="Go to Details"
        onPress={() => {
          // Passing parameters to the next screen
          navigation.navigate('Details', {
            itemId: 42,
            otherParam: 'Hello from Home Screen!',
          });
        }}
      />
    </View>
  );
}

// Details Screen Component
function DetailsScreen({ route }) {
  const { itemId, otherParam } = route.params;

  return (
    <View>
      <Text>Item ID: {itemId}</Text>
      <Text>Other Param: {otherParam}</Text>
    </View>
  );
}

// App Component with Navigation
export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="Home">
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Details" component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

### Explanation:
- **HomeScreen**: When the button is pressed, we call `navigation.navigate` with the screen name `'Details'` and an object containing the parameters (`itemId` and `otherParam`).
- **DetailsScreen**: We receive the parameters through the `route` prop using `route.params`.

---

### Using the `setParams` Method

You can also update the parameters of the current screen dynamically using the `setParams` method.

Example:
```javascript
function DetailsScreen({ route, navigation }) {
  // Access initial params
  const { itemId, otherParam } = route.params;

  // Update params dynamically
  React.useEffect(() => {
    navigation.setParams({ otherParam: 'Updated Parameter' });
  }, []);

  return (
    <View>
      <Text>Item ID: {itemId}</Text>
      <Text>Other Param: {route.params.otherParam}</Text>
    </View>
  );
}
```

---

### Passing Parameters in Deep Links

You can also pass parameters through deep links, enabling users to open your app directly on a specific screen with parameters.

Example:
```javascript
// Link format: myapp://details?itemId=42&otherParam=hello
```

---

### Notes:

- **Navigation Prop**: The `navigation` prop is used to control the navigation, including passing parameters, and the `route` prop is used to access the parameters on the target screen.
- **`route.params`**: This is where parameters passed to a screen are stored.
- **Updating Parameters**: The `setParams` method allows you to dynamically update the parameters of the current screen.

---

### Conclusion

Passing parameters between screens in React Native is simple using React Navigation. You can use `navigation.navigate()` to pass data and access it using `route.params` on the target screen. For dynamic updates, `setParams` is useful, and deep linking also supports parameter passing to specific screens directly from a URL.

