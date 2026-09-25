---
title: OpenFeature Providers
id: index
slug: /feature-management-experimentation/sdks-and-infrastructure/openfeature
sidebar_position: 1
nodeTitle: OpenFeature Providers
inputFilePath: >-
  docs/feature-management-experimentation/sdks-and-infrastructure/openfeature/index.md
originalUrl: >-
  https://developer.harness.io/docs/feature-management-experimentation/sdks-and-infrastructure/openfeature/index/
description: Learn about using Harness OpenFeature providers for feature management.
---

# OpenFeature Providers

[OpenFeature](https://openfeature.dev/docs/reference/intro) offers a standardized, vendor-agnostic SDK for feature flagging that can integrate with a variety of third-party providers. Whether you're using an open-source or commercial solution, self-hosted or cloud-hosted, OpenFeature gives developers a unified API for consistent feature flag evaluation.

OpenFeature SDKs provide flexible abstractions that make it easy to integrate feature flags into any application. Within your application, the feature flagging client uses the OpenFeature SDK to evaluate feature flags through the Evaluation API.

Each flag evaluation passes an evaluation context, which provides relevant data about the application or user.

```mermaid
flowchart LR
  %% Outer app box
  subgraph YourApp["Your App"]
    style YourApp fill:#D0E4FF,stroke:#0000FF,stroke-width:2px

    %% Feature Flagging Client (yellow dotted box)
    subgraph FeatureFlagClient["Feature Flagging Client"]
      style FeatureFlagClient stroke:#FFD700,stroke-dasharray: 5 5

      %% Puzzle piece: OpenFeature SDK
      subgraph OpenFeatureSDK["OpenFeature SDK"]
        style OpenFeatureSDK fill:#FFFF99,stroke:#FFD700

        %% Flag Evaluation API inside SDK
        FlagEvalAPI["Flag Evaluation API"]
      end

      %% OpenFeature Provider in second half of puzzle piece
      OpenFeatureProvider["Harness FME <br> OpenFeature Provider"]
    end

    %% Arrows from Flag Eval through Eval Context directly into FlagEvalAPI
    FlagEval1["Flag Eval"] --> EvalCtx1["Eval Context"] --> FlagEvalAPI
    FlagEval2["Flag Eval"] --> EvalCtx2["Eval Context"] --> FlagEvalAPI
  end

  %% External Harness FME Service in cloud
  HarnessFME["Harness FME Service"]:::cloudStyle

  %% Bidirectional arrow between OpenFeature Provider and Harness FME
  OpenFeatureProvider <--> HarnessFME

  %% Styling for cloud
  classDef cloudStyle fill:#A4E5A4,stroke:#2E8B57,stroke-width:2px,shape:cloud
```

<br>

The OpenFeature Provider wraps the Harness FME SDK, bridging the OpenFeature SDK with the Harness Feature Management & Experimentation (FME) service. The provider maps OpenFeature's standardized interface to the FME SDK, which handles communication with Harness services to evaluate feature flags and retrieve configuration updates.

### Use OpenFeature SDKs <a href="#use-openfeature-sdks" id="use-openfeature-sdks"></a>

Harness FME offers official OpenFeature providers for the following SDKs.

<table data-view="cards"><thead><tr><th align="center"></th><th align="center"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td align="center"><img src="../../../.gitbook/assets/image (1).png" alt="" data-size="original"></td><td align="center"><strong>Android SDK</strong></td><td><a href="android-sdk.md">android-sdk.md</a></td></tr><tr><td align="center"><img src="../../../.gitbook/assets/image (3).png" alt="" data-size="original"></td><td align="center"><strong>iOS SDK</strong></td><td><a href="ios-sdk.md">ios-sdk.md</a></td></tr><tr><td align="center"><img src="../../../.gitbook/assets/image (2).png" alt="" data-size="original"></td><td align="center"><strong>Web SDK</strong></td><td><a href="web-sdk.md">web-sdk.md</a></td></tr><tr><td align="center"><img src="../../../.gitbook/assets/image (97).svg" alt="" data-size="original"></td><td align="center"><strong>Dart SDK</strong></td><td><a href="dart-sdk.md">dart-sdk.md</a></td></tr><tr><td align="center"><img src="../../../.gitbook/assets/image (35).png" alt="" data-size="original"></td><td align="center"><strong>Angular</strong></td><td><a href="angular-sdk.md">angular-sdk.md</a></td></tr><tr><td align="center"><img src="../../../.gitbook/assets/image (38).png" alt="" data-size="original"></td><td align="center"><strong>React</strong></td><td><a href="react-sdk.md">react-sdk.md</a></td></tr><tr><td align="center"><img src="../../../.gitbook/assets/image (42).png" alt="" data-size="line"></td><td align="center"><strong>Java SDK</strong></td><td><a href="java-sdk.md">java-sdk.md</a></td></tr><tr><td align="center"><img src="../../../.gitbook/assets/node-js.svg" alt="" data-size="original"></td><td align="center"><strong>Node.js</strong></td><td><a href="nodejs-sdk.md">nodejs-sdk.md</a></td></tr><tr><td align="center"><img src="../../../.gitbook/assets/python-logo.png" alt=""></td><td align="center"><strong>Python SDK</strong></td><td><a href="python-sdk.md">python-sdk.md</a></td></tr><tr><td align="center"><img src="../../../.gitbook/assets/image (43).png" alt="" data-size="original"></td><td align="center"><strong>.NET SDK</strong></td><td><a href="net-sdk.md">net-sdk.md</a></td></tr><tr><td align="center"><img src="../../../.gitbook/assets/image (41).png" alt="" data-size="original"></td><td align="center"><strong>Go SDK</strong></td><td><a href="go-sdk.md">go-sdk.md</a></td></tr></tbody></table>

You can use these providers instead of the Harness FME SDK in your application.

{% @harness-feedback/feedback %}
