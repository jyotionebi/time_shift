# 🚀 OneBI Google Sign-In: The Complete Guide

This guide is for any developer who needs to set up Google Login for the OneBI 
---

## 🛠 Step 1: Google Cloud Console (The "Server" Setup)

### 1. Project Selection
- Go to [Google Cloud Console](https://console.cloud.google.com/).
- Select your project at the top (Example: `OneBI-HRMS-Mobile`).

### 2. The "Parent" ID (Web Client ID)
- Go to **APIs & Services > Credentials**.
- Click **Create Credentials > OAuth client ID**.
- Select **Web application**.
- **Name**: `Master Web Client`.
- **Why?**: This is the "bridge" between your app and Google.
- **Save it**: Copy the `Client ID` (e.g., `3930...apps.googleusercontent.com`).

### 3. The "Child" ID (Android Client ID)
- Click **Create Credentials > OAuth client ID** again.
- Select **Android**.
- **Package Name**: `com.onebi.ess` (Must match the app's internal ID).
- **SHA-1 Fingerprint**: This is where you tell Google your app is "trusted". (See Step 2 below).

---

## 🔐 Step 2: The "Two SHA-1" Mystery (Why two?)

Google needs to know your app is legitimate. You MUST provide **two separate** Fingerprints:

| Type | When is it used? | Where do I get it? |
| :--- | :--- | :--- |
| **Debug SHA-1** | When you run `npm run android` on your laptop. | Local `debug.keystore` |
| **Release SHA-1** | When you build the official APK/ABB for users. | Your `my-release-key.keystore` |

### ⚡ How to generate them:
Open your terminal in the project folder and run:

**For Debug SHA-1:**
```bash
cd android && ./gradlew signingReport
```
*(Look for the "SHA1" line under the "debug" variant)*

**For Release SHA-1:**
```bash
keytool -list -v -keystore android/app/my-release-key.keystore -alias my-key-alias
```
*(Default password is `password123` unless changed)*

---

## 💻 Step 3: Adding Credentials to Code

Now that you have the IDs, you must put them in the code:

### 1. The Web Client ID
Open `src/screens/LoginScreen.tsx` and find:
```javascript
GoogleSignin.configure({
  webClientId: 'PASTE_YOUR_WEB_CLIENT_ID_HERE', // Use the "Web application" ID from Step 1.2
  offlineAccess: true,
});
```

### 2. The Google Services File
- Download `google-services.json` from the Firebase/Google console.
- Place it exactly at: `android/app/google-services.json`.
