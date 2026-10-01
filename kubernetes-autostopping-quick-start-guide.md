---
description: >-
  This tutorial shows you how to create and test an AutoStopping rule for your
  Kubernetes cluster.
---


# Kubernetes Autostopping

This tutorial explains how to create and test an AutoStopping rule for your Kubernetes cluster.

### Install a sample application <a href="#install-a-sample-application" id="install-a-sample-application"></a>

To test AutoStopping, you can use a sample nginx application.

To install this sample application, apply the following YAML in your Kubernetes cluster. This installs a demo application named `autostopping-sample-app` that runs three pods running nginx in the `autostopping-sample` namespace.

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: autostopping-sample

---

apiVersion: apps/v1
kind: Deployment
metadata:
  name: autostopping-sample-app
  namespace: autostopping-sample
  labels:
    app: autostopping-sample-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: autostopping-sample-app
  template:
    metadata:
      labels:
        app: autostopping-sample-app
    spec:
      containers:
      - name: autostopping-sample-app
        image: nginx:1.14.2
        ports:
        - containerPort: 80

---

apiVersion: v1
kind: Service
metadata:
  name: autostopping-sample-svc
  namespace: autostopping-sample
spec:
  selector:
    app: autostopping-sample-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

### Supported ingress controllers for Kubernetes AutoStopping <a href="#supported-ingress-controllers-for-kubernetes-autostopping" id="supported-ingress-controllers-for-kubernetes-autostopping"></a>

Kubernetes AutoStopping support is fully available for the following ingress controllers and API gateways:

* Nginx
* [Istio](kubernetes-autostopping-for-istio.md)
* [Traefik](kubernetes-autostopping-traefik.md)
* [Ambassador](kubernetes-autostopping-ambassador.md)
* HAProxy

{% tabs %}
{% tab title="Kubernetes AutoStopping for Nginx" %}
1. You will need the following options set on your NGINX Ingress Controller:

```yaml
allow-snippet-annotations: true
annotations-risk-level: Critical
```

1. Expose your application by creating an ingress rule. Apply the following YAML to the cluster.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: autostopping-sample-ig
  namespace: autostopping-sample
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: autostopping-sample-svc
            port:
              number: 80

```

2. Create an AutoStopping Rule. Before applying this YAML, make sure that the `cloud-connector-id` is updated and the namespace of the Rule is the same as the `autostopping-sample-app` application.

```yaml
apiVersion: ccm.harness.io/v1
kind: AutoStoppingRule
metadata:
  name: autostopping-sample-rule
  namespace: autostopping-sample
  annotations:
      harness.io/cloud-connector-id: Lightwing_Non_Prod
spec:
    service:
        name: autostopping-sample-svc
        port: 80
    ingress:
        name: autostopping-sample-ig
        controllerName: nginx
    idleTimeMins: 5
    ahideProgressPage: false
```

Your sample application is now managed by an AutoStopping Rule called `autostopping-sample-ru`.
{% endtab %}

{% tab title="Kubernetes AutoStopping for Istio" %}
1. Expose your application by creating an ingress rule.

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: httpbin
  namespace: autostopping-sample
spec:
  gateways:
  - http-gateway
  http:
  - match:
    - uri:
        prefix: /autostopping-test
    route:
    - destination:
        port:
          number: 80
        host: autostopping-sample-svc
```

2. Create an AutoStopping Rule. Before applying this YAML, make sure that the `cloud-connector-id` is updated and the namespace of the Rule is the same as the `autostopping-sample-app` application.

```yaml
apiVersion: ccm.harness.io/v1
kind: AutoStoppingRule
metadata:
    name: autostopping-sample-rule
    namespace: autostopping-sample
    annotations:
        harness.io/cloud-connector-id: [CloudConnectorID]
spec:
    service:
        name: autostopping-sample-svc
        port: 80
    istio:
        virtualServices:
          - httpbin
    idleTimeMins: 5
    hideProgressPage: false
```

Your sample application is now managed by an AutoStopping Rule called `autostopping-sample-rule`.
{% endtab %}
{% endtabs %}

{% @harness-feedback/feedback %}
