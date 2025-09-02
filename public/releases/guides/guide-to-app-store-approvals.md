
This guide is designed to help prepare your app for an app store review if you’re using Kinde for auth. It provides recommendations and solutions for setting up reviewer access.

This isn’t a complete list of preparation tasks, and there’s no guarantee your app will pass using only this guide, but it’s a solid starting point. 

We’ll continue updating it as new scenarios arise. If you have suggestions, let us know.

## Prepare for Apple App Store review

Here’s how to set up Kinde in preparation for an Apple App Store review. We recommend setting up a hidden login route that’s only active when your app is being reviewed by Apple. Here’s how:

### Step 1: Detect the App Store review environment

Apple doesn’t offer an official way to detect its review environment, but you can use a few indirect methods.

**Method A: Check for a sandbox receipt**

When Apple reviews your app, all transactions occur in a sandbox environment. On iOS, you can check whether the app’s receipt is a sandbox receipt:

```swift

func isRunningInTestFlightOrAppStoreReview() -> Bool {
    #if DEBUG
        return false // Debug builds should not use the hidden route
    #else
        if let url = Bundle.main.appStoreReceiptURL {
            return url.lastPathComponent == "sandboxReceipt
        }
        return false
    #endif
}
```

This method works because Apple’s review devices generate a `sandboxReceipt` file.

**Method B: Check TestFlight for a sandbox receipt**

If your app is distributed via TestFlight, you can also check for a sandbox receipt:

```swift
func isRunningInTestFlight() -> Bool {
    return Bundle.main.appStoreReceiptURL?.lastPathComponent == "sandboxReceipt
}
```

### Step 2: Create a hidden login route for Apple testers

Once you detect that the app is running in Apple’s review environment, you can enable a special login route. 

**Method A: Display a hidden sign-in button** 

Using this method, you display a hidden sign-in button only when `isRunningInTestFlightOrAppStoreReview()` returns `true`. Testers can then sign in with predefined credentials:

```swift
if isRunningInTestFlightOrAppStoreReview() {
    showHiddenLoginButton()
}
```

**Method B: Provide a deep link for signing in**

Using this method, you provide Apple with a deep link like `myapp://hidden-login`. You must ensure this route is only active when the app is in review mode.

You can set this up so that when Apple taps the link, they are signed in automatically or they are shown a sign-in screen with pre-filled demo credentials.

You might need to set up a webhook or other alert so you know when the link is clicked.

### Step 3: Add notes in the App Store review submission

Whatever method you use, clearly explain how testers can access your app in the **App Review Notes**. Provide the method and the credentials they can use. 

Example message:

> Dear Apple Reviewer,

Since our app does not use email/password authentication, we have created a hidden sign in option for you to test.

If you are testing in `TestFlight` or the App Store review environment, a special sign in button will appear.

Alternatively, access the sign in screen via this deep link: `myapp://hidden-login`

Test credentials:
review@myapp.com
P******d
> 

## Google Play Store

Here’s how to set up Kinde in preparation for an Google Play review. We recommend setting up a hidden login route that’s only active when your app is being reviewed by Google. 

Here’s how to set up a hidden authentication route for the Google Play app review process.

### Step 1: Detect the Google Play review environment

Google Play doesn’t offer an official review environment flag, but you can use indirect methods to detect it:

- **Check the installer package**: Apps installed from Google Play will have `com.android.vending` as the installer.
- **Check device characteristics**: Review devices may share common traits, though this is less reliable.

**Example (Kotlin):**

```kotlin
import android.content.Context
import android.content.pm.PackageManager

fun isGooglePlayReviewEnvironment(context: Context): Boolean {
    val installer = context.packageManager.getInstallerPackageName(context.packageName)
    return installer == "com.android.vending
}
```

### **Step 2: Restrict access to the hidden auth route**

Once you detect the review environment, create a secure, hidden API route.

1. Check for a secret token in the request header.
2. Allow access only if the token matches your predefined key.

**Example (Next.js API Route):**

```kotlin
import { NextResponse } from 'next/server';

export async function POST(req: Request) {
  const playStoreSecret = req.headers.get('x-play-review-secret');
  const isPlayStoreReview = playStoreSecret === process.env.PLAY_REVIEW_SECRET;

  if (isPlayStoreReview) {
    return NextResponse.json({ message: 'Hidden auth route accessed!' });
  } else {
    return NextResponse.json({ error: 'Forbidden' }, { status: 403 });
  }
}
```

### **Step 3: Add a secret header in the app (during review only)**

When the app detects it's running in the Google Play review environment, send the secret header.

**Example (Kotlin using Retrofit):**

```kotlin
val isReview = isGooglePlayReviewEnvironment(context)

if (isReview) {
    val request = Request.Builder()
        .url("https://yourapi.com/hidden-auth")
        .header("x-play-review-secret", "your-secret-key")
        .build()
}
```

**Official resources**

- [Requirements for providing login credentials for app access](https://support.google.com/googleplay/android-developer/answer/15748846?hl=en&sjid=2614470000031657771-NC)
- [Make your app available for review](https://playacademy.exceedlms.com/student/collection/260728/path/345815/activity/776873)

## Use Kinde feature flags to switch on review mode

You can use Kinde feature flags to dynamically enable hidden routes or functionality during the app review process—without needing to redeploy or hard-code conditions.

### Step 1: Create a feature flag

1. In **Kinde** go to **Settings > Feature Flags.**
2. Select **Add feature flag**. The **Add feature flag** window opens.
    
    ![feature flag top half of screen](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/59e501e6-77d6-4cf6-feed-b88a46a96f00/public)
    
3. Fill out the name, description, and key (e.g., `app_store_review_mode`).
4. Select **Boolean** as the flag type.
    
    ![feature flag screen bottom half in Kinde](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/abd5ae55-cb13-4467-0d4b-d7eae2d7f800/public)
    
5. Set the **Boolean definition** to `false` (default state).
6. Select **Save**.

You’ll toggle this flag to `true` during the review period, then back to `false` afterward.

### Step 2: Manage the flag in your backend

You’ll now use the flag in your backend logic to control access to a hidden login route.

**Example: Next.js API Route with Kinde Feature Flag:**

```tsx
import { NextResponse } from 'next/server';
import { getKindeServerSession } from '@kinde-oss/kinde-auth-nextjs/server';

export async function POST(req: Request) {
  const { getBooleanFlag } = getKindeServerSession();

  const isFeatureEnabled = await getBooleanFlag(
    'app_store_review_mode', // Your feature flag key
    false                        // Default value
  );

  if (isFeatureEnabled.value) {
    return NextResponse.json({ message: 'Hidden auth route accessed via feature flag!' });
  } else {
    return NextResponse.json({ error: 'Forbidden' }, { status: 403 });
  }
}

```

### Step 3: Toggle the flag during review

When you submit your app for review:

- Set the flag `app_store_review_mode` to **true** in the Kinde dashboard.
- After approval, set it back to **false** to hide the route again.

## You're ready to give app access to app store reviewers!

With these strategies, you can securely provide access to your app, for both Apple and Google Play reviewers, while keeping the login routes hidden from real users. 

Using tools like Kinde feature flags, deep links, and environment checks ensures your review process is smooth and secure.

## Need help?

Join the [Kinde community on Slack](https://join.slack.com/t/thekindecommunity/shared_invite/zt-1vyq8qilj-jFH5V27jfFnHk~BuBSU0ZA) or the [Kinde community on Discord](https://discord.com/invite/tw5ng5tK6V) for support and advice from our team and other legends working with Kinde.
