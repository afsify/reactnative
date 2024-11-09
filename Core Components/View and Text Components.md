# React Native `View` Component

The `View` component is one of the most fundamental building blocks in React Native. It acts as a container for other UI elements and helps structure layouts.

#### Key Properties of `View`

1. **`style`**: Allows you to style the `View` component. You can use styles for layout, dimensions, background color, alignment, etc.
   - Example: `{ backgroundColor: 'blue', padding: 10 }`

2. **`accessibilityLabel`**: Provides a label for assistive technologies like screen readers.

3. **`accessible`**: A boolean that, when set to `true`, allows the `View` to be accessible to screen readers.

4. **`onLayout`**: Callback function that’s triggered when the layout of the `View` changes. Useful for measuring and responding to layout changes.

5. **`pointerEvents`**: Controls how touch interactions are handled (e.g., `"box-none"`, `"none"`, `"box-only"`, `"auto"`).

6. **`testID`**: Useful for identifying components during automated testing.

#### Example Usage of `View`

```javascript
import React from 'react';
import { View, StyleSheet } from 'react-native';

const ExampleView = () => {
  return (
    <View style={styles.container}>
      <View style={styles.box}></View>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  box: {
    width: 100,
    height: 100,
    backgroundColor: 'blue',
  },
});

export default ExampleView;
```

In this example:
- `View` serves as a container with `flex: 1`, centering any nested items.
- An inner `View` styled as a blue box is positioned at the center.

#### Common Use Cases for `View`

- **Containers**: Wrapping components in a structured layout.
- **Layouts**: Arranging components using `flex` properties.
- **Styling**: Using background colors, padding, and borders to create visually distinct sections.

---

### React Native `Text` Component

The `Text` component is used to display text in the app. It can hold plain text or styled text and can be nested within `View` components or other `Text` components.

#### Key Properties of `Text`

1. **`style`**: Allows you to style the text (e.g., font size, color, alignment).
   - Example: `{ fontSize: 16, color: 'black', textAlign: 'center' }`

2. **`numberOfLines`**: Limits the number of lines displayed. Useful for truncating long text.
   - Example: `numberOfLines={1}` truncates text to a single line with ellipsis.

3. **`ellipsizeMode`**: Specifies the position of ellipsis when text is truncated (`'head'`, `'middle'`, `'tail''`, `'clip'`).

4. **`onPress`**: Function triggered when the text is pressed, useful for creating tappable text.

5. **`selectable`**: A boolean that allows the text to be selectable when `true`.

6. **`accessibilityRole`**: Sets the accessibility role, e.g., `"header"`, `"button"`, for screen readers.

#### Example Usage of `Text`

```javascript
import React from 'react';
import { Text, View, StyleSheet } from 'react-native';

const ExampleText = () => {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Hello, React Native!</Text>
      <Text style={styles.subtitle} numberOfLines={1}>
        This is a simple text example that demonstrates styling and truncation.
      </Text>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    padding: 20,
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    color: 'blue',
  },
  subtitle: {
    fontSize: 16,
    color: 'grey',
  },
});

export default ExampleText;
```

In this example:
- `Text` components display different levels of text (`title` and `subtitle`) with distinct styles.
- `numberOfLines={1}` ensures the subtitle text is truncated if it exceeds one line.

#### Common Use Cases for `Text`

- **Headers and Labels**: Using text to define headings, titles, and labels.
- **Descriptions**: Providing content or explanations within the app.
- **Touchable Text**: Making text elements tappable by using the `onPress` property.

---

### Example Combining `View` and `Text`

```javascript
import React from 'react';
import { View, Text, StyleSheet } from 'react-native';

const ProfileScreen = () => {
  return (
    <View style={styles.container}>
      <Text style={styles.heading}>Profile</Text>
      <Text style={styles.description}>
        Welcome to your profile! Here, you can see your details.
      </Text>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 16,
    backgroundColor: '#f0f0f0',
  },
  heading: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#333',
  },
  description: {
    fontSize: 16,
    color: '#666',
    textAlign: 'center',
    marginTop: 8,
  },
});

export default ProfileScreen;
```

This example shows:
- A `View` container with centered alignment and padding.
- A heading and description using `Text`, styled distinctly.

---

### Summary

- **`View`** is a flexible container that helps in layout management and structuring components.
- **`Text`** is used to display and format text content.
- Both components accept `style` properties for customization, with `View` focused on layout and `Text` on typography.
  
Together, `View` and `Text` form the foundation of any React Native interface, enabling layout structuring and text presentation within mobile applications.