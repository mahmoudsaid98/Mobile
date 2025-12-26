# Firebase Setup Guide (Flutter – Android Only)

This file explains how to set up Firebase for a **Flutter Android app only**.

---

## 1. Create a Firebase Project

1. Go to the **Firebase Console**.
2. Click **Add project**.
3. Enter a project name.
4. Disable Google Analytics (optional).
5. Click **Create project**.

---

## 2. Register Android App

1. In Firebase Console, click **Add app → Android**.
2. Enter the **package name** (found in `android/app/build.gradle`).
3. (Optional) Add app nickname.
4. Download `google-services.json`.
5. Move the file to:

   ```
   android/app/google-services.json
   ```

---

## 3. Configure Android Gradle

### `android/build.gradle`

Add Google services classpath:

```gradle
buildscript {
    dependencies {
        classpath 'com.google.gms:google-services:4.4.0'
    }
}
```

### `android/app/build.gradle`

At the bottom of the file:

```gradle
apply plugin: 'com.google.gms.google-services'
```

Make sure `minSdkVersion` is **21 or higher**.

---

## 4. Install Firebase Packages

Run:

```bash
flutter pub add firebase_core
flutter pub add firebase_auth
flutter pub add cloud_firestore
```

Then:

```bash
flutter pub get
```

---

## 5. Initialize Firebase

### `main.dart`

```dart
import 'package:firebase_core/firebase_core.dart';
import 'package:flutter/material.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(const MyApp());
}
```

---

## 6. Enable Firebase Services

### Authentication (Email & Password)

1. Firebase Console → **Authentication**.
2. Click **Get started**.
3. Enable **Email/Password**.

### Firestore Database

1. Firebase Console → **Firestore Database**.
2. Click **Create database**.
3. Choose **test mode** (for development).

---

## 7. Firestore Security Rules (Basic)

```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

---

## 8. Common Android Errors

* **No currently active project**

  ```bash
  firebase use --add
  ```

* **Gradle lock / build stuck**

  * Close Android Studio
  * Stop all Gradle/Java processes
  * Run:

    ```bash
    flutter clean
    ```

* **App not navigating after signup**

  * Check `context.mounted` before navigation

---

## 9. Useful Commands

```bash
flutter clean
flutter pub get
flutter run
firebase login
firebase projects:list
```

---

## Done ✅

Firebase is now connected to your **Flutter Android app**.
