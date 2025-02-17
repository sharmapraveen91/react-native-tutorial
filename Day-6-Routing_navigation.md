# Navigation & Routing in React Native App Development

Navigation is an essential part of mobile app development. In React Native, navigation refers to how users move between different screens of the application. In this document, we'll cover React Navigation libraries for stack, tab, and drawer navigation, as well as deep linking and dynamic routes.

---

## Table of Contents

1. [React Navigation Basics](#react-navigation-basics)
2. [Stack Navigation](#stack-navigation)
3. [Tab Navigation](#tab-navigation)
4. [Drawer Navigation](#drawer-navigation)
5. [Deep Linking in React Native](#deep-linking-in-react-native)
6. [Dynamic Routes](#dynamic-routes)
7. [Do's Don'ts and Best Practices](#dos-donts-and-best-practices)
8. [Interview Questions](#interview-questions-on-navigation--routing-in-react-native)

---

## React Navigation Basics

### Installation

To get started with navigation in React Native, the primary library used is `react-navigation`. Install the necessary dependencies by running:

```bash
npm install @react-navigation/native
npm install @react-navigation/stack @react-navigation/bottom-tabs @react-navigation/drawer
npm install react-native-screens react-native-safe-area-context
npm install react-native-gesture-handler react-native-reanimated
```
Additionally, you need to install dependencies for linking:
```bash
npx pod-install ios
```

## Stack Navigation
Stack Navigation is used for navigating through a stack of screens in a sequential order, typically used for moving between different "pages" or views.

### Setting up Stack Navigator
First, import the necessary components:

```javascript
import * as React from 'react';
import { Button, Text, View } from 'react-native';
import { createStackNavigator } from '@react-navigation/stack';
import { NavigationContainer } from '@react-navigation/native';

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

In the above example:

**HomeScreen** has a button that navigates to **DetailsScreen**.
We use the `createStackNavigator` to define our stack and set the initial route to "Home".

### Tab Navigation
Tab Navigation is used for creating a tab-based navigation, usually used at the bottom of the screen to switch between different sections of the app.

### Setting up Tab Navigator

```javascript
import * as React from 'react';
import { Text, View } from 'react-native';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import { NavigationContainer } from '@react-navigation/native';

function HomeScreen() {
  return (
    <View>
      <Text>Home Screen</Text>
    </View>
  );
}

function SettingsScreen() {
  return (
    <View>
      <Text>Settings Screen</Text>
    </View>
  );
}

const Tab = createBottomTabNavigator();

export default function App() {
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

In the above example:

**HomeScreen** and **SettingsScreen** are displayed in a tab-based navigation.
The `createBottomTabNavigator` is used to define the tab navigation structure.

## Drawer Navigation
Drawer Navigation is used for creating a sliding drawer menu (typically from the left or right side) to navigate between different sections of the app.

Setting up Drawer Navigator
```javascript
import * as React from 'react';
import { Text, View } from 'react-native';
import { createDrawerNavigator } from '@react-navigation/drawer';
import { NavigationContainer } from '@react-navigation/native';

function HomeScreen() {
  return (
    <View>
      <Text>Home Screen</Text>
    </View>
  );
}

function SettingsScreen() {
  return (
    <View>
      <Text>Settings Screen</Text>
    </View>
  );
}

const Drawer = createDrawerNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Drawer.Navigator initialRouteName="Home">
        <Drawer.Screen name="Home" component={HomeScreen} />
        <Drawer.Screen name="Settings" component={SettingsScreen} />
      </Drawer.Navigator>
    </NavigationContainer>
  );
}
```
In the above example:

- The app uses a drawer to navigate between HomeScreen and SettingsScreen.
- createDrawerNavigator is used to set up the drawer.

## Deep Linking in React Native

Deep linking is the process of opening a specific screen in your app by providing a URL that contains the target screen information.

### Setting up Deep Linking
1. Android Setup In android/app/src/main/AndroidManifest.xml, add an intent filter for the URL scheme.

```xml
<activity
    android:name=".MainActivity"
    android:label="@string/app_name"
    android:launchMode="singleTask"
    android:configChanges="keyboard|keyboardHidden|orientation|screenSize"
    android:theme="@style/AppTheme"
    android:windowSoftInputMode="adjustResize">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="myapp" android:host="details" />
    </intent-filter>
</activity>
```
2. **iOS Setup** In `ios/{ProjectName}/Info.plist`, add the URL scheme:

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>myapp</string>
        </array>
    </dict>
</array>
```

3. React Navigation Linking Setup
```javascript
const linking = {
  prefixes: ['myapp://'],
  config: {
    screens: {
      Home: '',
      Details: 'details',
    },
  },
};

export default function App() {
  return (
    <NavigationContainer linking={linking}>
      <Stack.Navigator initialRouteName="Home">
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Details" component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```
Here:

- The app will respond to deep links with the scheme myapp://details to navigate to the DetailsScreen.

## Dynamic Routes
Dynamic routes allow us to pass parameters to different screens to create routes based on dynamic values.

### Example of Dynamic Routes
```javascript
import * as React from 'react';
import { Button, Text, View } from 'react-native';
import { createStackNavigator } from '@react-navigation/stack';
import { NavigationContainer } from '@react-navigation/native';

function HomeScreen({ navigation }) {
  return (
    <View>
      <Text>Home Screen</Text>
      <Button
        title="Go to Details"
        onPress={() => navigation.navigate('Details', { userId: 42 })}
      />
    </View>
  );
}

function DetailsScreen({ route }) {
  const { userId } = route.params;
  return (
    <View>
      <Text>Details Screen - User ID: {userId}</Text>
    </View>
  );
}

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

In the above code:

- The HomeScreen sends a dynamic value userId as a parameter to the DetailsScreen.
- The DetailsScreen receives this value using route.params and displays it.
## Conclusion
In this study material, we covered the essential topics related to navigation and routing in React Native:

1. React Navigation with Stack, Tab, and Drawer navigators.
2. Deep Linking to open specific screens from URLs.
3. Dynamic Routes to pass parameters between screens.


Navigation is a core part of any mobile app, and with the help of react-navigation, you can easily implement various types of navigation to provide users with a smooth experience.

---
## Do's Don'ts and Best Practices

# Do's, Don'ts, and Best Practices for Navigation & Routing in React Native App Development

## Do's

1. **Use the right navigator for the right use case:**
   - Use **Stack Navigator** for flows that go from one screen to another.
   - Use **Tab Navigator** for displaying multiple major sections of the app.
   - Use **Drawer Navigator** for global or more complex navigation menus.

2. **Make navigation declarative:**
   - Always try to define your navigation structure clearly using `NavigationContainer`, `Stack.Navigator`, `Tab.Navigator`, etc., at the top level of the app.

3. **Handle back button behavior:**
   - On Android, use `BackHandler` to override the default back button behavior when necessary (e.g., to prevent accidental exits or to implement custom flows).

4. **Use `useNavigation` hook for deep linking or navigating programmatically:**
   - `useNavigation` is a powerful hook for programmatically navigating between screens without the need to pass the `navigation` prop explicitly.

5. **Pass parameters carefully between screens:**
   - Use `navigation.navigate()` to pass parameters to the target screen. Ensure you check for parameters in the target screen using `route.params`.

6. **Implement deep linking correctly:**
   - Configure deep linking properly for both iOS and Android to handle URLs that can navigate to specific screens in the app.

7. **Use the `linking` prop in React Navigation for deep linking:**
   - The `linking` prop is crucial for handling incoming URLs, ensuring that deep linking is set up and routes are properly mapped.

8. **Use navigation guards (e.g., authentication flow):**
   - Manage app state like authentication to display the correct screen using conditional rendering. For example, show the login screen if the user is not authenticated.

9. **Handle nested navigators effectively:**
   - Nest navigators like `TabNavigator` inside `StackNavigator` or `DrawerNavigator` when you need to handle complex navigation flows.

10. **Test navigation thoroughly:**
    - Ensure your navigation structure is tested to handle transitions, edge cases, and deep linking for seamless user experience.

## Don'ts

1. **Avoid over-complicating navigation logic:**
   - Don’t try to build overly complex navigation flows. Use simpler, more intuitive navigation patterns wherever possible.

2. **Don't forget to handle the back button for Android:**
   - On Android, navigation actions on back press may not work as expected without proper handling (e.g., `BackHandler`). Avoid relying solely on default behavior.

3. **Don't use navigation libraries that are no longer maintained:**
   - Avoid using outdated or deprecated libraries. Stick to well-supported libraries like **React Navigation** and **React Native Navigation**.

4. **Don't navigate without checking for the required parameters:**
   - Ensure that parameters are passed correctly. Avoid assuming that parameters are always available in the `route` object.

5. **Avoid using multiple navigation containers in the app:**
   - Ensure only one `NavigationContainer` is present at the root of the app. Having multiple navigation containers can cause issues with navigation state.

6. **Don't ignore screen transitions and animation:**
   - While transitions can improve the user experience, avoid excessive or unnecessary animations that may negatively impact performance.

7. **Don't hardcode deep linking paths:**
   - Avoid hardcoding deep linking paths inside the app. Use dynamic routing and make sure your URLs are flexible and scalable.

8. **Don’t manually manage navigation state outside of React Navigation:**
   - Let React Navigation handle the navigation state. Manually managing navigation state can create conflicts and bugs.

9. **Avoid using too many modals and overlays:**
   - Limit the use of modals or overlays that interrupt the navigation flow. Excessive overlays can frustrate users by breaking the flow of navigation.

10. **Don't forget to handle navigation lifecycle events:**
    - Don’t skip lifecycle methods like `focus`, `blur`, `beforeRemove`, and `state` when handling navigation transitions or state changes. Use them to manage resources (like API calls or clean-up actions) effectively.

## Best Practices

1. **Maintain a consistent navigation pattern:**
   - Stick to consistent patterns (e.g., stack for flow navigation, tabs for major sections, drawers for global navigation) to ensure a cohesive and intuitive user experience.

2. **Use header customization effectively:**
   - Customize headers globally or per screen to provide better UX. Use `headerTitle`, `headerRight`, and `headerLeft` for buttons, titles, and other header elements.

3. **Leverage code splitting for large apps:**
   - For larger apps with complex navigation, consider using dynamic imports or lazy loading of screens to reduce initial load time.

4. **Use lazy-loading for deep-linked screens:**
   - Implement lazy loading for screens that can be deep-linked into. This can improve app performance and reduce unnecessary component rendering.

5. **Organize screens and navigation files effectively:**
   - Structure your app by separating navigation logic and screens into different folders to improve readability and maintainability.

6. **Consider accessibility when implementing navigation:**
   - Ensure navigation is accessible to all users. This includes implementing proper focus management, ensuring screen readers can access navigation elements, and supporting gestures.

7. **Test navigation flows on multiple devices:**
   - Always test navigation on both iOS and Android devices to ensure consistent behavior. React Navigation may behave differently depending on platform.

8. **Use `navigation.setOptions` for dynamic header customization:**
   - If you need to change header options based on dynamic data, use `navigation.setOptions` to adjust options during runtime.

9. **Use React Navigation's built-in navigation guards for authentication:**
   - Use `navigation.addListener` for handling navigation lifecycle events like `focus` and `blur` to implement features like authentication and re-authentication.

10. **Ensure a fallback screen for invalid deep linking:**
    - Always ensure that there is a fallback screen or error handling in case a deep link is invalid or the user navigates to a non-existent route.

---

# Interview Questions on Navigation & Routing in React Native

## Table of Contents

1. [What is React Navigation?](#what-is-react-navigation)
2. [What are the different types of navigators in React Navigation?](#what-are-the-different-types-of-navigators-in-react-navigation)
3. [How does Stack Navigator work in React Native?](#how-does-stack-navigator-work-in-react-native)
4. [What is Tab Navigation, and when would you use it?](#what-is-tab-navigation-and-when-would-you-use-it)
5. [What is Drawer Navigation in React Native?](#what-is-drawer-navigation-in-react-native)
6. [Explain the difference between Stack, Tab, and Drawer Navigators.](#explain-the-difference-between-stack-tab-and-drawer-navigators)
7. [How do you set up deep linking in a React Native app?](#how-do-you-set-up-deep-linking-in-a-react-native-app)
8. [How do you pass parameters between screens in React Navigation?](#how-do-you-pass-parameters-between-screens-in-react-navigation)
9. [What is dynamic routing in React Navigation?](#what-is-dynamic-routing-in-react-navigation)
10. [How do you handle navigation on back press in React Native?](#how-do-you-handle-navigation-on-back-press-in-react-native)
11. [What are the advantages of using `react-navigation` over other navigation libraries in React Native?](#what-are-the-advantages-of-using-react-navigation-over-other-navigation-libraries-in-react-native)
12. [What is the `NavigationContainer` component in React Navigation?](#what-is-the-navigationcontainer-component-in-react-navigation)
13. [How do you use `useNavigation` hook in React Native?](#how-do-you-use-usenavigation-hook-in-react-native)
14. [How do you configure a custom header in React Navigation?](#how-do-you-configure-a-custom-header-in-react-navigation)
15. [What is the role of `react-native-gesture-handler` and `react-native-reanimated` in React Navigation?](#what-is-the-role-of-react-native-gesture-handler-and-react-native-reanimated-in-react-navigation)
16. [How would you handle nested navigation in React Native?](#how-would-you-handle-nested-navigation-in-react-native)
17. [Can we use multiple types of navigators in a single app?](#can-we-use-multiple-types-of-navigators-in-a-single-app)
18. [How do you enable or disable screen transitions in React Navigation?](#how-do-you-enable-or-disable-screen-transitions-in-react-navigation)
19. [What is the `linking` prop in React Navigation?](#what-is-the-linking-prop-in-react-navigation)
20. [Explain how to handle authentication flow with React Navigation.](#explain-how-to-handle-authentication-flow-with-react-navigation)

---

## 1. What is React Navigation?

React Navigation is the standard library for navigation in React Native applications. It provides navigation primitives, such as stack, tab, and drawer navigation, and helps you create apps with seamless navigation patterns.

React Navigation supports features like deep linking, dynamic routing, and custom headers, making it one of the most popular navigation libraries in the React Native ecosystem.

---

## 2. What are the different types of navigators in React Navigation?

React Navigation provides several types of navigators:

- **Stack Navigator**: Used for navigating between screens in a stack. Ideal for sequential flows where you go from one screen to another (e.g., form steps, list details).
- **Tab Navigator**: Used for tab-based navigation at the bottom or top of the screen. Commonly used for apps with distinct sections like "Home", "Settings", and "Profile".
- **Drawer Navigator**: Used to create a slide-out menu that allows users to choose different sections of the app. It can be accessed from the left or right side of the screen.
- **Switch Navigator**: Typically used for authentication flows where only one screen is active at a time, and the user is either logged in or logged out.

---

## 3. How does Stack Navigator work in React Native?

The Stack Navigator allows you to navigate through a stack of screens where each screen is stacked on top of the previous one. It provides a push and pop operation to manage the stack.

### Example of Stack Navigator:

```javascript
import { createStackNavigator } from '@react-navigation/stack';

const Stack = createStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Details" component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```
### 4. What is Tab Navigation, and when would you use it?

Tab Navigation allows you to create a tab bar with multiple screens. Each screen corresponds to a tab, and the user can switch between them by tapping on the tab bar.

It is ideal for apps with distinct sections where each section represents a major part of the app, like a "Home" screen, a "Settings" screen, and a "Profile" screen.

**Example of Tab Navigation:**

```javascript
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';

const Tab = createBottomTabNavigator();

function App() {
  return (
    <NavigationContainer>
      <Tab.Navigator>
        <Tab.Screen name="Home" component={HomeScreen} />
        <Tab.Screen name="Profile" component={ProfileScreen} />
      </Tab.Navigator>
    </NavigationContainer>
  );
}
```
### 5. What is Drawer Navigation in React Native?

Drawer Navigation provides a sidebar (drawer) that can slide in from the edge of the screen. It's typically used for global navigation across the app. This is useful when the app has a large number of navigation options.

**Example of Drawer Navigation:**

```javascript
import { createDrawerNavigator } from '@react-navigation/drawer';

const Drawer = createDrawerNavigator();

function App() {
  return (
    <NavigationContainer>
      <Drawer.Navigator>
        <Drawer.Screen name="Home" component={HomeScreen} />
        <Drawer.Screen name="Settings" component={SettingsScreen} />
      </Drawer.Navigator>
    </NavigationContainer>
  );
}
```

### 6. Explain the difference between Stack, Tab, and Drawer Navigators.

- **Stack Navigator**: It is used for a linear navigation flow (e.g., moving from one screen to another). Screens are stacked on top of each other.
- **Tab Navigator**: It is used to display multiple sections in a horizontal or vertical tab-based UI. Only one tab's screen is visible at a time.
- **Drawer Navigator**: It is used for displaying a navigation menu, usually accessed by a swipe or button click. The drawer slides in from the edge of the screen.

---

### 7. How do you set up deep linking in a React Native app?

Deep linking is the process of linking directly to a specific screen in the app via URLs.

**Example setup for deep linking:**

**Android Setup** (Add the intent filter in AndroidManifest.xml):

```xml
<activity
    android:name=".MainActivity"
    android:label="@string/app_name"
    android:launchMode="singleTask">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="myapp" android:host="details" />
    </intent-filter>
</activity>
```
**iOS Setup** (Add URL scheme in Info.plist):
```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>myapp</string>
        </array>
    </dict>
</array>
```
React Navigation Linking:
```javascript

const linking = {
  prefixes: ['myapp://'],
  config: {
    screens: {
      Home: '',
      Details: 'details',
    },
  },
};

export default function App() {
  return (
    <NavigationContainer linking={linking}>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Details" component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```
### 8. How do you pass parameters between screens in React Navigation?

You can pass parameters between screens in React Navigation using the `navigation.navigate()` function. The parameters are accessible in the target screen via `route.params`.

**Example of passing parameters:**

```javascript
function HomeScreen({ navigation }) {
  return (
    <Button
      title="Go to Details"
      onPress={() => navigation.navigate('Details', { userId: 42 })}
    />
  );
}

function DetailsScreen({ route }) {
  const { userId } = route.params;
  return <Text>User ID: {userId}</Text>;
}
```
### 9. What is dynamic routing in React Navigation?

Dynamic routing refers to the ability to navigate to different screens based on conditions such as user input, data, or URLs. It allows for flexibility in routing based on application state or dynamic data.

**Example of dynamic routing:**

```javascript
function App() {
  const userLoggedIn = true; // Example condition

  return (
    <NavigationContainer>
      <Stack.Navigator>
        {userLoggedIn ? (
          <Stack.Screen name="Dashboard" component={DashboardScreen} />
        ) : (
          <Stack.Screen name="Login" component={LoginScreen} />
        )}
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

### 10. How do you handle navigation on back press in React Native?

In React Navigation, back button behavior is handled automatically. You can also use `BackHandler` from React Native to manage custom behavior when the user presses the back button.

**Example of custom back handling:**

```javascript
import { BackHandler } from 'react-native';

useEffect(() => {
  const backAction = () => {
    if (navigation.isFocused()) {
      navigation.goBack();
      return true;
    }
    return false;
  };

  const backHandler = BackHandler.addEventListener('hardwareBackPress', backAction);

  return () => backHandler.remove();
}, []);
```

### 11. What are the advantages of using react-navigation over other navigation libraries in React Native?

- **Comprehensive and Flexible**: Supports multiple navigation patterns like stack, tab, drawer, and even nested navigation.
- **Easy Setup**: It provides simple APIs for setting up navigation.
- **Customizable**: You can customize navigation headers, screen transitions, and more.
- **Community Support**: Being the most widely used navigation library in the React Native ecosystem, it has a large community and good documentation.

---

### 12. What is the NavigationContainer component in React Navigation?

`NavigationContainer` is the root component that holds the navigation state and manages the navigation tree. It must be placed at the root of the app, and all navigators are nested inside it.

```javascript
<NavigationContainer>
  <Stack.Navigator>
    <Stack.Screen name="Home" component={HomeScreen} />
  </Stack.Navigator>
</NavigationContainer>
```

### 13. How do you use the `useNavigation` hook in React Native?

The `useNavigation` hook provides access to the navigation object. It allows you to navigate programmatically without needing to pass navigation as a prop.

```javascript
import { useNavigation } from '@react-navigation/native';

function MyComponent() {
  const navigation = useNavigation();

  return (
    <Button title="Go to Details" onPress={() => navigation.navigate('Details')} />
  );
}
```
### 14. How do you configure a custom header in React Navigation?

You can customize headers globally or per screen using the `options` prop. You can set custom components or styles for the header.

```javascript
<Stack.Navigator>
  <Stack.Screen
    name="Home"
    component={HomeScreen}
    options={{
      headerTitle: 'Custom Title',
      headerRight: () => <Button title="Info" onPress={() => alert('Info')} />,
    }}
  />
</Stack.Navigator>
```
### 15. What is the role of `react-native-gesture-handler` and `react-native-reanimated` in React Navigation?

These two libraries are required for handling gestures and animations in React Navigation. They provide smooth transitions and enable features like swipe gestures and animations.

- **`react-native-gesture-handler`**: Manages touch events and gestures like swipe, tap, and drag.
- **`react-native-reanimated`**: Provides complex animations with better performance.

---

### 16. How would you handle nested navigation in React Native?

You can nest navigators inside one another to create nested navigation flows. For example, you can have a `TabNavigator` inside a `StackNavigator` for a more complex navigation structure.

```javascript
function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Tabs" component={TabScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```
### 17. Can we use multiple types of navigators in a single app?

Yes, you can use multiple types of navigators within the same app. For example, you can combine `StackNavigator`, `TabNavigator`, and `DrawerNavigator` to handle different sections of the app.

```javascript
<NavigationContainer>
  <Drawer.Navigator>
    <Drawer.Screen name="Home" component={HomeScreen} />
    <Drawer.Screen name="Profile" component={ProfileScreen} />
  </Drawer.Navigator>
</NavigationContainer>
```

### 18. What is the linking prop in React Navigation?

The `linking` prop is used to configure how the app should respond to deep links. It defines URL prefixes and routes for deep linking.

```javascript
const linking = {
  prefixes: ['myapp://'],
  config: {
    screens: {
      Home: '',
      Details: 'details',
    },
  },
};

<NavigationContainer linking={linking}>
  <Stack.Navigator>
    <Stack.Screen name="Home" component={HomeScreen} />
    <Stack.Screen name="Details" component={DetailsScreen} />
  </Stack.Navigator>
</NavigationContainer>
```
### 19. What is the linking prop in React Navigation?

The `linking` prop is used to configure how the app should respond to deep links. It defines URL prefixes and routes for deep linking.

```javascript
const linking = {
  prefixes: ['myapp://'],
  config: {
    screens: {
      Home: '',
      Details: 'details',
    },
  },
};

<NavigationContainer linking={linking}>
  <Stack.Navigator>
    <Stack.Screen name="Home" component={HomeScreen} />
    <Stack.Screen name="Details" component={DetailsScreen} />
  </Stack.Navigator>
</NavigationContainer>
```

### 20. Explain how to handle authentication flow with React Navigation.

Authentication flows typically involve showing a login screen or an authenticated screen based on the user's login state.

You can use conditional rendering to determine which screen to show based on the authentication state.

```javascript
const isAuthenticated = useSelector(state => state.isAuthenticated);

<NavigationContainer>
  <Stack.Navigator>
    {isAuthenticated ? (
      <Stack.Screen name="Home" component={HomeScreen} />
    ) : (
      <Stack.Screen name="Login" component={LoginScreen} />
    )}
  </Stack.Navigator>
</NavigationContainer>





