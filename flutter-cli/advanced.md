
# Flutter CLI Advanced

This document dives into advanced features and workflows of the Flutter Command-Line Interface (CLI). It covers build configurations, custom scripts, performance profiling, continuous integration, and more.

----------

## 1. Build Flavors & Environments

Flutter supports multiple build configurations (flavors) for Android and iOS. This allows you to maintain different environments (e.g., development, staging, production).

### 1.1 Android Flavors

1.  **Configure `android/app/build.gradle`:**
    
    ```groovy
    android {
      flavorDimensions "environment"
      productFlavors {
        dev {
          dimension "environment"
          applicationIdSuffix ".dev"
          versionNameSuffix "-dev"
        }
        prod {
          dimension "environment"
        }
      }
    }
    
    ```
    
2.  **Run with flavor**:
    
    ```bash
    flutter run --flavor dev -t lib/main_dev.dart
    flutter build apk --flavor prod -t lib/main_prod.dart
    
    ```
    

### 1.2 iOS Schemes

1.  **Add new schemes in Xcode** and map them to build configurations (e.g., Dev, Prod).
    
2.  **Run with scheme**:
    
    ```bash
    flutter build ios --flavor Dev \
      --export-options-plist=ios/ExportOptions-Dev.plist
    
    ```
    

----------

## 2. Dart-Define Variables

Use `--dart-define` to pass compile-time constants into your app.

```bash
flutter run \
  --dart-define=API_URL=https://api.dev.example.com \
  --dart-define=ENABLE_LOGGING=true

```

Access in code:

```dart
const String apiUrl = String.fromEnvironment('API_URL');
const bool enableLogging = bool.fromEnvironment('ENABLE_LOGGING');

```

----------

## 3. Custom Scripts & Aliases

You can create shell or npm scripts to simplify repetitive commands.

### 3.1 Example Bash Script (`scripts/run.sh`)

```bash
#!/usr/bin/env bash

ENV=\${1:-dev}

case "\$ENV" in
  dev)
    FLAVOR=dev
    ENTRY=lib/main_dev.dart
    ;;
  prod)
    FLAVOR=prod
    ENTRY=lib/main_prod.dart
    ;;
  *)
    echo "Usage: ./run.sh [dev|prod]"
    exit 1
    ;;
esac

flutter run --flavor \${FLAVOR} -t \${ENTRY}

```

Make executable:

```bash
chmod +x scripts/run.sh
./scripts/run.sh prod

```

----------

## 4. Code Analysis & Linting

### 4.1 Analyze

```bash
flutter analyze

```

### 4.2 Format

```bash
flutter format .

```

### 4.3 Fix

Apply fixes automatically:

```bash
dart fix --apply

```

----------

## 5. Performance Profiling & Debugging

### 5.1 Profile Mode

Run in profile mode:

```bash
flutter run --profile

```

### 5.2 DevTools

Open performance and memory tools:

```bash
flutter pub global activate devtools
flutter pub global run devtools

```

Then visit the provided URL.

----------

## 6. Continuous Integration (CI)

### 6.1 GitHub Actions Example

```yaml
name: Flutter CI

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
      - uses: subosito/flutter-action@v2
        with:
          flutter-version: '3.7.0'
      - run: flutter pub get
      - run: flutter analyze
      - run: flutter test --coverage
      - run: flutter build apk --release

```

----------

## 7. Code Signing & Deployment

### 7.1 Android Keystore

```bash
keytool -genkey -v -keystore key.jks -alias mykey \
  -keyalg RSA -keysize 2048 -validity 10000

```

Configure `key.properties` and reference in `build.gradle`.

### 7.2 iOS Signing

Use `match` from `fastlane` or configure provisioning profiles in Xcode.

----------

## 8. Cleaning & Cache

-   **Clean build**:
    
    ```bash
    flutter clean
    
    ```
    
-   **Clear pub cache**:
    
    ```bash
    flutter pub cache repair
    
    ```
    

----------

## 9. Further Resources

-   [Flavor Support](https://docs.flutter.dev/deployment/flavors)
    
-   [CI/CD with Flutter](https://docs.flutter.dev/deployment/ci)
    
-   [DevTools Guide](https://docs.flutter.dev/tools/devtools/overview)
    

----------

_End of Flutter CLI Advanced_
