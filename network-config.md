---
description: >-
  Configure mTLS and Harness Network Proxy (HNP) for the chaos runner and
  Discovery Agent on Kubernetes chaos infrastructure.
tags:
  - chaos-engineering
  - infrastructure
  - network
  - security
---


# Network configuration

This page covers the two network settings the chaos runner (DDCR) and the Discovery Agent expose on **Kubernetes chaos infrastructure**: **mTLS** for stronger client authentication on top of Harness token auth, and **proxy** (your own proxy or Harness Network Proxy) for outbound traffic from restricted clusters.

***

## Before you begin <a href="#before-you-begin" id="before-you-begin"></a>

* **Familiarity with Delegate mTLS.** Go to [Delegate mTLS support](https://developer.harness.io/harness-platform/use-harness-platform/delegates/delegate/secure-delegates/delegate-mtls-support) for the platform-level concepts.
* **An installed Delegate (DDCR).** Go to [Set up Kubernetes infrastructure](./) if you do not have one yet.
* **Cluster-side TLS material.** A client certificate and key, ideally stored in a Kubernetes secret on the cluster where the chaos runner executes.

***

## How traffic flows <a href="#how-traffic-flows" id="how-traffic-flows"></a>

The chaos runner authenticates to Harness with a token by default. The Discovery Agent runs in the same cluster and uses the same outbound path.

![Discovery Agent and Chaos Runner traffic flow](../../../.gitbook/assets/agent-proxy.png)

* Inbound connections to the runner always come **through the Delegate**.
* Outbound calls go straight to the Harness control plane if the cluster has connectivity.
* If the cluster cannot reach the control plane directly, route outbound traffic through your own proxy or through Harness Network Proxy (HNP). Each is configured the same way: set `HTTP_PROXY`, `HTTPS_PROXY`, and `NO_PROXY` on the Delegate and the Discovery Agent.

{% hint style="info" %}
HNP supports custom certificates, so you can pin TLS material to the proxy rather than spread it across every target cluster.
{% endhint %}

***

## Use mTLS with DDCR and the Discovery Agent <a href="#use-mtls-with-ddcr-and-the-discovery-agent" id="use-mtls-with-ddcr-and-the-discovery-agent"></a>

For both components, the configuration is the same: store the client cert and key in a Kubernetes secret in the cluster the component runs in, then point the component at that secret in the UI.

### Configure mTLS on DDCR <a href="#configure-mtls-on-ddcr" id="configure-mtls-on-ddcr"></a>

1. Create a Kubernetes secret containing the client certificate and key on the target cluster.
2.  Edit the Kubernetes chaos infrastructure and reference the secret in the mTLS fields.

    ![mTLS fields on the infrastructure](../../../.gitbook/assets/mtls-1.png)

If you do not want to provision a secret in every target cluster, [install HNP](network-config.md#install-harness-network-proxy-hnp) with the cert and key, then point the chaos runner at the HNP URL instead.

![Proxy URL on the chaos runner](../../../.gitbook/assets/proxy-2.png)

### Configure mTLS on the Discovery Agent <a href="#configure-mtls-on-the-discovery-agent" id="configure-mtls-on-the-discovery-agent"></a>

Same pattern. Create the secret on the cluster the Discovery Agent runs in, then reference it in the discovery agent settings under **Resilience Testing → Project Settings → Discovery**.

![mTLS fields on the discovery agent](../../../.gitbook/assets/mtls-1.png)

The HNP fallback applies here too.

![Proxy URL on the discovery agent](../../../.gitbook/assets/proxy-2.png)

***

## Configure proxy settings <a href="#configure-proxy-settings" id="configure-proxy-settings"></a>

If the cluster cannot reach the Harness control plane directly, the chaos runner and Discovery Agent can be put behind a proxy. Either use a proxy you already operate, or install [Harness Network Proxy (HNP)](network-config.md#install-harness-network-proxy-hnp).

Both components understand the standard environment variables:

* `HTTP_PROXY`
* `HTTPS_PROXY`
* `NO_PROXY` (include the in-cluster `kubernetes` service IP in the `default` namespace so intra-cluster traffic bypasses the proxy)

You can also set `PROXY_URL` directly on the component, which has the same effect for Harness-portal communication.

### Set the proxy on DDCR <a href="#set-the-proxy-on-ddcr" id="set-the-proxy-on-ddcr"></a>

1.  Go to **Resilience Testing → Project Settings → Environments** and pick the environment that hosts the Delegate.

    ![Environments list](../../../.gitbook/assets/proj-env.png)
2.  Open the Delegate, click the **⋮** menu, and select **Edit**.

    ![Edit the Delegate](../../../.gitbook/assets/select-edit.png)
3.  Set `HTTP_PROXY`, `HTTPS_PROXY`, `NO_PROXY` (or `PROXY_URL`) and save.

    ![Proxy settings on the Delegate](../../../.gitbook/assets/proxy.png)

    ![PROXY\_URL on the Delegate](../../../.gitbook/assets/mtls.png)

### Set the proxy on the Discovery Agent <a href="#set-the-proxy-on-the-discovery-agent" id="set-the-proxy-on-the-discovery-agent"></a>

1.  Go to **Resilience Testing → Project Settings → Discovery**.

    ![Discovery in Project Settings](../../../.gitbook/assets/proj-set.png)
2.  Open the discovery agent and click **Edit**.

    ![Edit the discovery agent](../../../.gitbook/assets/edit-discovery.png)
3.  Set the proxy environment variables (or `PROXY_URL`) and save.

    ![Proxy settings on the discovery agent](../../../.gitbook/assets/proxy.png)

    ![PROXY\_URL on the discovery agent](../../../.gitbook/assets/mtls.png)

***

## Install Harness Network Proxy (HNP) <a href="#install-harness-network-proxy-hnp" id="install-harness-network-proxy-hnp"></a>

HNP is an in-cluster proxy that consolidates outbound calls from the chaos runner and Discovery Agent. It can be deployed with or without mTLS.

```bash
helm repo add harness-chaos https://harness.github.io/chaos-infra-helm-chart
helm upgrade --install chaos-agent-proxy harness-chaos/chaos-infra \
  -n hce -f override.yaml
```

### Without mTLS <a href="#without-mtls" id="without-mtls"></a>

```yaml
tags:
  agentProxy: true
global:
  serverAddress: https://app.harness.io
```

### With mTLS <a href="#with-mtls" id="with-mtls"></a>

Create the client certificate first. Go to [Enable mTLS on delegate](https://developer.harness.io/harness-platform/use-harness-platform/delegates/delegate/secure-delegates/delegate-mtls-support#enable-mtls-on-delegate).

```yaml
tags:
  agentProxy: true
global:
  serverAddress: https://<customer-name>.agent.app.harness.io

agent-proxy:
  volumes:
    - name: client-certificate
      secret:
        secretName: client-certificate
  volumeMounts:
    - mountPath: /etc/mtls
      name: client-certificate
      readOnly: true
  env:
    - name: CLIENT_CERT_PATH
      value: /etc/mtls/client.crt
    - name: CLIENT_KEY_PATH
      value: /etc/mtls/client.key
```

Replace `<customer-name>` with the subdomain assigned to your Harness account.

***

## Next steps <a href="#next-steps" id="next-steps"></a>

* [Set up Kubernetes infrastructure](./): install the Delegate-driven runner.
* [Cluster permissions](permissions.md): review the Kubernetes RBAC the chaos service account needs.
* [Delegate mTLS support](https://developer.harness.io/harness-platform/use-harness-platform/delegates/delegate/secure-delegates/delegate-mtls-support): platform-level mTLS reference.

{% @harness-feedback/feedback %}
