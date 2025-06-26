# Cloudflare Workers for Deeplink Setup

This guide shows you how to set up Cloudflare Workers to handle deeplinking for your mobile app. This is especially useful when using services like Wix where it's difficult to add certificate files from Apple and Google directly.

## Overview

Cloudflare Workers allow you to run serverless functions at the edge, making them perfect for handling deeplink redirects and serving the required verification files for both iOS and Android platforms.

## Prerequisites

- Cloudflare account with your domain configured
- Mobile app with deeplink support
- Apple App Site Association (AASA) file for iOS
- Android Asset Links file for Android

## Step 1: Add Subdomain in Cloudflare

### Adding a CNAME Record

1. **Log into Cloudflare Dashboard**
   - Go to [Cloudflare Dashboard](https://dash.cloudflare.com/)
   - Select your domain

2. **Navigate to DNS Settings**
   - Click on the **DNS** tab in the left sidebar

3. **Add CNAME Record**
   - Click **Add record**
   - Select **CNAME** as the record type
   - Enter your subdomain name (e.g., `open`)
   - Enter your target domain (e.g., `expelino.de`)
   - **Enable Proxy** (orange cloud icon) - This is important for Workers to function
   - Click **Save**

### Example Configuration

Your DNS record should look like this in the Cloudflare dashboard:

| Type  | Name | Target      | Proxy Status |
|-------|------|-------------|--------------|
| CNAME | open | expelino.de | Proxied (🟠) |


> **Important**: The proxy must be enabled (orange cloud) for Cloudflare Workers to intercept requests. The "Mit Proxy" status indicates this is correctly configured.

## Step 2: Create Cloudflare Workers

### Setting Up the Workers

1. **Access Workers Dashboard**
   - In your Cloudflare dashboard, click **Workers & Pages** in the left sidebar
   - Click **Create**
   - Select **Create Worker**

2. **Create Required Workers**
   You'll need to create three workers for complete deeplink support:
   
   - `deeplink-resolver` - Main deeplink handler
   - `android-deeplink` - Android-specific handling
   - `ios-deeplink` - iOS-specific handling

### Creating Each Worker

For each worker, follow these steps:

1. **Click "Create Worker"**
   - Enter a descriptive name (e.g., `deeplink-resolver`)
   - Click **Save and Deploy**

2. **Edit Worker Code**
   - Click **Edit Code** to open the code editor
   - Replace the default code with the respective worker code below
   - Click **Save and Deploy**


### Worker 1: Deeplink Resolver (work in progress)

This worker handles the main deeplink resolution logic. Currently, it returns a simple 200 status response as a placeholder. This basic implementation ensures the deeplink infrastructure is functional while we develop more sophisticated user flow handling cases like entering the app from instagram brwoser which is a widely known issue.


### Worker 2: Android Deeplink (Asset Links)

```javascript
export default {
  async fetch(request, env, ctx) {
    const response = new Response(JSON.stringify([
      {
        "relation": ["delegate_permission/common.handle_all_urls"],
        "target": {
          "namespace": "android_app",
          "package_name": "YOUR_ANDROID_PACKAGE_NAME", // e.g., "com.expelino.app"
          "sha256_cert_fingerprints": [
            "YOUR_SHA256_CERT_FINGERPRINT_HERE" // e.g., "F6:6D:D1:47:DF:83:9D:A6:45:85:B2:36:D6:79:F8:5E:42:75:F8:50:26:E9:88:0E:B1:E8:E0:0A:AC:7B:62:76"
          ]
        }
      }
    ]), { status: 200 });
    
    response.headers.set("Content-Type", "application/json");
    response.headers.set("Access-Control-Allow-Origin", "*");
    
    return response;
  },
};
```

### Worker 3: iOS Deeplink (Apple App Site Association)

```javascript
export default {
  async fetch(request, env, ctx) {
    const response = new Response(JSON.stringify({
      "applinks": {
        "apps": [],
        "details": [
          {
            "appIDs": [
              "YOUR_TEAM_ID.YOUR_BUNDLE_IDENTIFIER" // e.g., "
              // XXXXX.com.expelino.app"
            ],
            "appId": "YOUR_TEAM_ID.YOUR_BUNDLE_IDENTIFIER", // e.g., "XXXXX.com.expelino.app"
            "paths": [
              "/*"
            ],
            "components": [
              {
                "/": "/*",
                "comment": "Handles all paths for deep linking"
              }
            ]
          }
        ]
      },
      "webcredentials": {
        "apps": [
          "YOUR_TEAM_ID.YOUR_BUNDLE_IDENTIFIER" // e.g., "XXXXXX.com.expelino.app"
        ]
      }
    }), {
      status: 200
    });
    
    response.headers.set("Content-Type", "application/json");
    response.headers.set("Access-Control-Allow-Origin", "*");
    
    return response;
  },
};
```

## Step 3: Configure Worker Routes

### Setting Up Routes

1. **Navigate to Workers**
   - Go to **Workers & Pages** > **Overview**
   - Click on your worker name

2. **Add Routes**
   - Click **Settings** tab
   - Click **Triggers** 
   - Click **Add route**

### Route Configuration

Configure routes for each worker:

| Worker | Route Pattern | Description |
|--------|---------------|-------------|
| `deeplink-resolver` | `open.expelino.de/*` | Main deeplink handling |
| `android-deeplink` | `open.expelino.de/.well-known/assetlinks.json` | Android verification |
| `ios-deeplink` | `open.expelino.de/.well-known/apple-app-site-association` | iOS verification |

## Step 4: Configuration for Your App

### Update Your App Configuration

Replace the following placeholders in the worker code:

1. **Android Worker Placeholders**
   - `YOUR_ANDROID_PACKAGE_NAME` - Your Android package name (e.g., `"com.expelino.app"`)
   - `YOUR_SHA256_CERT_FINGERPRINT_HERE` - Your Android app's SHA256 fingerprint (You can find them in Google Play console)

2. **iOS Worker Placeholders**  
   - `YOUR_TEAM_ID.YOUR_BUNDLE_IDENTIFIER` - Your complete App ID (e.g., `"XXXXX.com.expelino.app"`)
     - `YOUR_TEAM_ID` - Your iOS Team ID (10-character string like `"XXXXX"`)
     - `YOUR_BUNDLE_IDENTIFIER` - Your iOS bundle identifier (like `"com.expelino.app"`)

3. **Main Worker Placeholders**
   - `yourapp://` - Your app's custom URL scheme
   - `your-app-id` - Your iOS App Store ID
   - `your.package.name` - Your Android package name (for Play Store URL)

### Getting Required Values

#### Android SHA256 Fingerprint
```bash
keytool -list -v -keystore your-release-key.keystore -alias your-key-alias
```

#### iOS Team ID
- Found in your Apple Developer account
- Usually a 10-character string


### Verification Tools

- **Android**: Use [Android Asset Links Tester](https://developers.google.com/digital-asset-links/tools/generator)
- **iOS**: Use [Apple App Site Association Validator](https://branch.io/resources/aasa-validator/)

## Troubleshooting

### Common Issues

1. **Worker Not Triggering**
   - Ensure proxy is enabled (orange cloud) in DNS settings
   - Check route patterns match exactly
   - Verify worker is deployed and active

2. **Certificate Files Not Loading**
   - Check file paths in worker code
   - Ensure proper JSON formatting
   - Verify CORS headers are set

3. **Deeplinks Not Working**
   - Test on actual devices, not simulators
   - Check URL scheme registration in app
   - Verify certificate fingerprints are correct

---

**Note**: Remember to update your mobile app's configuration to handle the deeplinks properly and test thoroughly on both iOS and Android devices before going live.