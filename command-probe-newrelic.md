---
description: Guide to using the command probe with the source parameter in New Relic
title: Command probe with New Relic
sidebar_position: 6
redirect_from:
  - /docs/chaos-engineering/features/probes/cmd-probe/cmd-probe-newrelic
  - >-
    /docs/chaos-engineering/features/resilience-probes/cmd-probe/cmd-probe-newrelic
  - /docs/chaos-engineering/use-harness-ce/probes/cmd-probe-newrelic
---


# Command probe with New Relic

This topic guides you through steps to use the **command probe** in **source mode** to extract and validate the data from the APM tool New Relic.

### Before you begin, review the following <a href="#before-you-begin-review-the-following" id="before-you-begin-review-the-following"></a>

* [Command probe](index.md)
* [Create a command probe](../index.md#create-a-resilience-probe)
* [Command probe with source parameter](command-probe-usage.md#configure-command-probe-with-source-parameter)

#### Extract Data from New Relic <a href="#extract-data-from-new-relic" id="extract-data-from-new-relic"></a>

1. Create a binary file that stores the logic to extract data from New Relic. In this example, we will create the logic such that this file should extract the minimum, maximum, and mean values from the API response.
2. Dockerize the binary. This image contains the logic to query the New Relic GraphQL API and extract the minimum, maximum, and mean values from the API response.
3. [Create a new command probe](../index.md#create-a-resilience-probe), add the necessary details, and select **Configure Properties**.
4.  Add the necessary details, and select **Configure Details**.

    ![](../../../.gitbook/assets/details-2.png)
5. Specify the command as **./main**. This command runs the dockerized binary that contains the logic to extract data from New Relic.
6.  Select the type as **Float** in the **type** sub-field of **Data Comparison** field, because the data extracted from New Relic is expected to be a float value. Specify the **Comparison Criteria** and the expected value. Enable the **Source** button to allow using custom images, environment variables, and secrets.

    ![](../../../.gitbook/assets/details-3.png)
7. Once the **Source** mode is enabled, a YAML text editor appears on the UI. Specify all the details for the source probe that are required to fetch the data from New Relic. Pass the following parameters as environment variables:

* **NRQL\_QUERY** : The NRQL query that is to be fetched from New Relic.
* **NRQL\_ACCOUNT** : The account ID of the New Relic account.
* **NRQL\_QUERY\_METRICS** : The metrics that will be evaluated. The provided **NRQL** query can include multiple metrics.
* **NRQL\_API\_KEY** : The API key used to query New Relic.
* **NRQL\_EVALUATION\_TYPE** : The evaluation type, which could be min, max, or mean.

Below is the example configuration where the **NRQL\_API\_KEY** environment variable is referred from a Kubernetes secret. Ensure that the Kubernetes secret is present in the same namespace where chaos infrastructure is running.

```
image: docker.io/aady12/newrelic-p:3.2
env:
 - name: NRQL_QUERY
   value: SELECT (average(net.rxBytesPerSecond) / 1000) AS `Received KBps`, (average(net.txBytesPerSecond) / 1000) AS `Transmitted KBps`, average(net.errorsPerSecond) AS `Errors / sec` FROM K8sPodSample WHERE (entityGuid = 'NDQ1Mzg5NXxJTkZSQXxOQXwxMjg5MjUwNjUxOTg2MDI1OTA1') TIMESERIES AUTO
 - name: NRQL_ACCOUNT
   value: "12345"
 - name: NRQL_API_KEY
    valueFrom:
      secretKeyRef:
        name: newrelic-sec
        key: NEWRELIC_KEY
 - name: NRQL_QUERY_METRICS
   value: Received KBps
 - name: NRQL_EVALUATION_TYPE
   value: max
```

8. Attach the probe created in the experiment and validate the metrics from New Relic.

{% @harness-feedback/feedback %}
