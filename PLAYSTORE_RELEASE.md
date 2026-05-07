# Play Store Production Release (Expo + EAS)

## 1) Set production env values

Create/update `.env` (or EAS env vars) with production values:

```env
EXPO_PUBLIC_Backend=https://devapibizcard.support.marketaspex.com/api/v1/
EXPO_PUBLIC_REVENUECAT_ANDROID_API_KEY=goog_MimiMAobZndDMxqMiLzCiLiTndjgoog_xxx
```

## 2) Configure Android signing

Use one of these approaches:

1. Recommended: let EAS manage Android credentials.
2. Manual keystore (local/CI):
   - set `MYAPP_UPLOAD_STORE_FILE`
   - set `MYAPP_UPLOAD_STORE_PASSWORD`
   - set `MYAPP_UPLOAD_KEY_ALIAS`
   - set `MYAPP_UPLOAD_KEY_PASSWORD`

For local Gradle builds, you can add these to `android/gradle.properties` (do not commit secrets).

## 3) Google Sign-In production checks

1. In Google Cloud Console, use OAuth client for Android package `com.raza7304.bizcardapp`.
2. Add **both** SHA-1/SHA-256 fingerprints:
   - upload keystore fingerprint
   - Play App Signing fingerprint (from Play Console)
3. Ensure `EXPO_PUBLIC_GOOGLE_WEB_CLIENT_ID` matches the Web OAuth client used by your backend token verification.

## 4) RevenueCat production checks

1. In RevenueCat, ensure app is linked to Play package `com.raza7304.bizcardapp`.
2. Confirm products/offering IDs match Play Console subscriptions.
3. Verify entitlement mapping (for example `Monthly Pro`) is attached to the correct product IDs.

## 5) Build production AAB

```bash
yarn build:android:prod
```

This uses EAS `production` profile and builds `app-bundle` (`.aab`).

## 6) Submit to Play

Automatic submit with EAS:

```bash
yarn submit:android:prod
```

Or upload the generated `.aab` manually in Play Console.

## 7) Play Console release steps

1. Complete Store Listing, Data Safety, Content Rating, App Access, and Ads declaration.
2. Create/choose a Production release.
3. Upload `.aab`.
4. Review warnings/errors (permissions, policy, SDK declarations).
5. Roll out to production (start with staged rollout if needed).

## 8) Post-release verification

1. Install from Play internal/production track.
2. Verify Google Sign-In on a real device.
3. Verify new subscription purchase and restore flow.
4. Verify backend API base URL is production.
