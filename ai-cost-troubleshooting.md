---
description: >-
  Resolve common issues with AI Cost Management, from connectors that show no
  spend to traces that appear without cost.
tags:
  - cloud-cost-management
  - ai-cost-management
---


# AI Cost Troubleshooting

Use this page to resolve common issues with AI Cost Management, whether you are connecting a provider or instrumenting traces. Each entry expands to a solution.

***

## Connector issues <a href="#connector-issues" id="connector-issues"></a>

Issues with provider connectors and billed cost.

<details>

<summary>AI spend does not appear in Cost Explorer after 12 hours</summary>

Confirm the connector uses an org-wide admin API key, not a project-level or workspace-level key. Verify the key has read access to the provider's usage and billing endpoints, and that the connector saved without errors. First sync can take up to 12 hours; if data is still missing after that, contact Harness Support with your account ID and connector details.

</details>

<details>

<summary>Historical AI cost data is missing before a certain date</summary>

Historical data on first sync is capped per provider: OpenAI provides 90 days, Anthropic provides 30 days, and cloud connectors are limited to the underlying billing-export retention. Data cannot be backfilled past these limits. Export older data from the provider directly if you need it.

</details>

***

## Trace issues <a href="#trace-issues" id="trace-issues"></a>

Issues with GenAI trace instrumentation and trace-based cost.

<details>

<summary>No traces appear in Cost Explorer after 20 minutes</summary>

Confirm the OTLP endpoint matches your Harness cluster (for example app.harness.io vs app3.harness.io), the service account token is valid, and the application can reach https://app.harness.io/udp-ingest/otel/v1/traces (test with curl from the same environment). Check the application logs for OTLP exporter activity, and for the Harness SDK make sure Agent().instrument() runs before importing AI libraries. Set OTEL\_LOG\_LEVEL=debug to surface export errors.&#x20;

</details>

<details>

<summary>Traces appear but no cost is shown</summary>

Cost calculation requires the GenAI attributes gen\_ai.provider.name (or the legacy gen\_ai.system), gen\_ai.request.model, gen\_ai.usage.input\_tokens, and gen\_ai.usage.output\_tokens on each span, with non-zero token counts and a model identifier that matches Harness pricing data. Open Cloud and AI Cost Management, then Cost Explorer, select the AI Traces view, open the Service Traces drawer, inspect a span, and confirm these attributes are present and non-zero. If they are missing, update your instrumentation to emit them; if they are present but cost is still zero, contact Harness Support with the trace ID and model identifier.

</details>

<details>

<summary>High trace data volume or storage cost</summary>

Large prompt and response payloads and over-instrumentation inflate span volume and storage cost. Disable payload capture (HARNESS\_GEN\_AI\_PAYLOAD\_CAPTURE\_ENABLED=false for the Harness SDK), scope instrumentation to LLM calls only, and sample a percentage of traces in high-traffic production.

</details>

<details>

<summary>Trace costs do not match provider invoices</summary>

This is expected. Trace costs are approximate, calculated from token counts and published model pricing, so they differ from billed costs due to volume discounts, credits, refunds, and pricing changes. Pair traces with a provider connector for accurate billed costs, and use traces for debugging and relative comparisons rather than finance reporting.

</details>

<details>

<summary>Harness SDK does not instrument LLM calls even though spans are emitted</summary>

Call Agent().instrument() before importing any AI library (litellm, openai, anthropic) or web framework. The SDK patches these libraries at import time, so importing them first prevents instrumentation.

</details>

***

## Get help <a href="#get-help" id="get-help"></a>

If you are still stuck, [contact Harness Support](https://support.harness.io/) or your Harness account team. To speed up diagnosis, include:

* Your **account ID** and **Harness cluster** (for example `app3.harness.io`).
* A **trace ID** and the **model identifier** (`gen_ai.request.model`) for any span that shows no cost.
* Which setup path you are on (route existing traces, Harness SDK, or framework instrumentation), and your instrumentation method.

***

## Next steps <a href="#next-steps" id="next-steps"></a>

* Go to [AI Cost Management Quickstart](../new-to-cacm/ai-cost-management/quickstart.md) to connect a provider.
* Go to the [AI Cost Management Quickstart](../new-to-cacm/ai-cost-management/quickstart.md) to instrument your application.
* Go to the [GenAI Span Attribute Reference](../references/ai-cost-management/genai-span-attribute-reference.md) to review the attributes CACM needs to price a span.

{% @harness-feedback/feedback %}
