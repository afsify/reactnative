# React Navigation Library

React Navigation is a popular library for navigating between screens in React Native applications. It provides a simple, flexible, and easy-to-use way to manage navigation within your app. It supports various types of navigation patterns, such as stack navigation, tab navigation, drawer navigation, and more.

Here’s a detailed guide on the **React Navigation** library, including its components, setup, and usage:

---

### Key Concepts

1. **Navigation Prop**: 
   The navigation prop is passed automatically to your screen components when using React Navigation. This prop allows you to navigate between screens, pass parameters, and manage navigation state.

2. **Navigator**:
   A navigator is responsible for managing the transitions between screens and handling the navigation stack. There are different types of navigators for different use cases:
   - **Stack Navigator**: Used for stack-based navigation (pushing and popping screens).
   - **Tab Navigator**: Used for tab-based navigation (bottom or top tab bars).
   - **Drawer Navigator**: Provides a sliding drawer menu that allows access to different screens.
   - **Switch Navigator**: Used for managing authentication flows or conditional screen navigation.
   
3. **Screen**:
   Screens are the actual components that will be rendered inside the navigators. Each screen corresponds to a specific route in the app.

4. **Route**:
   A route represents a unique destination within the app, usually linked to a screen.

5. **Navigation Actions**:
   Actions allow you to navigate programmatically. You can push, pop, replace, or reset screens. Actions can be triggered using the `navigation` prop.

---

### Installation

To get started with React Navigation, follow these steps:

#### 1. Install Core Dependencies

```bash
npm install @react-navigation/native
```

#### 2. Install Dependencies for React Native

React Navigation relies on **react-native-screens** and **react-native-safe-area-context** for better performance and safety when using gestures. Install them as well:

```bash
npm install react-native-screens react-native-safe-area-context
```

#### 3. Install Stack Navigator (Example)

For stack-based navigation, install the stack navigator:

```bash
npm install @react-navigation/stack
```

For tab or drawer navigators, you can install the appropriate dependencies (e.g., `@react-navigation/bottom-tabs` for bottom tabs).

#### 4. Linking Native Code (if required)
For some older versions of React Native, you may need to link native dependencies using `react-native link`. Newer versions of React Native (0.60 and higher) support auto-linking, so this step may not be necessary.

---

### Basic Setup and Usage

1. **Importing React Navigation Libraries**

```javascript
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
```

2. **Creating a Stack Navigator**

Create a stack navigator to manage screen transitions.

```javascript
const Stack = createStackNavigator();

const App = () => {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="Home">
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Details" component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
};
```

In this example:
- `NavigationContainer` is used to wrap the entire navigation structure.
- `createStackNavigator` is used to create a stack-based navigator.
- `Stack.Screen` defines individual screens inside the navigator.

---

### Navigating Between Screens

You can navigate between screens by calling `navigation.navigate()`, `navigation.push()`, `navigation.goBack()`, or `navigation.replace()` depending on your needs.

1. **Navigate to Another Screen**

```javascript
const HomeScreen = ({ navigation }) => (
  <View>
    <Text onPress={() => navigation.navigate('Details')}>Go to Details</Text>
  </View>
);
```

2. **Go Back to the Previous Screen**

```javascript
const DetailsScreen = ({ navigation }) => (
  <View>
    <Text onPress={() => navigation.goBack()}>Go Back</Text>
  </View>
);
```

3. **Push a New Screen onto the Stack**

If you want to navigate to a screen multiple times, you can use the `navigation.push()` method.

```javascript
navigation.push('Details');
```

4. **Replace a Screen in the Stack**

```javascript
navigation.replace('Details');
```

This method will remove the current screen and replace it with a new one.

---

### Passing Parameters Between Screens

You can pass data between screens using the `navigation` prop.

1. **Passing Parameters to a Screen**

```javascript
navigation.navigate('Details', { itemId: 86, otherParam: 'anything' });
```

2. **Accessing Parameters in the Destination Screen**

```javascript
const DetailsScreen = ({ route }) => {
  const { itemId, otherParam } = route.params;
  return (
    <View>
      <Text>Item ID: {itemId}</Text>
      <Text>Other Param: {otherParam}</Text>
    </View>
  );
};
```

The `route` prop is used to access parameters passed to the screen.

---

### Types of Navigators

#### 1. Stack Navigator

A **Stack Navigator** allows users to transition between screens in a stack-based manner (like pushing and popping screens from the stack).

```javascript
import { createStackNavigator } from '@react-navigation/stack';

const Stack = createStackNavigator();

const MyStack = () => (
  <Stack.Navigator>
    <Stack.Screen name="Home" component={HomeScreen} />
    <Stack.Screen name="Profile" component={ProfileScreen} />
  </Stack.Navigator>
);
```

#### 2. Tab Navigator

A **Tab Navigator** is used to create a bottom or top tab navigation.

```javascript
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';

const Tab = createBottomTabNavigator();

const MyTabs = () => (
  <Tab.Navigator>
    <Tab.Screen name="Home" component={HomeScreen} />
    <Tab.Screen name="Settings" component={SettingsScreen} />
  </Tab.Navigator>
);
```

#### 3. Drawer Navigator

A **Drawer Navigator** allows users to access different parts of the app via a sliding drawer menu.

```javascript
import { createDrawerNavigator } from '@react-navigation/drawer';

const Drawer = createDrawerNavigator();

const MyDrawer = () => (
  <Drawer.Navigator>
    <Drawer.Screen name="Home" component={HomeScreen} />
    <Drawer.Screen name="Settings" component={SettingsScreen} />
  </Drawer.Navigator>
);
```

#### 4. Switch Navigator

The **Switch Navigator** is typically used for authentication flows, where users are either shown a login screen or the main app, depending on certain conditions.

```javascript
import { createSwitchNavigator } from '@react-navigation/core';

const Switch = createSwitchNavigator();

const AppSwitch = () => (
  <Switch.Navigator>
    <Switch.Screen name="Login" component={LoginScreen} />
    <Switch.Screen name="Home" component={HomeScreen} />
  </Switch.Navigator>
);
```

---

### Customizing Navigation

1. **Custom Header**: You can customize the header of a screen using the `options` prop in the `Stack.Screen` component.

```javascript
<Stack.Screen
  name="Home"
  component={HomeScreen}
  options={{
    headerTitle: 'Welcome',
    headerStyle: {
      backgroundColor: '#f4511e',
    },
    headerTintColor: '#fff',
  }}
/>
```

2. **Custom Tab Bar**: You can customize the tab bar in `createBottomTabNavigator` using the `tabBarOptions` or `tabBar` prop.

```javascript
<Tab.Navigator
  screenOptions={{
    tabBarActiveTintColor: 'tomato',
    tabBarInactiveTintColor: 'gray',
  }}
>
  <Tab.Screen name="Home" component={HomeScreen} />
</Tab.Navigator>
```

---

### Conclusion

React Navigation provides a powerful and flexible way to navigate between screens in React Native apps. By using navigators like stack, tab, drawer, and switch, you can implement different types of navigation patterns and transitions. It also allows you to pass parameters, control screen transitions, and customize headers and tab bars to fit your app’s design needs.

When using React Navigation, it's important to carefully plan your app's navigation structure, keeping scalability, maintainability, and performance in mind.