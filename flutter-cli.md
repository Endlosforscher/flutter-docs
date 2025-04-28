
# Flutter CLI Basics

This document provides an overview of the essential commands and workflows available in the Flutter Command-Line Interface (CLI). It covers installation, common commands, and practical examples to help you get started.

----------

## 1. Prerequisites

-   **Dart SDK**: Bundled with Flutter.
    
-   **Git**: Required for installing Flutter from the repository.
    
-   **Platform Tools**:
    
    -   **Android SDK** (for Android development)
        
    -   **Xcode** (for iOS/macOS development on macOS)
        

----------

## 2. Installation

1.  **Clone the Flutter repository**:
    
    ```bash
    git clone https://github.com/flutter/flutter.git -b stable
    
    ```
    
2.  **Add Flutter to your PATH**:
    
    ```bash
    export PATH="$PATH:`pwd`/flutter/bin"
    
    ```
    
3.  **Verify the installation**:
    
    ```bash
    flutter --version
    
    ```
    
    Example output:
    
    ```text
    Flutter 3.7.0 • channel stable • https://github.com/flutter/flutter.git
    Framework  • revision 90b59f0908 (2 months ago)
    Engine     • revision 2b220b5c93
    Dart       • 2.19.0
    
    ```
    

----------

## 3. Common Commands

Below are the most frequently used Flutter CLI commands:

Command

Description

`flutter doctor`

Checks your environment and displays any issues.

`flutter create <name>`

Creates a new Flutter project.

`flutter run`

Runs your Flutter app on an attached device or emulator.

`flutter build`

Compiles your app for release (apk/ipa/web).

`flutter test`

Runs unit and widget tests.

`flutter clean`

Deletes build/output directories.

`flutter pub get`

Fetches dependencies listed in `pubspec.yaml`.

`flutter pub upgrade`

Upgrades dependencies to latest allowed versions.

----------

## 4. Workflow Examples

### 4.1 Creating a New Project

```bash
flutter create my_app
cd my_app

```

This generates a new directory `my_app` with a default Flutter counter app.

### 4.2 Running the App

1.  **Start an emulator or connect a device**.
    
2.  **Launch the app**:
    
    ```bash
    flutter run
    
    ```
    
    -   Use `-d` to specify the device ID:
        
        ```bash
        flutter run -d emulator-5554
        
        ```
        

### 4.3 Building for Release

-   **Android (APK)**:
    
    ```bash
    flutter build apk --release
    
    ```
    
-   **iOS (IPA)**:
    
    ```bash
    flutter build ios --release
    
    ```
    
-   **Web**:
    
    ```bash
    flutter build web
    
    ```
    

### 4.4 Testing

Run all tests in the `test/` directory:

```bash
flutter test

```

----------

## 5. Advanced CLI Tips

-   **Hot Reload**: While running your app, press `r` in the terminal to perform a hot reload.
    
-   **Hot Restart**: Press `R` for a full restart without rebuilding native code.
    
-   **Device List**: See all connected devices/emulators:
    
    ```bash
    flutter devices
    
    ```
    
-   **Upgrade Flutter**: Keep Flutter up to date:
    
    ```bash
    flutter upgrade
    
    ```
    

----------

## 6. Further Reading

-   [Flutter CLI Documentation](https://docs.flutter.dev/reference/flutter-cli)
    
-   [Flutter Cookbook](https://docs.flutter.dev/cookbook)
    

----------

_End of Flutter CLI Basics_
