---
description: Create AutoStopping rules for Kubernetes with the Traefik ingress controller.
---


# Kubernetes Autostopping for Traefik

Learn how to create AutoStopping rules for Kubernetes with the Traefik ingress controller.

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Make sure to meet the following prerequisites before you create an AutoStopping rule for Traefik.

* Make sure you are running at the least version 1.0.5 of `autostopping-controller` in your Kubernetes cluster.
* Traefik `ingressRoute` is configured and is routing traffic to the service as expected.
* Routing to external names is allowed in Traefik by setting the following flag: `--providers.kubernetescrd.allowexternalnameservices=true`
* You should have access to edit the Traefik ingressRoute.

{% hint style="info" %}
Allowing traffic to external names by setting the flag `--providers.kubernetescrd.allowexternalnameservices=true` is required because the autostopping-router is an external name service for all other services.
{% endhint %}

### Set up your cluster <a href="#set-up-your-cluster" id="set-up-your-cluster"></a>

Create an AutoStopping rule in your cluster, either through the Harness UI or by applying YAML directly on the cluster of the form:

```yaml
apiVersion: ccm.harness.io/v1
kind: AutoStoppingRule
metadata:
    name: test-rule
    namespace: default
    annotations:
        harness.io/cloud-connector-id: CloudAccountID
spec:
    service:
        name: echo
        port: 8080
    traefik:
        ingressRoute: httpbin
    idleTimeMins: 5
    hideProgressPage: false
```

After applying the YAML, an AutoStopping Rule is created in your cluster for service echo which is running on port 8080.

### Create Traefik middleware to pass the extra-header <a href="#create-traefik-middleware-to-pass-the-extra-header" id="create-traefik-middleware-to-pass-the-extra-header"></a>

This header sends the AutoStoppingRule header to all the associated ingress routes.

```yaml
apiVersion: traefik.io/v1alpha1
kind: Middleware
metadata:
  name: test-rule-header
spec:
  headers:
    customRequestHeaders:
      AutoStoppingRule: default-test-rule
```

The `AutoStoppingRule` header should be set to a value following the pattern `<namespace>-<rule>` which can be found from the metadata of the `AutoStoppingRule` you created above.

### Change IngressRoute <a href="#change-ingressroute" id="change-ingressroute"></a>

Once the Traefik ingressRoute is supported as a first class entity for AutoStopping, these changes will be automated.

After creating the AutoStopping Rule, make the following changes in your Traefik IngressRoute:

1. Change the destination

_From_

```yaml
services:
- kind: Service
  name: echo
  port: 8080
```

_To_

```yaml
services:
- kind: Service
  name: autostopping-router
  port: 80
```

2. Add header middleware. Under the spec for the ingressroute, add the middleware that was created earlier.

```text
....
middlewares:
  - name: test-rule-header
.....
```

Your ingressRoute should be similar to the following:

```yaml
apiVersion: traefik.io/v1alpha1
kind: IngressRoute
metadata:
  annotations:
    helm.sh/hook: post-install,post-upgrade
  name: echo-traefik
  labels:
    app: echo
    traefik_version: 10.20.1
spec:
  routes:
  - kind: Rule
    match: Host(`someip.nip.io`)
    services:
    - kind: Service
      name: autostopping-router
      port: 80
    middlewares:
      - name: test-rule-header
```

Now your traffic flows through the autostopping-router and the AutoStopping rule takes actions based on activity and inactivity.

{% @harness-feedback/feedback %}
