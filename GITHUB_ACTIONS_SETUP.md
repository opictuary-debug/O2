# GitHub Actions Setup for Opictuary Mobile Builds

This guide explains how to set up GitHub Actions to automatically build your Android AAB and iOS IPA files.

---

## Step 1: Push Your Code to GitHub

If you haven't already, push your Replit project to GitHub:

1. Go to the **Git** panel in Replit (left sidebar)
2. Connect to GitHub and create a new repository
3. Push your code

---

## Step 2: Set Up Android Build Secrets

In your GitHub repository:

1. Go to **Settings → Secrets and variables → Actions**
2. Click **New repository secret**
3. Add these two secrets:

| Secret Name | Value |
|-------------|-------|
| `ANDROID_KEYSTORE_PASSWORD` | Your keystore password (same as in Replit) |
| `ANDROID_KEY_PASSWORD` | Your key password (same as in Replit) |

**Note:** The keystore file (`opictuary-play.jks`) is already included in the repository.

---

## Step 3: Set Up iOS Build Secrets (Optional)

iOS builds require Apple Developer credentials. Add these secrets:

| Secret Name | Description |
|-------------|-------------|
| `IOS_CERTIFICATE_BASE64` | Your .p12 certificate encoded in base64 |
| `IOS_CERTIFICATE_PASSWORD` | Password for the .p12 certificate |
| `IOS_PROVISIONING_PROFILE_BASE64` | Your provisioning profile encoded in base64 |
| `IOS_KEYCHAIN_PASSWORD` | Any password for temporary keychain |
| `APPLE_TEAM_ID` | Your Apple Developer Team ID |

### How to encode files to base64:

On Mac/Linux:
```bash
base64 -i YourCertificate.p12 | pbcopy
base64 -i YourProfile.mobileprovision | pbcopy
```

On Windows (PowerShell):
```powershell
[Convert]::ToBase64String([IO.File]::ReadAllBytes("YourCertificate.p12")) | Set-Clipboard
```

---

## Step 4: Run the Builds

### Android Build (Automatic)
- Builds automatically when you push to the `main` branch
- Or manually trigger from **Actions → Build Android App → Run workflow**

### iOS Build (Manual)
- Go to **Actions → Build iOS → Run workflow**
- Optionally enter a build number
- Click **Run workflow**

---

## Step 5: Download Your Build Files

After a successful build:

1. Go to **Actions** tab in GitHub
2. Click on the completed workflow run
3. Scroll down to **Artifacts**
4. Download:
   - `opictuary-release.aab` - For Google Play Console
   - `opictuary-release.apk` - For direct testing
   - `Opictuary-iOS` - For App Store Connect (iOS)

---

## Uploading to App Stores

### Google Play Console
1. Go to [play.google.com/console](https://play.google.com/console)
2. Select your app → Release → Production
3. Create new release and upload the `.aab` file

### App Store Connect
1. Go to [appstoreconnect.apple.com](https://appstoreconnect.apple.com)
2. Use **Transporter** app to upload the `.ipa` file
3. Or upload directly through Xcode Organizer

---

## Troubleshooting

### Android Build Fails
- Verify keystore passwords are correct in GitHub Secrets
- Check that `opictuary-play.jks` exists in the repository
- Review the workflow logs for specific errors

### iOS Build Fails
- Ensure your Apple Developer certificates are valid
- Check that the provisioning profile matches your bundle ID
- Verify Team ID is correct

### Build Takes Too Long
- GitHub Actions has a 6-hour timeout
- Android builds typically take 5-10 minutes
- iOS builds typically take 10-15 minutes

---

## Current App Configuration

- **Bundle ID:** `com.opictuary.memorial`
- **App Name:** Opictuary
- **Version:** 2.0.0
- **Android Version Code:** 2025112100

---

## Need Help?

- Check [GitHub Actions documentation](https://docs.github.com/en/actions)
- Review [Capacitor iOS docs](https://capacitorjs.com/docs/ios)
- Review [Capacitor Android docs](https://capacitorjs.com/docs/android)
