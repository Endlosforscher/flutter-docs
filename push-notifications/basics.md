# Flutter Push Notifications

This document describes how to implement **push notifications** in Flutter applications using services like Firebase Cloud Messaging (FCM).

---

## 1. What Are Push Notifications?

- Messages sent to the user's device even when the app is not active.
- Used for alerts, updates, promotional content, or important communication.

---

## 2. Common Services for Push Notifications

- **Firebase Cloud Messaging (FCM)**
- **OneSignal**
- **Pusher Beams**
- **Custom push servers**

---

## 3. Setting Up Push Notifications with Firebase Cloud Messaging (FCM)

### 3.1. Install Required Packages

```yaml
dependencies:
  firebase_core: ^3.0.0
  firebase_messaging: ^15.0.0
```

### 3.2. Configure Firebase

1. Create a Firebase project.
2. Add Android and iOS apps to the project.
3. Download and configure `google-services.json` (Android) or `GoogleService-Info.plist` (iOS).

### 3.3. Initialize Firebase in Your App

```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(MyApp());
}
```

### 3.4. Request Notification Permissions (iOS)

```dart
FirebaseMessaging messaging = FirebaseMessaging.instance;

NotificationSettings settings = await messaging.requestPermission(
  alert: true,
  badge: true,
  sound: true,
);
```

### 3.5. Listen for Messages

```dart
FirebaseMessaging.onMessage.listen((RemoteMessage message) {
  print('Received a message while in the foreground!');
  print('Message data: ${message.data}');

  if (message.notification != null) {
    print('Message also contained a notification: ${message.notification}');
  }
});
```

### 3.6. Background & Terminated States

```dart
FirebaseMessaging.onMessageOpenedApp.listen((RemoteMessage message) {
  print('Message clicked!');
});

Future<void> _firebaseMessagingBackgroundHandler(RemoteMessage message) async {
  await Firebase.initializeApp();
  print('Handling a background message: ${message.messageId}');
}

FirebaseMessaging.onBackgroundMessage(_firebaseMessagingBackgroundHandler);
```

### 3.7. Get the Device Token

```dart
String? token = await FirebaseMessaging.instance.getToken();
print('Device FCM Token: $token');
```

---

## 4. Sending Notifications

- Use Firebase Console (manual push).
- Use Cloud Functions or server SDKs to trigger notifications programmatically.
- Include topics or token targeting.

---

## 5. Handling Notifications

- **Foreground**: Handle in `onMessage`.
- **Background**: Handle in `onMessageOpenedApp`.
- **Terminated**: App opens from notification click.

Display notifications using the [`flutter_local_notifications`](https://pub.dev/packages/flutter_local_notifications) package if you want custom local popups.

---

## 6. Best Practices

- Always ask for permissions explicitly.
- Handle notification clicks appropriately (deep linking).
- Group and manage notifications.
- Secure token management.
- Respect user preferences (opt-in/opt-out settings).

---

## 7. Common Packages

- [`firebase_messaging`](https://pub.dev/packages/firebase_messaging)
- [`flutter_local_notifications`](https://pub.dev/packages/flutter_local_notifications)
- [`awesome_notifications`](https://pub.dev/packages/awesome_notifications)

---

## 8. Further Reading

- [Firebase Messaging Documentation](https://firebase.google.com/docs/cloud-messaging)
- [Flutter Push Notifications Codelab](https://firebase.flutter.dev/docs/messaging/overview/)

---

*End of Flutter Push Notifications*

