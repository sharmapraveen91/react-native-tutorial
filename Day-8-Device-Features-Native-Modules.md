# Device Features & Native Modules in React Native

## Introduction
React Native allows mobile apps to use device features like **Camera**, **Media**, **Geolocation**, **Maps**, and **Push Notifications** through native modules. These features help create interactive and user-friendly applications.

Think of a smartphone as a toolbox. Native modules act like tools within the box—using them correctly allows your app to perform real-world tasks.

---

## 1. Camera

### Analogy:
- Using the camera is like having a digital eye inside your app—it sees and captures the world.

### Installation:
```bash
yarn add react-native-camera react-native-permissions
npx pod install
```

### Permissions:
Add these permissions in `Info.plist` (iOS):
```xml
<key>NSCameraUsageDescription</key>
<string>App needs access to your camera to take photos.</string>
```

Add these permissions in `AndroidManifest.xml` (Android):
```xml
<uses-permission android:name="android.permission.CAMERA" />
```

### Code Example:
```typescript
import React from 'react';
import { View, Button } from 'react-native';
import { launchCamera } from 'react-native-image-picker';

const CameraExample = () => {
    const openCamera = () => {
        launchCamera({ mediaType: 'photo' }, (response) => {
            if (response.assets) console.log(response.assets[0].uri);
        });
    };

    return (
        <View>
            <Button title="Open Camera" onPress={openCamera} />
        </View>
    );
};

export default CameraExample;
```

### Do's and Don'ts:
- ✅ Use permissions to ensure privacy.
- ✅ Optimize image size for performance.
- ❌ Don’t block the UI while capturing.

---

## 2. Media (Gallery & Audio)

### Analogy:
- Accessing media is like browsing a photo album or playing songs on a music player.

### Installation:
```bash
yarn add react-native-image-picker react-native-sound
npx pod install
```

### Code Example (Gallery):
```typescript
import { launchImageLibrary } from 'react-native-image-picker';

const openGallery = () => {
    launchImageLibrary({ mediaType: 'photo' }, (response) => {
        if (response.assets) console.log(response.assets[0].uri);
    });
};
```

### Code Example (Audio):
```typescript
import Sound from 'react-native-sound';

const playSound = () => {
    const sound = new Sound('sample.mp3', Sound.MAIN_BUNDLE, (error) => {
        if (!error) sound.play();
    });
};
```

### Do's and Don'ts:
- ✅ Optimize audio and images for faster loading.
- ✅ Provide fallback options if permissions are denied.
- ❌ Avoid excessive media usage that affects performance.

---

## 3. Geolocation

### Analogy:
- Geolocation is like a digital compass that helps your app know where the user is.

### Installation:
```bash
yarn add @react-native-community/geolocation
npx pod install
```

### Permissions:
In `Info.plist` (iOS):
```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>App needs access to your location to provide location-based services.</string>
```

In `AndroidManifest.xml` (Android):
```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

### Code Example:
```typescript
import Geolocation from '@react-native-community/geolocation';

const getLocation = () => {
    Geolocation.getCurrentPosition(
        (position) => console.log(position.coords),
        (error) => console.error(error),
        { enableHighAccuracy: true, timeout: 15000, maximumAge: 10000 }
    );
};
```

### Do's and Don'ts:
- ✅ Always request permissions before accessing location.
- ✅ Use high accuracy mode only when needed.
- ❌ Don’t track location continuously unless essential.

---

## 4. Maps

### Analogy:
- Maps are like a digital guide that helps users navigate and explore locations.

### Installation:
```bash
yarn add react-native-maps
npx pod install
```

### Code Example:
```typescript
import React from 'react';
import MapView, { Marker } from 'react-native-maps';

const MapsExample = () => (
    <MapView
        style={{ flex: 1, height: 400 }}
        initialRegion={{ latitude: 37.78825, longitude: -122.4324, latitudeDelta: 0.0922, longitudeDelta: 0.0421 }}
    >
        <Marker coordinate={{ latitude: 37.78825, longitude: -122.4324 }} title="My Location" />
    </MapView>
);

export default MapsExample;
```

### Do's and Don'ts:
- ✅ Limit the number of map markers for better performance.
- ✅ Use caching for faster loading.
- ❌ Avoid loading too many map layers.

---

## 5. Push Notifications

### Analogy:
- Push notifications are like digital reminders that alert users even when the app is closed.

### Installation:
```bash
yarn add @react-native-firebase/app @react-native-firebase/messaging
npx pod install
```

### Permissions:
In `Info.plist` (iOS):
```xml
<key>FirebaseAppDelegateProxyEnabled</key>
<false/>
```

In `AndroidManifest.xml` (Android):
```xml
<uses-permission android:name="android.permission.POST_NOTIFICATIONS" />
```

### Code Example:
```typescript
import messaging from '@react-native-firebase/messaging';

const requestUserPermission = async () => {
    const authStatus = await messaging().requestPermission();
    if (authStatus === messaging.AuthorizationStatus.AUTHORIZED) {
        console.log('Authorization status:', authStatus);
    }
};

const onMessageReceived = async (remoteMessage: any) => {
    console.log('Notification received:', remoteMessage);
};

messaging().onMessage(onMessageReceived);
```

### Do's and Don'ts:
- ✅ Use notifications sparingly to avoid annoying users.
- ✅ Customize notifications for relevance.
- ❌ Don’t send notifications without user consent.

---

## Best Practices
- 🟢 Always handle permissions gracefully.
- 🟢 Optimize media and map usage for performance.
- 🟢 Ensure fallback behavior when features are unavailable.
- 🟢 Test features on both Android and iOS devices.

---

## Conclusion
Device features and native modules make React Native apps more interactive and functional. Using analogies and simple code examples helps beginners understand how to use Camera, Media, Geolocation, Maps, and Push Notifications effectively.

---

