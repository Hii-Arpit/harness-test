---
description: Introduction to Harness Resilience Testing
---


# Overview

Resilience Testing is the practice of proactively validating that your systems can withstand and recover from failures, performance degradation, and disasters. By testing how your applications and infrastructure behave under stress, failure, and catastrophic scenarios, you can identify weaknesses before they impact your users and business.

Harness Resilience Testing provides a comprehensive platform to build confidence in your system's reliability through three integrated testing approaches: Chaos Testing, Load Testing, and Disaster Recovery Testing.

{% embed url="https://youtu.be/ghXqnIbqs2Y" %}

## Why resilience testing matters <a href="#why-resilience-testing-matters" id="why-resilience-testing-matters"></a>

Modern distributed systems are complex. A single application might depend on dozens of microservices, multiple cloud providers, third-party APIs, and various infrastructure components. Any of these can fail, and often do.

Without resilience testing, you are discovering these weaknesses in production when the stakes are highest. Resilience testing shifts this discovery left, helping you:

* **Reduce downtime costs**: Identify and fix issues before they cause outages
* **Meet SLA commitments**: Validate that your systems can handle real-world conditions
* **Build customer trust**: Deliver reliable experiences even when things go wrong
* **Improve incident response**: Practice recovery procedures before you need them in production
* **Accelerate development**: Deploy with confidence knowing your systems are battle-tested

## The three pillars of resilience testing <a href="#the-three-pillars-of-resilience-testing" id="the-three-pillars-of-resilience-testing"></a>

Harness Resilience Testing brings together three complementary testing approaches, each addressing a different dimension of system reliability:

### Chaos Testing <a href="#chaos-testing" id="chaos-testing"></a>

Tests your system's resilience against **unexpected failures**. Chaos Testing introduces controlled faults - like pod failures, network latency, or resource exhaustion - to validate that your system can detect, withstand, and recover from infrastructure and application failures.

**When to use**: Validate fault tolerance, test auto-healing mechanisms, verify monitoring and alerting, practice incident response.

### Load Testing <a href="#load-testing" id="load-testing"></a>

Tests your system's resilience under **expected and peak demand**. Load Testing simulates realistic user traffic patterns to validate that your system maintains performance, availability, and reliability as load increases.

**When to use**: Validate performance under normal and peak traffic, identify bottlenecks, test auto-scaling policies, prepare for high-traffic events.

### Disaster Recovery Testing <a href="#disaster-recovery-testing" id="disaster-recovery-testing"></a>

Tests your system's resilience during **catastrophic scenarios**. DR Testing validates that your backup systems, failover mechanisms, and recovery procedures work as expected when entire regions, data centers, or critical services become unavailable.

**When to use**: Validate backup and restore procedures, test failover mechanisms, verify RTO/RPO targets, ensure business continuity.

## How they work together <a href="#how-they-work-together" id="how-they-work-together"></a>

True resilience requires all three approaches working in concert. The approaches complement each other as follows:

**During normal operations**: Load Testing validates your system can handle expected traffic while Chaos Testing ensures it remains resilient when individual components fail.

**During peak events**: Combine Load Testing with Chaos Testing to simulate Black Friday traffic while a database replica fails - the most realistic test of production conditions.

**During disasters**: DR Testing validates your recovery procedures, while Chaos Testing can verify that your failover systems are themselves resilient to failures.

**In your pipeline**: Integrate all three into your deployment process for continuous resilience validation alongside functional and security testing.

**Across all three:** Onboarded services give the three pillars a common subject. Continuous discovery invents workloads in the cluster. Resilience Testing onboarding turns the ones you select into services. For targets outside Kubernetes discovery, such as Linux VMs, use the [Custom Service Agent](../shared-capabilities/services/custom-service-agent.md). Each service then collects chaos, load, and DR results in one place. Go to [Services](../shared-capabilities/services/) for the model, and go to [Automated service onboarding](../shared-capabilities/services/service-discovery.md) for the bulk wizard.

## Use cases <a href="#use-cases" id="use-cases"></a>

**Continuous resilience validation**: Integrate resilience tests into your deployment pipelines to validate system reliability alongside functional and performance testing. Catch resilience issues before they reach production.

**Peak traffic preparation**: Combine load testing with chaos experiments to simulate high-traffic events like product launches or seasonal sales while validating that your system remains resilient to infrastructure failures.

**Disaster recovery validation**: Systematically test backup systems, failover mechanisms, and recovery procedures to ensure your DR plans work when you need them most.

**Multi-region resilience**: Validate that your system can handle region failures, network partitions, and cross-region failover scenarios while maintaining performance and availability.

## Platform capabilities <a href="#platform-capabilities" id="platform-capabilities"></a>

**200+ Built-in Faults**: Ready-to-use chaos faults covering Kubernetes, cloud platforms, Linux, Windows, and application runtimes.

**Service-Based Testing**: Discover workloads with a Kubernetes discovery agent, or define custom targets with the Custom Service Agent, then onboard them as services so experiments, risks, and scores attach to a named part of your system. Bulk discovery onboarding attaches default health probes and includes a risk scanning stage.

**Load Testing**: Simulate realistic user traffic patterns to validate system performance, identify bottlenecks, and test auto-scaling under expected and peak demand.

**DR Testing**: Validate disaster recovery procedures, backup systems, and failover mechanisms to ensure business continuity during catastrophic scenarios.

**Resilience Probes**: Programmatically validate system behavior and steady state through integrations with APMs and applications - no manual observation required.

**Actions**: Execute custom tasks within experiments for notifications, webhooks, load testing triggers, and more.

**Risk Management**: Identify and track resilience, performance, and compliance risks across your systems with automated discovery and continuous monitoring.

**Enterprise Governance**: Fine-grained control over who can run which tests on what systems and during which time periods through ChaosGuard.

**Seamless Integrations**: Connect with CI/CD pipelines, monitoring tools, and cloud service providers through built-in connectors.

**AI-Powered Insights**: Get intelligent recommendations for experiment creation, optimization, and failure resolution from the AI Reliability Agent.

The platform includes enterprise features like RBAC, SSO, comprehensive logging, and audit capabilities. Available in SaaS and on-premise deployments with a free plan that includes all capabilities. For information about general Harness Platform concepts and features, go to [Harness Platform key concepts](https://developer.harness.io/harness-platform/new-to-harness-platform/overview).

## Next steps <a href="#next-steps" id="next-steps"></a>

* [Key Concepts](key-concepts.md): Understand core resilience testing terminology and concepts
* [Get Started with Chaos Testing](../chaos-testing/get-started.md): Run your first chaos experiment
* [Explore Chaos Faults](https://app.gitbook.com/s/lSkpbpeYJ3rfGUkIrcyQ/faults/chaos-fault-categories/chaos-faults-reference): Browse 200+ ready-to-use fault scenarios
* [Set Up Governance](../shared-capabilities/rbac.md): Configure RBAC and ChaosGuard for safe testing
* [Get Started with Load Testing](../load-testing/get-started.md): Simulate traffic and test performance
* [Get Started with DR Testing](../disaster-recovery-testing/get-started.md): Validate disaster recovery procedures

{% @harness-feedback/feedback %}
