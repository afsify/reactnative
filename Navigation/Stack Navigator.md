# Stack Navigator in React Navigation

The **Stack Navigator** is one of the most fundamental navigation patterns in React Navigation, used to navigate between screens in a stack-like manner. When a user navigates to a new screen, it’s pushed onto the stack, and when they navigate back, the top screen is popped off the stack.

#### Key Concepts of Stack Navigator:
1. **Stack**: A stack is a data structure where you can push and pop items. In terms of navigation, screens are stacked on top of each other as users navigate forward.
2. **Push**: Pushing a new screen onto the stack when navigating to a new screen.
3. **Pop**: Popping a screen off the stack when navigating back.
4. **Navigation**: The Stack Navigator handles the flow of users through multiple screens in a hierarchical manner.

---

### Setting Up Stack Navigator

To use the Stack Navigator, you need to install `@react-navigation/native` and `@react-navigation/stack` along with the necessary dependencies like `react-native-gesture-handler` and `react-native-reanimated`.

#### 1. Install dependencies:
```bash
npm install @react-navigation/native @react-navigation/stack
npm install react-native-gesture-handler react-native-reanimated
```

For iOS, run the following:
```bash
cd ios && pod install && cd ..
```

#### 2. Setup Navigation Container:
First, import the `NavigationContainer` and `createStackNavigator` from React Navigation.

```javascript
import * as React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
import { View, Text } from 'react-native';

// Define your screens
function HomeScreen({ navigation }) {
  return (
    <View>
      <Text>Home Screen</Text>
      <Button title="Go to Details" onPress={() => navigation.navigate('Details')} />
    </View>
  );
}

function DetailsScreen() {
  return (
    <View>
      <Text>Details Screen</Text>
    </View>
  );
}

// Create the Stack Navigator
const Stack = createStackNavigator();

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

**Explanation**:
- **`NavigationContainer`**: This is a container for the entire navigation structure, which keeps track of the navigation state.
- **`createStackNavigator`**: Creates a stack navigator which allows pushing and popping of screens.
- **`initialRouteName`**: Specifies the initial screen when the app loads (Home screen in this case).
- **`Stack.Screen`**: Represents each screen in the stack. You define the `name` and `component` (screen component).

---

### Navigating Between Screens

In a stack navigator, screens can be navigated to using the `navigation` prop, which provides methods like `navigate`, `goBack`, `push`, and `pop`.

#### 1. **Navigate to another screen**:
```javascript
navigation.navigate('Details');
```
- Navigates to the screen specified by its name (`Details` in this case).
  
#### 2. **Go Back**:
```javascript
navigation.goBack();
```
- This method pops the current screen off the stack, taking the user back to the previous screen.

#### 3. **Push a new screen onto the stack**:
```javascript
navigation.push('Details');
```
- This is used to navigate to a screen that is already in the stack, but it creates a new instance of that screen.

---

### Stack Navigator Options

You can customize the Stack Navigator's behavior and appearance using screen options:

#### 1. **Screen Header Options**:
You can change the header for individual screens or the entire navigator.

- **Setting title for a screen**:
```javascript
<Stack.Screen
  name="Home"
  component={HomeScreen}
  options={{ title: 'Welcome Home' }}
/>
```

- **Customizing the header**:
```javascript
<Stack.Screen
  name="Home"
  component={HomeScreen}
  options={{
    headerStyle: { backgroundColor: 'blue' },
    headerTintColor: '#fff',
    headerTitleStyle: { fontWeight: 'bold' },
  }}
/>
```

#### 2. **Header Buttons**:
You can add buttons like a back button or a custom button to the header.

- **Adding a custom button to the header**:
```javascript
<Stack.Screen
  name="Home"
  component={HomeScreen}
  options={({ navigation }) => ({
    headerRight: () => (
      <Button title="Info" onPress={() => alert('This is a button!')} />
    ),
  })}
/>
```

#### 3. **Transition Animations**:
You can also specify custom transition animations when navigating between screens.

```javascript
<Stack.Navigator
  screenOptions={{
    gestureEnabled: true,
    transitionSpec: {
      open: { animation: 'timing', config: { duration: 500 } },
      close: { animation: 'timing', config: { duration: 300 } },
    },
  }}
>
  <Stack.Screen name="Home" component={HomeScreen} />
  <Stack.Screen name="Details" component={DetailsScreen} />
</Stack.Navigator>
```

---

### Deep Linking with Stack Navigator

Deep linking allows you to navigate to specific screens in your app from an external URL. You can configure deep linking in your Stack Navigator.

#### Example:
```javascript
<NavigationContainer
  linking={{
    prefixes: ['myapp://'],
    config: {
      screens: {
        Home: '',
        Details: 'details',
      },
    },
  }}
>
  <Stack.Navigator>
    <Stack.Screen name="Home" component={HomeScreen} />
    <Stack.Screen name="Details" component={DetailsScreen} />
  </Stack.Navigator>
</NavigationContainer>
```

- Here, a URL like `myapp://details` would take you directly to the `Details` screen.

---

### Stack Navigator with Nested Navigators

You can combine different types of navigators (e.g., Stack, Tab, Drawer) in your app. For example, you can have a stack navigator inside a tab navigator.

```javascript
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import { createStackNavigator } from '@react-navigation/stack';

const Tab = createBottomTabNavigator();
const Stack = createStackNavigator();

function HomeScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen name="Home" component={HomeScreenComponent} />
      <Stack.Screen name="Details" component={DetailsScreen} />
    </Stack.Navigator>
  );
}

function App() {
  return (
    <NavigationContainer>
      <Tab.Navigator>
        <Tab.Screen name="Home" component={HomeScreen} />
        <Tab.Screen name="Settings" component={SettingsScreen} />
      </Tab.Navigator>
    </NavigationContainer>
  );
}
```

- Here, the `HomeScreen` contains a Stack Navigator with `Home` and `Details` screens, while the `Settings` screen is a separate tab.

---

### Handling Parameters in Stack Navigator

You can pass parameters to screens using `navigation.navigate()` and access them with `route.params`.

#### 1. **Passing Parameters**:
```javascript
navigation.navigate('Details', { itemId: 42, otherParam: 'anything' });
```

#### 2. **Accessing Parameters**:
```javascript
function DetailsScreen({ route }) {
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

### Conclusion

The Stack Navigator is one of the most commonly used navigation patterns in React Navigation, and it allows developers to navigate between screens in a stack-based manner. It provides powerful features like screen transitions, passing parameters, customizing headers, and handling deep linking. By understanding how to configure and use Stack Navigator, you can build highly functional navigation flows for your React Native apps.