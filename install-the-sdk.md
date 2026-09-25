---
title: Install the FME SDK
sidebar_label: Install the FME SDK
sidebar_position: 2
redirect_from:
  - >-
    /docs/feature-management-experimentation/sdks-and-infrastructure/faqs-general-sdk/is-it-possible-to-call-getthreatment-function-without-passing-a-user-id
  - >-
    /docs/feature-management-experimentation/sdks-and-infrastructure/faqs-general-sdk/how-to-ensure-sdk-is-configured-to-handle-the-generated-impressions-and-events-load
  - >-
    /docs/feature-management-experimentation/getting-started/overview/install-the-sdk
nodeTitle: Install the FME SDK
inputFilePath: docs/feature-management-experimentation/getting-started/install-the-sdk.mdx
originalUrl: >-
  https://developer.harness.io/docs/feature-management-experimentation/getting-started/install-the-sdk/x
description: Learn how to install the Harness FME SDKs.
---

# Install the FME SDK

Our SDKs are designed to run at the application layer of your application and provide a secure, out-of-the-box way to control your experiments and feature flags.

Each SDK has two main functions:

* [Serve as a decision engine for your application](install-the-sdk.md#decision-engine)
* [Automatically capture what variants of your features your customers are served](install-the-sdk.md#capturing-what-your-customer-was-served)

### Harness FME SDKs <a href="#harness-fme-sdks" id="harness-fme-sdks"></a>

Harness FME SDKs are for specific languages or use cases. To choose the best SDK for your scenario, see the respective SDK documentation.

#### Client-side SDKs

<table data-view="cards"><thead><tr><th align="center"></th><th align="center"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td align="center"><img src="../../.gitbook/assets/image (1).png" alt="" data-size="original"></td><td align="center"><strong>Android SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/client-side-sdks/client-side-standard-sdks/android-sdk.md">android-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (35).png" alt="" data-size="original"></td><td align="center"><strong>Angular Utilities</strong></td><td><a href="../sdks-and-customer-deployed-components/client-side-sdks/client-side-standard-sdks/angular-utilities.md">angular-utilities.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (2).png" alt="" data-size="original"></td><td align="center"><strong>Browser SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/client-side-sdks/client-side-standard-sdks/browser-sdk.md">browser-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (97).svg" alt="" data-size="line"></td><td align="center"><strong>Dart SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/client-side-sdks/client-side-standard-sdks/dart-sdk.md">dart-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (36).png" alt="" data-size="original"></td><td align="center"><strong>Flutter Plugin</strong></td><td><a href="../sdks-and-customer-deployed-components/client-side-sdks/client-side-standard-sdks/flutter-plugin.md">flutter-plugin.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (3).png" alt="" data-size="original"></td><td align="center"><strong>iOS SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/client-side-sdks/client-side-standard-sdks/ios-sdk.md">ios-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (37).png" alt="" data-size="original"></td><td align="center"><strong>JavaScript SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/client-side-sdks/client-side-standard-sdks/javascript-sdk.md">javascript-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (38).png" alt="" data-size="original"></td><td align="center"><strong>React SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/client-side-sdks/client-side-standard-sdks/react-sdk.md">react-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (38).png" alt="" data-size="original"></td><td align="center"><strong>React Native SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/client-side-sdks/client-side-standard-sdks/react-native-sdk.md">react-native-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (39).png" alt="" data-size="original"></td><td align="center"><strong>Redux SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/client-side-sdks/client-side-standard-sdks/redux-sdk.md">redux-sdk.md</a></td></tr></tbody></table>

#### Client-side SDK Suites

<table data-view="cards"><thead><tr><th align="center"></th><th align="center"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td align="center"><img src="../../.gitbook/assets/image (1).png" alt="" data-size="original"></td><td align="center"><strong>Android Suite</strong></td><td><a href="../sdks-and-customer-deployed-components/client-side-sdk-suites/android-suite.md">android-suite.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (2).png" alt="" data-size="original"></td><td align="center"><strong>Browser Suite</strong></td><td><a href="../sdks-and-customer-deployed-components/client-side-sdk-suites/browser-suite.md">browser-suite.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (3).png" alt="" data-size="original"></td><td align="center"><strong>iOS Suite</strong></td><td><a href="../sdks-and-customer-deployed-components/client-side-sdk-suites/ios-suite.md">ios-suite.md</a></td></tr></tbody></table>

#### Client-side RUM Agents

<table data-view="cards"><thead><tr><th align="center"></th><th align="center"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td align="center"><img src="../../.gitbook/assets/image (1).png" alt="" data-size="original"></td><td align="center"><strong>Android Agents</strong></td><td><a href="../sdks-and-customer-deployed-components/client-side-agents/android-rum-agent.md">android-rum-agent.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (2).png" alt="" data-size="original"></td><td align="center"><strong>Browser Agents</strong></td><td><a href="../sdks-and-customer-deployed-components/client-side-agents/browser-rum-agent.md">browser-rum-agent.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (3).png" alt="" data-size="original"></td><td align="center"><strong>iOS Agents</strong></td><td><a href="../sdks-and-customer-deployed-components/client-side-agents/ios-rum-agent.md">ios-rum-agent.md</a></td></tr></tbody></table>

#### Server-side SDKs

<table data-view="cards"><thead><tr><th align="center"></th><th align="center"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td align="center"><img src="../../.gitbook/assets/image (40).png" alt="" data-size="line"></td><td align="center"><strong>Elixir Thin Client SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/server-side-sdks/elixir-thin-client-sdk.md">elixir-thin-client-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (41).png" alt="" data-size="original"></td><td align="center"><strong>Go SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/server-side-sdks/go-sdk.md">go-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (42).png" alt="" data-size="line"></td><td align="center"><strong>Java SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/server-side-sdks/java-sdk.md">java-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (43).png" alt="" data-size="original"></td><td align="center"><strong>.NET SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/server-side-sdks/net-sdk.md">net-sdk.md</a></td></tr><tr><td align="center"><picture><source srcset="../../.gitbook/assets/nodejs-dark.svg" media="(prefers-color-scheme: dark)"><img src="../../.gitbook/assets/node-js.svg" alt="" data-size="original"></picture></td><td align="center"><strong>NodeJS SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/server-side-sdks/nodejs-sdk.md">nodejs-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (45).png" alt="" data-size="original"></td><td align="center"><strong>PHP SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/server-side-sdks/php-sdk.md">php-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (45).png" alt="" data-size="original"></td><td align="center"><strong>PHP Thin Client SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/server-side-sdks/php-thin-client-sdk.md">php-thin-client-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/python-logo.png" alt="" data-size="original"></td><td align="center"><strong>Python SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/server-side-sdks/python-sdk.md">python-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/ruby.png" alt=""></td><td align="center"><strong>Ruby SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/server-side-sdks/ruby-sdk.md">ruby-sdk.md</a></td></tr></tbody></table>

#### Optional Infrastructure

<table data-view="cards"><thead><tr><th align="center"></th><th align="center"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td align="center"><img src="../../.gitbook/assets/image (15).png" alt="" data-size="line"></td><td align="center"><strong>Split Daemon (splitd)</strong></td><td><a href="../sdks-and-customer-deployed-components/customer-deployed-components/split-daemon-splitd.md">split-daemon-splitd.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (15).png" alt="" data-size="line"></td><td align="center"><strong>Split Evaluator</strong></td><td><a href="../sdks-and-customer-deployed-components/customer-deployed-components/split-evaluator.md">split-evaluator.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (15).png" alt="" data-size="line"></td><td align="center"><strong>Split Proxy</strong></td><td><a href="../sdks-and-customer-deployed-components/customer-deployed-components/split-proxy.md">split-proxy.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (15).png" alt="" data-size="line"></td><td align="center"><strong>Split Synchronizer</strong></td><td><a href="../sdks-and-customer-deployed-components/customer-deployed-components/split-synchronizer.md">split-synchronizer.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (37).png" alt="" data-size="original"></td><td align="center"><strong>Split JavaScript Synchronizer Tools</strong></td><td><a href="../sdks-and-customer-deployed-components/customer-deployed-components/split-javascript-synchronizer-tools.md">split-javascript-synchronizer-tools.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (46).png" alt="" data-size="line"></td><td align="center"><strong>Harness Proxy</strong></td><td><a href="../sdks-and-customer-deployed-components/customer-deployed-components/harness-proxy.md">harness-proxy.md</a></td></tr></tbody></table>

#### OpenFeature Providers

<table data-view="cards"><thead><tr><th align="center"></th><th align="center"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td align="center"><img src="../../.gitbook/assets/image (1).png" alt="" data-size="original"></td><td align="center"><strong>Android SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/openfeature-providers/android-sdk.md">android-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (3).png" alt="" data-size="original"></td><td align="center"><strong>iOS SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/openfeature-providers/ios-sdk.md">ios-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (2).png" alt="" data-size="original"></td><td align="center"><strong>Web SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/openfeature-providers/web-sdk.md">web-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (97).svg" alt="" data-size="line"></td><td align="center"><strong>Dart SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/openfeature-providers/dart-sdk.md">dart-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (35).png" alt="" data-size="original"></td><td align="center"><strong>Angular</strong></td><td><a href="../sdks-and-customer-deployed-components/openfeature-providers/angular-sdk.md">angular-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (38).png" alt="" data-size="original"></td><td align="center"><strong>React</strong></td><td><a href="../sdks-and-customer-deployed-components/openfeature-providers/react-sdk.md">react-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (42).png" alt="" data-size="line"></td><td align="center"><strong>Java SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/openfeature-providers/java-sdk.md">java-sdk.md</a></td></tr><tr><td align="center"><picture><source srcset="../../.gitbook/assets/nodejs-dark.svg" media="(prefers-color-scheme: dark)"><img src="../../.gitbook/assets/node-js.svg" alt="" data-size="original"></picture></td><td align="center"><strong>Node.js</strong></td><td><a href="../sdks-and-customer-deployed-components/openfeature-providers/nodejs-sdk.md">nodejs-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/python-logo.png" alt=""></td><td align="center"><strong>Python SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/openfeature-providers/python-sdk.md">python-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (43).png" alt="" data-size="original"></td><td align="center"><strong>.NET SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/openfeature-providers/net-sdk.md">net-sdk.md</a></td></tr><tr><td align="center"><img src="../../.gitbook/assets/image (41).png" alt="" data-size="original"></td><td align="center"><strong>Go SDK</strong></td><td><a href="../sdks-and-customer-deployed-components/openfeature-providers/go-sdk.md">go-sdk.md</a></td></tr></tbody></table>

### Decision engine <a href="#decision-engine" id="decision-engine"></a>

When you set up rules for your feature flags and experiments to be targeted to subsets of your customer base in Harness FME (e.g., run a 50/50 test of a new home page on customers in New York), our SDKs automatically download down these rules and maintain a local copy of them on your machines. From there, our SDKs then take care of keeping themselves up to date by periodically checking for any changes to the rules that are made in the Harness FME user interface.

When your application then loads for your customers, you can simply ask the SDK via a method called `getTreatment` to decide what variant of a feature the customer should see.

Since the SDK is maintaining a local copy of the rules that govern your features and experiments, it can simply reference that copy of your rules and make the decision to serve "on" or "off" to your customer without having to make a single remote call. From there, you can take the decision returned by our SDK and use that information to serve up the proper experience to your customer.

The `getTreatment` function requires a customer ID, which is usually a hash representation of the current session's customer. The SDK uses the customer ID when the feature flag includes percentage-based targeting rules (for example: 50% `"On"` and 50% `"Off"`). In this case, this is not relevant since we are assigning 100% of a single treatment. However, the SDK still requires the customer ID to calculate the treatment.

If the implementation will not use percentage-based treatments, then apply a dummy customer ID with any string value.

In this manner, our SDK is able to abstract out any need to hardcode this type of decision making in your application.

### Capturing what your customer was served <a href="#capturing-what-your-customer-was-served" id="capturing-what-your-customer-was-served"></a>

Each time our SDK makes a decision of what your customer should be served, it automatically takes that information and queues it up on your machines. The SDK then takes care of all the work in passing this information up to Harness FME in the background without ever slowing down your application.

By capturing this information, you can easily understand what customers are being served and set the basis for being able to properly measure your experiments.

### Handling impressions and events load <a href="#handling-impressions-and-events-load" id="handling-impressions-and-events-load"></a>

{% hint style="danger" %}
**WHAT HAPPENS IF THE SDK CANNOT KEEP UP WITH THE INCOMING IMPRESSIONS AND EVENTS LOAD?**

The SDK will post the queue content when it becomes full. However, if the load consistently exceeds capacity, the queue will repeatedly fill and be posted, clearing it. Meanwhile, new impressions or events generated during this period are not stored because the queue is full, resulting in data loss.

Harness recommends verifying these SDK configuration parameters against your production generation rates for impressions and events, ensuring the SDK is properly configured to handle the load.
{% endhint %}

By default, all FME SDKs have configuration parameters that allow them to process a heavy load of generated Impressions and Events. These default parameter values are documented in the respective SDK documentation.

In the [Java SDK](../sdks-and-customer-deployed-components/server-side-sdks/java-sdk.md#configuration), for example, the Configuration section includes parameters and default values for posting impressions:

* `impressionsRefreshRate = 60` (seconds)
* `impressionsQueueSize = 30k`

These settings mean the SDK can handle up to 30,000 impressions (each generated by a single `getTreatment` call) every 60 seconds.

If your application generates more than 30,000 impressions per minute, you will need to adjust these parameters. For instance, generating 60,000 impressions per minute could be handled by setting:

* `impressionsRefreshRate = 20` or `impressionsQueueSize = 70k`

Harness recommends providing some buffer so the SDK can handle the actual load. If the queue size is reached, the SDK will attempt to post its contents regardless of the impression post thread’s scheduled run time.

The same concept applies to events created using the SDK’s `track()` method.

The relevant Java SDK parameters for posting events are:

* `eventFlushIntervalInMillis = 30000` (30 seconds)
* `eventsQueueSize = 500`

These values allow the Java SDK to handle up to 1,000 events per minute.

{% @harness-feedback/feedback %}
