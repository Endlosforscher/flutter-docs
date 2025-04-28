# Flutter Authentication

This document covers how to implement **authentication** in Flutter apps, including methods like email/password, social login, and third-party services like Firebase.

---

## 1. Why Authentication Matters

- Verify user identity.
- Personalize user experience.
- Secure user data and sessions.
- Enable features like account management, notifications, and cloud data.

---

## 2. Common Authentication Methods

- **Email & Password** authentication.
- **Phone number** authentication.
- **Social sign-in** (Google, Apple, Facebook, etc.).
- **Biometric authentication** (fingerprint, face recognition).
- **OAuth 2.0** with custom identity providers.

---

## 3. Email and Password Authentication (Firebase Example)

**Setup**:

1. Add `firebase_auth` to `pubspec.yaml`:

```yaml
dependencies:
  firebase_auth: ^4.0.0
  firebase_core: ^3.0.0
```

2. Initialize Firebase:

```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(MyApp());
}
```

3. Sign up and Sign in Methods:

```dart
final FirebaseAuth _auth = FirebaseAuth.instance;

Future<UserCredential> signUp(String email, String password) async {
  return await _auth.createUserWithEmailAndPassword(email: email, password: password);
}

Future<UserCredential> signIn(String email, String password) async {
  return await _auth.signInWithEmailAndPassword(email: email, password: password);
}
```

4. Sign Out:

```dart
Future<void> signOut() async {
  await _auth.signOut();
}
```

---

## 4. Social Authentication (Google Example)

1. Add `google_sign_in` package:

```yaml
dependencies:
  google_sign_in: ^6.1.0
```

2. Implement Google Sign-In:

```dart
final GoogleSignIn _googleSignIn = GoogleSignIn();

Future<void> signInWithGoogle() async {
  final GoogleSignInAccount? googleUser = await _googleSignIn.signIn();
  if (googleUser == null) return; // The user canceled the sign-in

  final GoogleSignInAuthentication googleAuth = await googleUser.authentication;

  final credential = GoogleAuthProvider.credential(
    accessToken: googleAuth.accessToken,
    idToken: googleAuth.idToken,
  );

  await FirebaseAuth.instance.signInWithCredential(credential);
}
```

---

## 5. Biometric Authentication (Local Auth)

1. Add `local_auth` package:

```yaml
dependencies:
  local_auth: ^2.1.6
```

2. Authenticate user:

```dart
final LocalAuthentication auth = LocalAuthentication();

Future<bool> authenticateWithBiometrics() async {
  final bool canAuthenticate = await auth.canCheckBiometrics;

  if (!canAuthenticate) return false;

  return await auth.authenticate(
    localizedReason: 'Please authenticate to access',
    options: const AuthenticationOptions(biometricOnly: true),
  );
}
```

---

## 6. Best Practices for Authentication

- Always validate input fields (email format, password strength).
- Handle authentication exceptions and provide user feedback.
- Securely store tokens and sensitive information.
- Use biometric authentication to improve UX and security.
- Use session management (token refresh, timeout handling).

---

## 7. Common Packages

- [`firebase_auth`](https://pub.dev/packages/firebase_auth)
- [`google_sign_in`](https://pub.dev/packages/google_sign_in)
- [`local_auth`](https://pub.dev/packages/local_auth)
- [`flutter_facebook_auth`](https://pub.dev/packages/flutter_facebook_auth)
- [`sign_in_with_apple`](https://pub.dev/packages/sign_in_with_apple)

---

## 8. Further Reading

- [Firebase Authentication Documentation](https://firebase.google.com/docs/auth)
- [Flutter Authentication Samples](https://github.com/FirebaseExtended/flutterfire/tree/master/packages/firebase_auth/firebase_auth/example)

---

*End of Flutter Authentication*

