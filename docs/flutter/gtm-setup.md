# Firebase Analytics & GTM Integration Guide

This guide walks you through integrating Firebase Analytics with Google Tag Manager (GTM) in your Flutter app, including server-side tracking configuration for both iOS and Android platforms.

## Prerequisites

- Flutter project setup
- Firebase project created
- Google Tag Manager container created

## Step 1: Add Firebase Analytics to Your Flutter Project

### Add Dependencies

Add the following dependencies to your `pubspec.yaml`:

```yaml
dependencies:
  firebase_core: ^2.24.2
  firebase_analytics: ^10.7.4
```

### Initialize Firebase

Update your `main.dart` file:

```dart
import 'package:firebase_core/firebase_core.dart';
import 'package:firebase_analytics/firebase_analytics.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(MyApp());
}
```

## Step 2: Configure iOS for GTM

### Enable Debug Mode in Xcode

1. Open your project in Xcode
2. Go to **Product** > **Scheme** > **Edit Scheme**
3. Select **Run** from the left panel
4. Go to the **Arguments** tab
5. Under **Arguments Passed On Launch**, add these three arguments:
   - `-FIRDebugEnabled`
   - `-FIRAnalyticsEnabled`
   - `-FIRAnalyticsVerboseLoggingEnabled`

### Update Info.plist

Add the following keys to your `ios/Runner/Info.plist` file:

```xml
<key>GOOGLE_ANALYTICS_SGTM_UPLOAD_ENABLED</key>
<true/>
<key>FirebaseAutomaticScreenReportingEnabled</key>
<false/>
```

## Step 3: Configure Android for GTM

### Enable Debug Mode

Run this command in your terminal (replace with your actual package ID):

```bash
adb shell setprop debug.firebase.analytics.app com.your.package.id
```

### Update AndroidManifest.xml

#### Add Preview Activity

Add this activity inside the `<application>` tag in `android/app/src/main/AndroidManifest.xml`:

```xml
<!-- Preview Activity to enable preview mode -->
<activity
    android:name="com.google.firebase.analytics.GoogleAnalyticsServerPreviewActivity"
    android:exported="true"
    android:noHistory="true">
  <intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />
    <data android:scheme="tagmanager.sgtm.c.YOUR_PACKAGE_ID" />
  </intent-filter>
</activity>
```

> **Note:** Replace `YOUR_PACKAGE_ID` with your actual app package ID (e.g., `com.your.package.id`)

#### Add Meta-data Configuration

Add these meta-data tags inside the `<application>` tag:

```xml
<!-- Enable uploads to Server-side GTM -->
<meta-data 
    android:name="google_analytics_sgtm_upload_enabled" 
    android:value="true" />

<!-- Disable automatic screen reporting -->
<meta-data 
    android:name="google_analytics_automatic_screen_reporting_enabled"
    android:value="false" />
```

## Step 4: Set Up Server-Side GTM Configuration

1. Follow this guide from Google: https://developers.google.com/tag-platform/tag-manager/server-side/server-side-tagging-for-mobile-apps?hl=de#ios


## Step 5: Test Your Setup

### Start Preview Mode

1. Open your GTM container
2. Go to the **Container Version** page
3. Create a preview session
4. Generate a QR code for mobile preview
5. Use the QR code generated in Step 4 to start container preview
6. Launch your app on the emulator/device with Debug Mode enabled
7. Check the following to confirm events are firing:
   - Firebase DebugView in the Firebase Console
   - Preview mode in GTM


## Step 6: Implement Manual Screen Tracking

Since Firebase's automatic screen tracking can be unreliable, implement custom screen tracking:

```dart
import 'package:firebase_analytics/firebase_analytics.dart';

class AnalyticsService {
  static final FirebaseAnalytics _analytics = FirebaseAnalytics.instance;
  
  static Future<void> logScreenView(String screenName) async {
    await _analytics.logEvent(
      name: 'screen_view',
      parameters: {
        'screen_name': screenName,
        'screen_class': screenName,
      },
    );
  }
}
```

### Usage Example

Call this method whenever you navigate to a new screen:

```dart
@override
void initState() {
  super.initState();
  AnalyticsService.logScreenView('HomeScreen');
}
```

## Troubleshooting

### Common Issues

1. **Events not showing in DebugView**
   - Ensure debug mode is properly enabled
   - Check that the app package ID matches in all configurations
   - Verify Firebase configuration files are correctly placed

2. **Preview mode not working**
   - Confirm the intent-filter scheme matches your package ID
   - Make sure the preview activity is properly declared
   - Check that the app is running on the same device used to scan the QR code

3. **Server-side tracking not working**
   - Verify `google_analytics_sgtm_upload_enabled` is set to `true`
   - Ensure your GTM server container is properly configured
   - Check network connectivity and server endpoint configuration

### Useful Commands

```bash
# Check Android debug properties
adb shell getprop debug.firebase.analytics.app

# Clear Android debug properties
adb shell setprop debug.firebase.analytics.app ""

# View Android logs
adb logcat | grep -i firebase
```

## Next Steps

Once your setup is complete and testing successfully:

1. Configure your GTM tags and triggers
2. Set up conversion tracking
3. Implement custom events for key user actions
4. Test thoroughly before publishing to production

---

**Note:** Remember to disable debug mode and remove verbose logging before releasing your app to production.