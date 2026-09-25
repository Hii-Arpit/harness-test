---
title: Versioning policy for SDKs and customer-deployed components
sidebar_label: Versioning Policy
sidebar_position: 1
redirect_from:
  - >-
    /docs/feature-management-experimentation/sdks-and-infrastructure/sdk-overview/sdk-versioning-policy
  - >-
    /docs/feature-management-experimentation/sdks-and-infrastructure/sdk-overview/versioning-policy-for-sdks-and-customer-deployed-components
nodeTitle: Versioning policy for SDKs and customer-deployed components
inputFilePath: >-
  docs/feature-management-experimentation/sdks-and-infrastructure/versioning-policy.md
originalUrl: >-
  https://developer.harness.io/docs/feature-management-experimentation/sdks-and-infrastructure/versioning-policy/
---


# Versioning policy for SDKs and customer-deployed components

Harness FME versions its SDKs and customer-deployed components (such as the Split Proxy, Evaluator, Synchronizer, and Split Daemon) according to semantic versioning industry standards. FME actively supports major versions of these components for 12 months following their release.

{% hint style="info" %}
The versioning and support policy described here applies to all SDKs and customer-deployed components maintained by FME. This includes the Split Proxy, Evaluator, Synchronizer, Split Daemon (SplitD), JavaScript synchronizer tools, and any new tools released in the future.
{% endhint %}

### What is semantic versioning? <a href="#what-is-semantic-versioning" id="what-is-semantic-versioning"></a>

Semantic versioning establishes a standard to uniquely name a particular release of an artifact. With semantic versioning, the unique label is made up by three components: `major` version number, `minor` version number, and `patch` version number which assembled and separated by a period `.`. The following summary is extracted from the specification referenced above.

When we release a new version of our SDKs, we increment the major, minor, or patch number. Which component is incremented depends on the change introduced and are separated by periods.

As is conventional in semantic versioning, we increment each according to the descriptions below:

* `Major` version: when we make backwards incompatible API changes.
* `Minor` version: when we add backwards compatible new functionality.
* `Patch` version: when we make backwards compatible bug fixes.

Additional labels for pre-release candidates and build metadata are available as extensions. These vary on a per language basis. Examples include `x.x.x-rcx`, `x.x.x-canary.x`, and `x.x.x.pre.rcx`.

For more information, see [Semantic Versioning 2.0](https://semver.org).

### Adding new functionality <a href="#adding-new-functionality" id="adding-new-functionality"></a>

When Harness FME introduces new functionality, it qualifies as a minor release. If that functionality is a breaking change or represents an API compatibility change, it qualifies as a major version change.

### Fixing a bug <a href="#fixing-a-bug" id="fixing-a-bug"></a>

Bug fixes that preserve compatibility are released as a patch version. Depending on the invasiveness of the fix, the versioning is incremented as minor or major.

### Version support <a href="#version-support" id="version-support"></a>

FME supports and patches prior major releases of SDKs and customer-deployed components (such as the Split Proxy, Evaluator, Synchronizer, and Split Daemon) for up to 12 months following the version release date. If 12 months has elapsed, support and the Harness FME SDK engineering team may ask you to first upgrade to the current major release before attempting to patch old versions.

### Supported versions and end-of-support dates <a href="#supported-versions-and-end-of-support-dates" id="supported-versions-and-end-of-support-dates"></a>

Harness FME tracks support at the major version level. All patch and minor releases under the same major version share the same support window.

For example, versions `10.0.0` through `10.20.1` are all considered part of major release `10.x` and will have the same end-of-support date.

#### Server-side SDKs <a href="#server-side-sdks" id="server-side-sdks"></a>

{% tabs %}
{% tab title=".NET SDK" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `7.0.0` | 2022-05-26 | Actively supported |
{% endtab %}

{% tab title="Elixir Thin Client SDK" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `1.0.0` | 2025-02-25 | Actively supported |
| `0.0.0` | 2025-01-21 | 2026-01-27         |
{% endtab %}

{% tab title="Go SDK" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `6.0.0` | 2020-10-06 | Actively supported |
{% endtab %}

{% tab title="Java SDK" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `4.0.0` | 2020-08-19 | Actively supported |
{% endtab %}

{% tab title="Node.js SDK" %}
| Version  | GA Date    | Support End Date   |
| -------- | ---------- | ------------------ |
| `11.0.0` | 2024-11-01 | Actively supported |
| `10.0.0` | 2018-02-26 | 2025-01-04         |
{% endtab %}

{% tab title="PHP SDK" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `7.0.0` | 2021-11-23 | Actively supported |
{% endtab %}

{% tab title="PHP Thin Client SDK" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `2.0.0` | 2025-04-14 | Actively supported |
| `1.1.0` | 2023-09-06 | 2025-01-25         |
{% endtab %}

{% tab title="Python SDK" %}
| Version  | GA Date    | Support End Date   |
| -------- | ---------- | ------------------ |
| `10.0.0` | 2024-06-27 | Actively supported |
| `9.0.0`  | 2021-05-03 | 2024-02-15         |
{% endtab %}

{% tab title="Ruby SDK" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `8.0.0` | 2022-05-10 | Actively supported |
{% endtab %}
{% endtabs %}

#### Client-side SDKs <a href="#client-side-sdks" id="client-side-sdks"></a>

{% tabs %}
{% tab title="Android SDK" %}
| Version | General Availability Date | Support End Date   |
| ------- | ------------------------- | ------------------ |
| `5.0.0` | 2024-11-01                | Actively supported |
| `4.0.0` | 2024-01-04                | 2025-11-01         |
{% endtab %}

{% tab title="Angular SDK" %}
| Version | General Availability Date | Support End Date   |
| ------- | ------------------------- | ------------------ |
| `3.0.0` | 2024-05-17                | Actively supported |
| `2.0.1` | 2024-03-11                | 2024-05-17         |
{% endtab %}

{% tab title="Browser SDK" %}
| Version | General Availability Date | Support End Date   |
| ------- | ------------------------- | ------------------ |
| `1.0.0` | 2024-11-01                | Actively supported |
| `0.1.0` | 2021-03-30                | 2024-11-01         |
{% endtab %}

{% tab title="Flutter SDK" %}
| Version | General Availability Date | Support End Date   |
| ------- | ------------------------- | ------------------ |
| `0.1.0` | 2022-08-03                | Actively supported |
{% endtab %}

{% tab title="iOS SDK" %}
| Version | General Availability Date | Support End Date   |
| ------- | ------------------------- | ------------------ |
| `3.0.0` | 2024-11-01                | Actively supported |
| `2.0.0` | 2019-02-01                | 2024-11-01         |
{% endtab %}

{% tab title="JavaScript SDK" %}
| Version  | General Availability Date | Support End Date   |
| -------- | ------------------------- | ------------------ |
| `11.0.0` | 2024-11-01                | Actively supported |
| `10.0.0` | 2018-02-26                | 2025-11-01         |
{% endtab %}

{% tab title="React SDK" %}
| Version | General Availability Date | Support End Date   |
| ------- | ------------------------- | ------------------ |
| `2.0.0` | 2024-11-01                | Actively supported |
| `1.0.0` | 2020-01-24                | 2025-04-15         |
{% endtab %}

{% tab title="React Native SDK" %}
| Version | General Availability Date | Support End Date   |
| ------- | ------------------------- | ------------------ |
| `1.0.0` | 2024-11-01                | Actively supported |
| `0.0.1` | 2021-07-29                | 2025-11-01         |
{% endtab %}

{% tab title="Redux SDK" %}
| Version | General Availability Date | Support End Date   |
| ------- | ------------------------- | ------------------ |
| `2.0.0` | 2024-11-14                | Actively supported |
| `1.0.0` | 2020-01-24                | 2025-04-01         |
{% endtab %}

{% tab title="Dart SDK" %}
| Version | General Availability Date | Support End Date   |
| ------- | ------------------------- | ------------------ |
| `1.1.0` | 2026-09-09                | Actively supported |
{% endtab %}
{% endtabs %}

#### Customer-deployed components <a href="#customer-deployed-components" id="customer-deployed-components"></a>

{% tabs %}
{% tab title="Split Daemon (SplitD)" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `1.0.1` | 2023-09-05 | Actively supported |
{% endtab %}

{% tab title="Split Evaluator" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `2.0.0` | 2019-09-23 | Actively supported |
{% endtab %}

{% tab title="Split JavaScript Synchronizer" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `1.0.0` | 2025-05-28 | Actively supported |
| `0.1.0` | 2022-01-11 | 2024-02-29         |
{% endtab %}

{% tab title="Split Synchronizer/Proxy" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `5.0.0` | 2021-11-01 | Actively supported |
{% endtab %}
{% endtabs %}

#### OpenFeature providers <a href="#openfeature-providers" id="openfeature-providers"></a>

{% tabs %}
{% tab title="Android SDK" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `1.0.0` | 2025-09-30 | Actively supported |
{% endtab %}

{% tab title="iOS SDK" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `1.0.0` | 2023-10-31 | Actively supported |
{% endtab %}

{% tab title="Web SDK" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `1.0.0` | 2025-09-30 | Actively supported |
{% endtab %}

{% tab title="Angular" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `1.0.0` | 2023-09-30 | Actively supported |
{% endtab %}

{% tab title="React" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `1.0.0` | 2023-09-30 | Actively supported |
{% endtab %}

{% tab title="Java SDK" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `1.0.0` | 2025-10-07 | Actively supported |
{% endtab %}

{% tab title="Node.js SDK" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `1.0.0` | 2023-04-27 | Actively supported |
{% endtab %}

{% tab title="Python SDK" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `1.0.0` | 2023-11-10 | Actively supported |
{% endtab %}

{% tab title=".NET SDK" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `2.0.0` | 2023-11-12 | Actively supported |
{% endtab %}

{% tab title="Go SDK" %}
| Version | GA Date    | Support End Date   |
| ------- | ---------- | ------------------ |
| `2.0.0` | 2026-03-20 | Actively supported |
{% endtab %}

{% tab title="Dart SDK" %}
| Version | General Availability Date | Support End Date   |
| ------- | ------------------------- | ------------------ |
| `1.0.0` | 2026-09-15                | Actively supported |

{% endtabs %}

{% @harness-feedback/feedback %}
