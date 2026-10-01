---
description: >-
  Which SDKs, gateways, and frameworks emit GenAI traces natively for AI Cost
  Management trace attribution.
tags:
  - cloud-cost-management
  - ai-cost-management
---


# Compatibility Matrix

Trace attribution needs OpenTelemetry traces with GenAI semantic conventions. The tables below show which model SDKs, gateways, and frameworks emit those attributes natively, and which are supported by the Harness instrumentation SDK.

Use this matrix to decide your setup path: if your stack already emits GenAI traces, route them to Harness; if not, add instrumentation. Go to [AI Cost Management Quickstart](../../new-to-cacm/ai-cost-management/quickstart.md) for the steps.

#### Model client SDKs <a href="#model-client-sdks" id="model-client-sdks"></a>

Used for LLM inference.

| SDK                       | GenAI semconv export? | Support in Harness instrumentation SDK |
| ------------------------- | --------------------- | -------------------------------------- |
| OpenAI Python SDK         | ✅ Yes (most mature)   | ✅ Yes                                  |
| Anthropic SDK             | ✅ Yes                 | ✅ Yes                                  |
| Google GenAI / Vertex SDK | ✅ Yes                 | ✅ Yes                                  |
| AWS Bedrock               | ✅ Yes                 | Coming soon                            |

#### Gateways <a href="#gateways" id="gateways"></a>

| Gateway | GenAI semconv export? | Support in Harness instrumentation SDK |
| ------- | --------------------- | -------------------------------------- |
| LiteLLM | ✅ Yes                 | ✅ Yes                                  |
| Envoy   | ❌ No                  | N/A                                    |

#### Agent Orchestration frameworks <a href="#agent-orchestration-frameworks" id="agent-orchestration-frameworks"></a>

| Framework                                   | GenAI semconv export? |
| ------------------------------------------- | --------------------- |
| Google Agent Development Kit                | ✅ Native              |
| OpenAI Agents SDK ("OpenAI ADK")            | ✅ Yes                 |
| Anthropic Agents SDK                        | ✅ Yes                 |
| Microsoft Agent Framework ("Microsoft ADK") | ✅ Native (partial)    |
| Microsoft Semantic Kernel                   | ✅ Native (opt-in)     |
| AWS Strands Agents SDK                      | ✅ Native              |
| LangGraph / LangChain                       | ⚠️ Not native         |
| CrewAI                                      | ⚠️ Mixed              |
| LlamaIndex                                  | ⚠️ Not native         |
| Pydantic AI                                 | ✅ Native              |
| AutoGen / AG2                               | ⚠️ Partial            |
| Haystack                                    | ⚠️ Not native         |
| Smolagents                                  | ⚠️ Not native         |

{% hint style="info" %}
**FRAMEWORKS MARKED "NOT NATIVE" STILL WORK**

A framework that does not emit GenAI semantic conventions natively can still be instrumented. Use the OpenInference instrumentation or the LangSmith OTel export for LangChain and LangGraph, or the Harness SDK when the framework routes through LiteLLM. Go to [SDK Integrations](../../integrations/sdk-integrations/README.md) for per-framework steps.
{% endhint %}

***

### Next steps <a href="#next-steps" id="next-steps"></a>

* Go to [AI Cost Management Quickstart](../../new-to-cacm/ai-cost-management/quickstart.md) to connect a provider and see billed cost.
* Go to the [AI Cost Management Quickstart](../../new-to-cacm/ai-cost-management/quickstart.md) to instrument your application for trace attribution.
* Go to the [GenAI Span Attribute Reference](genai-span-attribute-reference.md) to review the attributes CACM reads from each span.
* Go to [AI Cost Troubleshooting](../../resources/ai-cost-troubleshooting.md) if data does not appear as expected.
* Go to the [AI Cost Management FAQ](../../resources/faq.md) to review common questions.

{% @harness-feedback/feedback %}
