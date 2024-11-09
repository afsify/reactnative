# Notes on **Error Handling** in React Native

Error handling is a critical part of building robust React Native applications. It allows developers to manage and recover from unexpected issues during runtime, ensuring that the app remains stable and provides a good user experience. In React Native, error handling involves managing JavaScript errors, network errors, and UI/UX issues that may arise.

---

### 1. **Types of Errors in React Native**

#### a. **JavaScript Errors**
- Syntax errors: Misspellings, wrong brackets, etc.
- Runtime errors: Errors that occur during execution (e.g., accessing undefined properties, network request failures).
- Logic errors: Errors that don't throw exceptions but lead to incorrect behavior.

#### b. **Network Errors**
- These occur when fetching data from APIs, such as timeouts, server issues, or incorrect responses.

#### c. **UI Errors**
- These include issues such as unresponsive UI components, failed rendering, or UI components that fail to interact as expected.

---

### 2. **Error Boundaries in React Native**

React provides **Error Boundaries** to catch JavaScript errors anywhere in the component tree, log those errors, and display a fallback UI.

- **Error Boundaries** catch errors during rendering, lifecycle methods, and inside event handlers.
- They don't catch errors in asynchronous code (e.g., setTimeout, promises).
- Error boundaries should be implemented at high levels in your component tree to catch errors in child components.

#### How to Create an Error Boundary:

1. **Create an Error Boundary Component**:

   ```javascript
   import React, { Component } from 'react';
   import { Text, View } from 'react-native';

   class ErrorBoundary extends Component {
     constructor(props) {
       super(props);
       this.state = { hasError: false };
     }

     static getDerivedStateFromError(error) {
       // Update state so the next render will show the fallback UI
       return { hasError: true };
     }

     componentDidCatch(error, info) {
       // You can log error information to an external service
       console.error('Error captured:', error, info);
     }

     render() {
       if (this.state.hasError) {
         return (
           <View>
             <Text>Something went wrong. Please try again later.</Text>
           </View>
         );
       }
       return this.props.children;
     }
   }

   export default ErrorBoundary;
   ```

2. **Wrap your components with ErrorBoundary**:

   ```javascript
   import React from 'react';
   import { View, Text } from 'react-native';
   import ErrorBoundary from './ErrorBoundary';

   const App = () => {
     return (
       <ErrorBoundary>
         <View>
           <Text>Hello World!</Text>
         </View>
       </ErrorBoundary>
     );
   };

   export default App;
   ```

#### Notes:
- **`getDerivedStateFromError`**: This method is used to render a fallback UI after an error has been thrown.
- **`componentDidCatch`**: It allows you to log error details to an external service for monitoring purposes.
- **Fallback UI**: You can display a custom fallback UI, like a message or error page, when an error is caught.

---

### 3. **Handling Async Errors**

In React Native, errors related to asynchronous operations (such as network requests) can be handled using `try/catch` blocks or promise error handling techniques like `.catch()`.

#### a. **Async/Await with Try/Catch**

When using `async/await` to handle asynchronous functions, you can use `try/catch` blocks to catch errors.

Example:
```javascript
import React, { useState } from 'react';
import { View, Text, Button } from 'react-native';

const fetchData = async () => {
  try {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    return data;
  } catch (error) {
    throw new Error('Failed to fetch data');
  }
};

const App = () => {
  const [error, setError] = useState(null);

  const handleFetch = async () => {
    try {
      await fetchData();
    } catch (err) {
      setError(err.message);
    }
  };

  return (
    <View>
      {error && <Text>Error: {error}</Text>}
      <Button title="Fetch Data" onPress={handleFetch} />
    </View>
  );
};

export default App;
```

#### b. **Promise with `.catch()`**

You can also use `.catch()` to handle errors in promises.

Example:
```javascript
const fetchData = () => {
  fetch('https://api.example.com/data')
    .then(response => response.json())
    .catch(error => console.error('Error fetching data:', error));
};
```

---

### 4. **Handling Network Errors**

Network errors are common when making requests to external APIs or services. React Native provides error handling mechanisms that can help you handle timeouts, server errors, and failed requests.

#### Example of Handling Network Errors:

```javascript
import React, { useState } from 'react';
import { View, Text, Button } from 'react-native';

const App = () => {
  const [error, setError] = useState('');

  const fetchData = async () => {
    try {
      const response = await fetch('https://api.example.com/data');
      if (!response.ok) {
        throw new Error('Network response was not ok');
      }
      const data = await response.json();
      console.log(data);
    } catch (error) {
      setError('Failed to fetch data: ' + error.message);
    }
  };

  return (
    <View>
      {error ? <Text>{error}</Text> : null}
      <Button title="Fetch Data" onPress={fetchData} />
    </View>
  );
};

export default App;
```

**Notes:**
- **Network response check**: `response.ok` helps to check if the response status is successful (200-299).
- **Error Handling**: If the status code is outside the success range or the request fails, an error will be thrown.

---

### 5. **Global Error Handling**

For critical errors that cannot be caught by error boundaries, such as unhandled promise rejections or global errors, React Native provides a global error handler.

#### a. **Global Error Handler with `ErrorUtils`**

React Native provides an API for global error handling: `ErrorUtils`.

```javascript
ErrorUtils.setGlobalHandler((error, isFatal) => {
  console.log('Global error:', error);
  console.log('Is fatal:', isFatal);
});
```

- **`error`**: The error object containing information about the error.
- **`isFatal`**: A boolean indicating if the error is fatal (causing the app to crash).

#### b. **Global Unhandled Promise Rejection Handler**

To catch unhandled promise rejections, use `process.on`:

```javascript
process.on('unhandledRejection', (error, promise) => {
  console.log('Unhandled Rejection:', error);
});
```

---

### 6. **Error Logging and Monitoring**

For production applications, logging and monitoring errors is essential. You can integrate error reporting tools such as:

- **Sentry**: A popular tool for error tracking and performance monitoring.
- **Firebase Crashlytics**: A service for logging errors and crashes in mobile apps.

#### Example with Sentry:

1. Install Sentry SDK:
   ```bash
   npm install @sentry/react-native
   ```

2. Configure Sentry:
   ```javascript
   import * as Sentry from '@sentry/react-native';

   Sentry.init({ dsn: 'your-dsn-here' });
   ```

3. Capture errors manually:
   ```javascript
   try {
     // Your code
   } catch (error) {
     Sentry.captureException(error);
   }
   ```

---

### 7. **Best Practices for Error Handling**

- **Graceful Error Handling**: Provide fallback UI elements (e.g., loading indicators, error messages) when errors occur.
- **Catch Specific Errors**: Catch specific types of errors (network errors, input validation errors) to provide more meaningful feedback.
- **Use Error Boundaries**: Wrap your components with error boundaries to catch JavaScript errors.
- **Log Errors**: Use logging services like Sentry, Firebase, or custom loggers to monitor production errors.
- **Avoid Silent Failures**: Always handle errors properly and avoid scenarios where errors are ignored silently.

---

### 8. **Conclusion**

Effective error handling in React Native helps ensure that your app remains robust, stable, and user-friendly. By using tools like Error Boundaries, `try/catch` for async functions, network error handling, and integrating error logging tools, you can manage and mitigate errors in your application.