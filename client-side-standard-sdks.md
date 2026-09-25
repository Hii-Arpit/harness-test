---
title: Client-side Standard SDKs
redirect_from:
  - >-
    /docs/feature-management-experimentation/sdks-and-infrastructure/faqs-client-side-sdks/ios-android-browser-sdk-fme-changes-roll-out-slowly-to-user-devices
  - >-
    /docs/feature-management-experimentation/sdks-and-infrastructure/faqs-client-side-sdks/ios-android-browser-sdk-does-the-sdk-cache-expire
  - >-
    /docs/feature-management-experimentation/sdks-and-infrastructure/faqs-client-side-sdks/ios-and-android-sdk-how-to-initialize-for-multiple-user-ids
  - >-
    /docs/feature-management-experimentation/sdks-and-infrastructure/faqs-client-side-sdks/
nodeTitle: Client-side Standard SDKs
inputFilePath: >-
  docs/feature-management-experimentation/sdks-and-infrastructure/client-side-sdks/index.md
originalUrl: >-
  https://developer.harness.io/docs/feature-management-experimentation/sdks-and-infrastructure/client-side-sdks/index/
description: Learn about Harness FME client-side standard SDKs.
---


# Client-side Standard SDKs

Harness FME provides client-side SDKs that let you evaluate feature flags, run experiments, and deliver personalized experiences directly in your application's frontend.

These SDKs are optimized for real-time updates and minimal latency, ensuring that your users always experience the most up-to-date feature set.

Standard SDKs evaluate feature flags locally on the device. They pull rollout rules and segment-membership data from FME cloud, cache them, and run `getTreatment` in-process. **Standard SDKs are the recommended path for most applications**: user attributes stay on the device, and once the SDK is ready it can evaluate any flag for any target.

If you have not chosen between local and remote evaluation yet, see [Choosing an evaluation mode](../evaluation-modes.md).

### Certificate pinning <a href="#certificate-pinning" id="certificate-pinning"></a>

If your application uses certificate pinning with Harness FME SDKs, you may need to [update your configuration](../../examples/certificate-pinning-migration.md) to support streaming infrastructure migrations and future SDK capabilities.

For platform-specific configuration instructions, see [Certificate Pinning for Android](android-sdk.md#certificate-pinning) and [Certificate Pinning for iOS](ios-sdk.md#certificate-pinning).

### Does the SDK cache expire? <a href="#does-the-sdk-cache-expire" id="does-the-sdk-cache-expire"></a>

The Split mobile (iOS and Android) and JavaScript Browser SDKs download a local cache of flag data and store it in the device or browser file system. This cache has a default expiration period of 10 days in most SDKs, after which the SDK refreshes the cache from scratch.

The default and configuration options differ slightly by SDK:

* JavaScript SDK (v11.2.0 and later): Default expiration of 10 days, configurable via the `LOCALSTORAGE` setting.
* Browser SDK (v1.2.0 and later): Default expiration of 10 days, configurable via the `InLocalStorage` setting. See [Configure cache behavior for the SDK](browser-sdk.md#configure-cache-behavior).
* Android SDK (v5.3.0 and later): Default expiration of 10 days, configurable via the `rolloutCacheConfiguration` setting.
* iOS SDK (v3.3.0 and later): Default expiration of 10 days, configurable via the `rolloutCacheConfiguration` setting.

All SDKs continue to store impressions and events for up to 90 days. After that period, the cache is considered stale and may be purged.

### How to initialize for multiple user IDs? <a href="#how-to-initialize-for-multiple-user-ids" id="how-to-initialize-for-multiple-user-ids"></a>

The JavaScript SDK supports initializing [multiple client objects](javascript-sdk.md#instantiate-multiple-sdk-clients) from the same SDK factory, each with a unique user key (or user ID):

```javascript
client1 = factory.client("user_id1");
client2 = factory.client("user_id2");
```

For mobile SDKs, see the [iOS](ios-sdk.md#instantiate-multiple-sdk-clients) and [Android SDK documentation](android-sdk.md#instantiate-multiple-sdk-clients).

{% hint style="info" %}
**FEATURE FLAG UPDATE TIMING**

When you make changes to a feature flag in the Harness UI, mobile (iOS, Android) and JavaScript Browser SDKs may not reflect the update immediately for all users.

Updates roll out gradually (some devices sync within the first day, with more devices updating in the following days) unless your code waits for the `SDK_READY` event. If your app only uses cached data from the previous session (triggered by `SDK_READY_FROM_CACHE`), users may continue to see the old values until their cache syncs.

To ensure the latest changes are reflected, calculate treatments after the `SDK_READY` event fires, and update them if the value changes after initial load.
{% endhint %}

### Get started <a href="#get-started" id="get-started"></a>

Select a platform to start integrating FME into your client application.

<table data-view="cards"><thead><tr><th align="center"></th><th align="center"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td align="center"><img src="../../../../.gitbook/assets/image (1).png" alt="" data-size="original"></td><td align="center"><strong>Android SDK</strong></td><td><a href="android-sdk.md">android-sdk.md</a></td></tr><tr><td align="center"><img src="../../../../.gitbook/assets/image (35).png" alt="" data-size="original"></td><td align="center"><strong>Angular Utilities</strong></td><td><a href="angular-utilities.md">angular-utilities.md</a></td></tr><tr><td align="center"><img src="../../../../.gitbook/assets/image (2).png" alt="" data-size="original"></td><td align="center"><strong>Browser SDK</strong></td><td></td></tr><tr><td align="center"><img src="../../../../.gitbook/assets/image (97).svg" alt="" data-size="line"></td><td align="center"><strong>Dart SDK</strong></td><td><a href="dart-sdk.md">dart-sdk.md</a></td></tr><tr><td align="center"><img src="../../../../.gitbook/assets/image (36).png" alt="" data-size="original"></td><td align="center"><strong>Flutter Plugin</strong></td><td><a href="flutter-plugin.md">flutter-plugin.md</a></td></tr><tr><td align="center"><img src="../../../../.gitbook/assets/image (3).png" alt="" data-size="original"></td><td align="center"><strong>iOS SDK</strong></td><td><a href="ios-sdk.md">ios-sdk.md</a></td></tr><tr><td align="center"><img src="../../../../.gitbook/assets/image (37).png" alt="" data-size="original"></td><td align="center"><strong>JavaScript SDK</strong></td><td><a href="javascript-sdk.md">javascript-sdk.md</a></td></tr><tr><td align="center"><img src="../../../../.gitbook/assets/image (38).png" alt="" data-size="original"></td><td align="center"><strong>React SDK</strong></td><td><a href="react-sdk.md">react-sdk.md</a></td></tr><tr><td align="center"><img src="../../../../.gitbook/assets/image (38).png" alt="" data-size="original"></td><td align="center"><strong>React Native SDK</strong></td><td><a href="react-native-sdk.md">react-native-sdk.md</a></td></tr><tr><td align="center"><img src="../../../../.gitbook/assets/image (39).png" alt="" data-size="original"></td><td align="center"><strong>Redux SDK</strong></td><td><a href="redux-sdk.md">redux-sdk.md</a></td></tr></tbody></table>

{% @harness-feedback/feedback %}
