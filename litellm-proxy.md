---
description: >-
  Emit GenAI traces to Cloud & AI Cost Management from a LiteLLM proxy using its
  built-in OpenTelemetry callback.
tags:
  - cloud-cost-management
  - ai-cost-management
---


# LiteLLM Proxy

"LiteLLM" refers to two different products, and how you send its cost data to Harness depends on which one you run:

* **LiteLLM proxy:** A standalone server that routes LLM traffic for your applications. It has OpenTelemetry export built in, so you instrument the proxy once and every call routed through it emits a trace, with no application code changes. **This page covers the proxy.**
* **LiteLLM SDK:** The Python library called directly in application code (`import litellm`). There is no proxy to configure, so you instrument the library itself. Go to the [Harness SDK integration](../harness-sdk.md#add-instrumentation) to set it up. **This page does not cover the SDK.**

The proxy is configured the same way whether or not it was already exporting traces:

| Your setup                                                              | What you do                                                                                                                                                                         |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| The proxy already exports OpenTelemetry traces to another backend.      | Enable the `otel` callback if it is not on already, then change the endpoint and headers to the Harness values in the setup [below](litellm-proxy.md#instrument-the-litellm-proxy). |
| The proxy does not export traces yet.                                   | Enable the `otel` callback and set the Harness endpoint variables, as shown in the setup [below](litellm-proxy.md#instrument-the-litellm-proxy).                                    |
| You call the LiteLLM **SDK** in Python (`import litellm`), not a proxy. | Go to the [Harness SDK integration](../harness-sdk.md#add-instrumentation) instead.                                                                                                 |

Before you start, generate an ingestion token and connect a provider. Go to the [AI Cost Management Quickstart](../../../new-to-cacm/ai-cost-management/quickstart.md) for those steps. Replace `<YOUR_TOKEN>` below with that token and adjust the endpoint for your Harness cluster.

{% hint style="info" %}
**GENAI SEMANTIC CONVENTIONS REQUIRED**

Cost is calculated from OpenTelemetry traces with GenAI semantic conventions. Go to the [GenAI Span Attribute Reference](../../../references/ai-cost-management/genai-span-attribute-reference.md) to review the attributes CACM reads.
{% endhint %}

***

### Instrument the LiteLLM proxy <a href="#instrument-the-litellm-proxy" id="instrument-the-litellm-proxy"></a>

[LiteLLM proxy](https://docs.litellm.ai/docs/observability/opentelemetry_integration) emits OTel traces for every LLM call via its `otel` callback.

Enable the callback in `config.yaml`:

```yaml
litellm_settings:
  callbacks: ["otel"]
```

Set these environment variables on the proxy, then restart it:

```bash
export OTEL_EXPORTER="otlp_http"
export OTEL_ENDPOINT="https://app.harness.io/udp-ingest/otel/v1/traces"
export OTEL_HEADERS="Authorization=Bearer <YOUR_TOKEN>"
```

{% hint style="info" %}
**LITELLM PROXY USES ITS OWN VARIABLE NAMES**

`OTEL_EXPORTER`, `OTEL_ENDPOINT`, and `OTEL_HEADERS` are LiteLLM-proxy-specific variable names, not the standard OpenTelemetry SDK variables (`OTEL_EXPORTER_OTLP_ENDPOINT`, `OTEL_EXPORTER_OTLP_HEADERS`) used for other integrations. Use these exact names only for the LiteLLM proxy.
{% endhint %}

{% hint style="info" %}
**ALTERNATIVE CONFIGURATION WITH STANDARD OTEL VARIABLES**

Some LiteLLM proxy versions emit traces with the `otel` callback and the standard OpenTelemetry exporter variables instead of the proxy-specific names above. If traces do not appear with the variables above, enable the callback and use the standard variables instead:

```bash
export OTEL_EXPORTER_OTLP_ENDPOINT=https://app.harness.io/udp-ingest/otel
export OTEL_EXPORTER_OTLP_HEADERS="Authorization=Bearer <YOUR_TOKEN>"
export OTEL_TRACES_EXPORTER=otlp
export OTEL_EXPORTER_OTLP_PROTOCOL=http/protobuf
```
{% endhint %}

Once the proxy restarts, every request routed through it emits traces to Harness.

**What this produces:**

* One span per LLM call routed through the proxy.
* Span attributes: `gen_ai.system`, `gen_ai.request.model`, token usage.
* Cost calculated from token counts and model pricing.

{% hint style="info" %}
**PROXY INSTRUMENTATION COVERS ONLY PROXIED TRAFFIC**

This instruments the proxy, not the application code. If the application makes direct LLM calls that bypass the proxy, those calls do not appear in telemetry. Instrument those calls with the [Harness SDK](../harness-sdk.md).
{% endhint %}

***

### Verify traces in Cost Explorer <a href="#verify-traces-in-cost-explorer" id="verify-traces-in-cost-explorer"></a>

1. Send a request through the proxy so traces flow. They usually appear within a few minutes; allow up to about 20 minutes.
2. Go to **Cloud & AI Cost Management** > **Cost Explorer**.
3. Select the **AI Traces** view or group by **Service Name**, and find your service.
4. Select a service row to open the **Service Traces** drawer and inspect the span waterfall.

***

### Next steps <a href="#next-steps" id="next-steps"></a>

* Go to [Supported Providers and Frameworks](../../../references/ai-cost-management/supported-providers-and-frameworks.md) to check native GenAI export support.
* Go to the [GenAI Span Attribute Reference](../../../references/ai-cost-management/genai-span-attribute-reference.md) to review the exact attributes CACM reads.
* Go to [AI Cost Troubleshooting](../../../resources/ai-cost-troubleshooting.md) if traces do not appear or show no cost.

{% @harness-feedback/feedback %}
