---
description: >-
  This topic describes how to configure an Autostopping Proxy as a downstream to
  AWS ALB.
---


# Configuring Autostopping Proxy as a Downstream to AWS ALB

The AutoStopping Proxy can be used as a downstream system to existing ALB(s) in order to leverage dynamic idle-time detection required for AutoStopping of resources. This can be done for various types of load balancers. This would mean no changes to existing DNS mappings done on AWS ALB and involves easier configuration without any disruptions.

![](../../.gitbook/assets/autostopping-proxy-alb.png)

#### Steps to be performed to configure an AutoStopping proxy as a downstream system of AWS ALB: <a href="#steps-to-be-performed-to-configure-an-autostopping-proxy-as-a-downstream-system-of-aws-alb" id="steps-to-be-performed-to-configure-an-autostopping-proxy-as-a-downstream-system-of-aws-alb"></a>

* Create a target group for the proxy VM with a health check configuration.
* Edit ALB rules and add forwarding action to the proxy target group.
* Create AutoStopping rule with HTTP/HTTPS workload and configure custom domains in the AutoStopping rule.

### Create a target group for the AutoStopping proxy VM with a health check configuration <a href="#create-a-target-group-for-the-autostopping-proxy-vm-with-a-health-check-configuration" id="create-a-target-group-for-the-autostopping-proxy-vm-with-a-health-check-configuration"></a>

1. In the AWS console, navigate to Target Groups and create a new target group.
2. Choose the AutoStopping Proxy VM and register it as a target.
3. The port should match the port details of the application.
4. Configure the health check settings as per the port information. Success codes should be configured to be a range from 200-499.

![](../../.gitbook/assets/alb-health-check.png)

5. If the Proxy needs to handle multiple ports (80, 443), create a target group for each of the ports.

![](../../.gitbook/assets/alb-target-group.png)

### Edit ALB rules and add forwarding action to the proxy target group <a href="#edit-alb-rules-and-add-forwarding-action-to-the-proxy-target-group" id="edit-alb-rules-and-add-forwarding-action-to-the-proxy-target-group"></a>

All the URLs configured on the AutoStopping Rules should point to the AutoStopping Proxy target group.

![](../../.gitbook/assets/edit-alb-forwarding-rule.png)

### Create an AutoStopping rule with custom domains <a href="#create-an-autostopping-rule-with-custom-domains" id="create-an-autostopping-rule-with-custom-domains"></a>

![](../../.gitbook/assets/autostopping-rule-with-custom-domains.png)

{% @harness-feedback/feedback %}
