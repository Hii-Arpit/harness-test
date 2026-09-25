---
description: >-
  Integrate OpenFeature with Harness FME in your Dart applications to evaluate
  feature flags, manage contexts, and track events using a standardized SDK.
nodeTitle: OpenFeature Provider for the Dart Client SDK
inputFilePath: >-
  docs/feature-management-experimentation/sdks-and-infrastructure/openfeature/dart-sdk.md
originalUrl: >-
  https://developer.harness.io/docs/feature-management-experimentation/sdks-and-infrastructure/openfeature/dart-sdk/
---

# OpenFeature Provider for Dart

Integrate OpenFeature with Harness FME in your Dart and Flutter applications to evaluate feature flags using a standardized, vendor-agnostic SDK.

Integrate your Dart and Flutter applications with Harness FME using the Dart OpenFeature Provider, `split_openfeature_provider_dart_client`. This provider implements the OpenFeature specification and bridges the OpenFeature client-side API with the Harness FME Dart Client SDK (`splitio_client_side`).

This page walks you through installing, configuring, and using the Dart OpenFeature provider to evaluate feature flags in your application.

All of our SDKs are open source. Go to our [Dart OpenFeature Provider GitHub repository](https://github.com/splitio/split-openfeature-provider-dart-client) to see the source code, or view the package on [pub.dev](https://pub.dev/packages/split_openfeature_provider_dart_client).

### Before you begin <a href="#before-you-begin" id="before-you-begin"></a>

* A valid Harness FME client-side SDK key for your project
* Dart SDK v3.0.0 or later

### Version compatibility <a href="#version-compatibility" id="version-compatibility"></a>

| Component                                   | Minimum Version |
| -------------------------------------------- | ---------------- |
| `split_openfeature_provider_dart_client`     | ^0.1.0            |
| `openfeature_dart_client_sdk`                | ^0.0.1-beta.1     |
| `splitio_client_side`                        | ^1.1.0            |

### Install the provider and dependencies <a href="#install-the-provider-and-dependencies" id="install-the-provider-and-dependencies"></a>

All three packages below are required peer dependencies. Add them to your `pubspec.yaml` file:

```yaml
dependencies:
  split_openfeature_provider_dart_client: ^0.1.0
  openfeature_dart_client_sdk: ^0.0.1-beta.1
  splitio_client_side: ^1.1.0
```

### Initialize the provider <a href="#initialize-the-provider" id="initialize-the-provider"></a>

Register the Harness FME OpenFeature provider by using a `SplitFactory` instance.

```dart
import 'package:openfeature_dart_client_sdk/openfeature_dart_client_sdk.dart';
import 'package:split_openfeature_provider_dart_client/split_openfeature_provider_dart_client.dart';
import 'package:splitio_client_side/splitio_client_side.dart'
    show SplitFactory, SplitClientConfig;

Future<void> main() async {
  final splitFactory = SplitFactory.create(
    'CLIENT_SIDE_SDK_KEY',
    SplitClientConfig(),
    'TARGETING_KEY',
  );

  final provider = OpenFeatureSplitProvider(splitFactory);
  await OpenFeatureAPI.instance.setProviderAndWait(provider);

  final client = OpenFeatureAPI.instance.getClient();
}
```

### Construct an evaluation context <a href="#construct-an-evaluation-context" id="construct-an-evaluation-context"></a>

Provide an evaluation context with a targeting key to evaluate flags. The evaluation context passes targeting information such as user IDs, email addresses, or plan types for flag targeting.

```dart
await OpenFeatureAPI.instance.setEvaluationContextAndWait(
  EvaluationContext(targetingKey: 'TARGETING_KEY'),
);
```

### Evaluate flags <a href="#evaluate-flags" id="evaluate-flags"></a>

Use the standard OpenFeature client API to evaluate flags, rather than calling the underlying `splitio_client_side` SDK directly.

```dart
final showBanner = client.getBooleanValue('show_banner', false);
```

For more information, go to the [Harness FME Dart OpenFeature Provider GitHub repository](https://github.com/splitio/split-openfeature-provider-dart-client), or read more about the [OpenFeature specification](https://openfeature.dev/docs/reference/concepts/provider).

### Reference links <a href="#reference-links" id="reference-links"></a>

* Repo: [https://github.com/splitio/split-openfeature-provider-dart-client](https://github.com/splitio/split-openfeature-provider-dart-client)
* Issues: [https://github.com/splitio/split-openfeature-provider-dart-client/issues](https://github.com/splitio/split-openfeature-provider-dart-client/issues)
* Pub.dev: [https://pub.dev/packages/split_openfeature_provider_dart_client](https://pub.dev/packages/split_openfeature_provider_dart_client)
* OpenFeature spec: [https://openfeature.dev/docs/reference/concepts/provider](https://openfeature.dev/docs/reference/concepts/provider)

Questions or feedback? Reach out to your Harness account team, or open an issue in the GitHub repository above.