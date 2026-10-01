---
description: >-
  Instrument Google Agent Development Kit (ADK) applications to emit GenAI
  traces to Cloud & AI Cost Management using the OpenInference ADK
  instrumentation.
tags:
  - cloud-cost-management
  - ai-cost-management
---


# Google ADK

Use this integration for Google Agent Development Kit (ADK) applications. Add the OpenInference ADK instrumentation and point it at Harness. This is a code-based integration, because ADK has no external tracing service to route from. The instrumentation captures the full workflow: tools, LLM calls, and agent steps, not just the LLM call.

Before you start, generate an ingestion token and connect a provider. Go to the [AI Cost Management Quickstart](../../../new-to-cacm/ai-cost-management/quickstart.md) for those steps. Replace `<YOUR_TOKEN>` below with that token and adjust the endpoint for your Harness cluster.

{% hint style="info" %}
**GENAI SEMANTIC CONVENTIONS REQUIRED**

Cost is calculated from OpenTelemetry traces with GenAI semantic conventions. Go to the [GenAI Span Attribute Reference](../../../references/ai-cost-management/genai-span-attribute-reference.md) to review the attributes CACM reads.
{% endhint %}

***

### Instrument Google ADK <a href="#instrument-google-adk" id="instrument-google-adk"></a>

Google's [Agent Development Kit (ADK)](https://google.github.io/adk-docs/) is instrumented with OpenTelemetry. Use the [OpenInference ADK instrumentation](https://github.com/Arize-ai/openinference/tree/main/python/instrumentation/openinference-instrumentation-google-adk) to route traces to Harness.

Install the instrumentation library:

```bash
pip install openinference-instrumentation-google-adk \
  opentelemetry-sdk opentelemetry-exporter-otlp
```

Configure the OpenTelemetry exporter once at application startup, then add the framework instrumentor:

```python
from opentelemetry import trace
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor
from opentelemetry.exporter.otlp.proto.http.trace_exporter import OTLPSpanExporter
from openinference.instrumentation.google_adk import GoogleADKInstrumentor

provider = TracerProvider()
provider.add_span_processor(
    BatchSpanProcessor(
        OTLPSpanExporter(
            endpoint="https://app.harness.io/udp-ingest/otel/v1/traces",
            headers={"Authorization": "Bearer <YOUR_TOKEN>"},
        )
    )
)
trace.set_tracer_provider(provider)

GoogleADKInstrumentor().instrument(tracer_provider=provider)
```

**What this produces:**

* One trace per ADK agent invocation.
* Nested spans for tools, LLM calls, and agent steps.
* GenAI semantic conventions on LLM spans.
* Cost calculated from token counts.

Go to the [ADK observability docs](https://google.github.io/adk-docs/observability/) to review the built-in tracing model.

***

### Reduce trace data volume <a href="#reduce-trace-data-volume" id="reduce-trace-data-volume"></a>

Large payloads and over-instrumentation inflate span volume and storage cost. To keep trace data manageable in high-traffic production:

* **Scope instrumentation to LLM calls:** Instrument the model calls that carry cost, not every function in the application.
* **Sample a percentage of traces:** Export a representative sample rather than every trace.

***

### Verify traces in Cost Explorer <a href="#verify-traces-in-cost-explorer" id="verify-traces-in-cost-explorer"></a>

1. Run the application so traces flow. They usually appear within a few minutes; allow up to about 20 minutes.
2. Go to **Cloud & AI Cost Management** > **Cost Explorer**.
3. Select the **AI Traces** view or group by **Service Name**, and find your service.
4. Select a service row to open the **Service Traces** drawer and inspect the span waterfall.

***

### Next steps <a href="#next-steps" id="next-steps"></a>

* Go to [Supported Providers and Frameworks](../../../references/ai-cost-management/supported-providers-and-frameworks.md) to check native GenAI export support.
* Go to the [GenAI Span Attribute Reference](../../../references/ai-cost-management/genai-span-attribute-reference.md) to review the exact attributes CACM reads.
* Go to [AI Cost Troubleshooting](../../../resources/ai-cost-troubleshooting.md) if traces do not appear or show no cost.

{% @harness-feedback/feedback %}
