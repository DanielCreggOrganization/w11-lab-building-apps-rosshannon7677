# Lab: Building APK Apps from an Ionic App

## Introduction
In this lab, you will learn how to set up your development environment and build an APK from an Ionic app step by step.

## Part 1: Environment Setup

### Step 1: Install Java Development Kit
First, we need to install Java 17, which is required for Android development:
```bash
sudo apt-get update
sudo apt-get install -y openjdk-17-jdk
```

Set up the JAVA_HOME environment variable:
```bash
export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:/bin/java::")
echo "export JAVA_HOME=$JAVA_HOME" >> ~/.bashrc
echo "export PATH=\$PATH:\$JAVA_HOME/bin" >> ~/.bashrc
```

### Step 2: Set Up Android SDK
1. Create a directory for the Android SDK:
```bash
mkdir -p ~/android-sdk
cd ~/android-sdk
```

2. Download and set up Android command-line tools:
```bash
wget https://dl.google.com/android/repository/commandlinetools-linux-9477386_latest.zip
unzip commandlinetools-linux-*_latest.zip
mkdir -p cmdline-tools/latest
mv cmdline-tools/* cmdline-tools/latest/
rmdir cmdline-tools/latest/cmdline-tools
rm commandlinetools-linux-*_latest.zip
```

3. Configure Android SDK environment variables:
```bash
export ANDROID_SDK_ROOT=~/android-sdk
echo "export ANDROID_SDK_ROOT=~/android-sdk" >> ~/.bashrc
echo "export PATH=\$PATH:\$ANDROID_SDK_ROOT/cmdline-tools/latest/bin" >> ~/.bashrc
echo "export PATH=\$PATH:\$ANDROID_SDK_ROOT/platform-tools" >> ~/.bashrc
```

4. Accept licenses and install required Android packages:
```bash
source ~/.bashrc
yes | sdkmanager --licenses
sdkmanager "platform-tools" "platforms;android-33" "build-tools;33.0.0"
```

## Part 2: Building the APK

### Step 1: Create and Build Ionic Project
1. Build your Ionic project:
```bash
ionic build --prod
```

2. Add Android platform support:
```bash
ionic capacitor add android
```

3. Sync your project with Capacitor:
```bash
ionic capacitor sync android
```

### Step 2: Build the APK
1. Navigate to the Android project directory:
```bash
cd android
```

2. Make the Gradle wrapper executable:
```bash
chmod +x gradlew
```

3. Build the debug APK:
```bash
./gradlew assembleDebug
```

Your APK will be created at: `android/app/build/outputs/apk/debug/app-debug.apk`

## Part 3: Building and Deploying a PWA

### Step 1: Add PWA Elements
First, add PWA elements to your Ionic project:
```bash
npm install @ionic/pwa-elements
```

Add the following to your `src/main.ts`:
```typescript
import { defineCustomElements } from '@ionic/pwa-elements/loader';
defineCustomElements(window);
```

### Step 2: Generate PWA Assets
Generate icons and splash screens for your PWA:
```bash
ionic capacitor resources
```

### Step 3: Build the PWA
Build your project with the production flag:
```bash
ionic build --prod
```

### Step 4: Test the PWA Locally
Install a local HTTP server if you haven't already:
```bash
npm install -g http-server
```

Serve your PWA:
```bash
cd www
http-server -p 8080
```

Visit `http://localhost:8080` in your browser. To test PWA features:
1. Open Chrome DevTools (F12)
2. Go to Application tab
3. Look for the "Service Workers" section

## Part 4: Building APK with Ionic AppFlow

### Step 1: Set Up Ionic AppFlow
1. Create an Ionic account at [dashboard.ionicframework.com](https://dashboard.ionicframework.com)
2. Install Ionic CLI with AppFlow capabilities:
```bash
npm install -g @ionic/cli@latest
```

### Step 2: Connect Your App
1. Initialize AppFlow in your project:
```bash
ionic link
```
2. Follow the prompts to:
   - Create a new app in AppFlow
   - Connect to your existing git repository

### Step 3: Configure Build Settings
1. Go to your app dashboard on AppFlow
2. Navigate to `Deploy > Builds`
3. Click "New Build"
4. Select following settings:
   - Platform: Android
   - Build Type: Debug/Release
   - Environment: Production
   - Build Stack: Latest

### Step 4: Initiate the Build
1. Click "Start Build"
2. AppFlow will:
   - Clone your repository
   - Install dependencies
   - Build the APK
   - Provide download link

## AppFlow vs Local Building
```mermaid
graph LR
    A[Your Code] --> B{Build Choice}
    B -->|Local| C[Local Machine Build]
    B -->|Cloud| D[Ionic AppFlow]
    C --> E[Manual Setup Required]
    C --> F[Free]
    D --> G[No Setup Required]
    D --> H[Subscription Based]
```

### Advantages of AppFlow
- No local environment setup needed
- Consistent build environment
- Build for iOS without a Mac
- Automated builds on git push
- Built-in signing key management

### Cost Considerations
- Free tier: Limited builds/month
- Paid tiers: 
  - More builds
  - Concurrent builds
  - Extra features

## Updated Process Flow
```mermaid
graph TD
    A[Install JDK] --> B[Setup Android SDK]
    B --> C[Install Android Tools]
    C --> D[Build Ionic Project]
    D --> E[Add Android Platform]
    E --> F[Sync with Capacitor]
    F --> G[Build APK with Gradle]
    D --> H[Add PWA Elements]
    H --> I[Generate PWA Assets]
    I --> J[Build Production PWA]
    J --> K[Deploy to Web Server]
    
    style A fill:#f9f,stroke:#333
    style G fill:#9ff,stroke:#333
    style K fill:#9ff,stroke:#333
```

## Testing PWA Features
- Test offline functionality by disabling network in DevTools
- Try "Add to Home Screen" on a mobile device
- Verify push notifications if implemented

## Common Issues and Solutions
1. If JAVA_HOME is not set, rerun: `source ~/.bashrc`
2. If Android SDK tools aren't found, verify your PATH settings
3. For permission issues, ensure you use sudo for apt commands

## Conclusion
You've learned how to manually set up your environment and build an Android APK from your Ionic app. These steps help you understand the build process and troubleshoot issues when they arise.
