---
description: Guide to run a chaos experiment as a Jenkins pipeline
---


# Run chaos experiments as Jenkins pipelines

This tutorial describes how to create chaos experiments using Harness Chaos Engineering (HCE) and run them in Jenkins pipelines. Chaos experiments in Harness are created the same way in the chaos engineering module, irrespective of where they are invoked from.

1.  [Create a chaos experiment in the Harness Chaos Engineering module.](../../../chaos-testing/experiments/) Execute this experiment to verify the configuration and ensure that the resilience probes are working as expected. The experiment ID and resilience score determined from this experiment run will be used to integrate the experiment with Jenkins.

    ![chaos experiment with ID and resilience score](../../../.gitbook/assets/chaos-experiments-with-id.png)
2.  Create a launch script. HCE APIs are used to invoke or launch a chaos experiment from the pipeline.

    To simplify creating an API call with the required secure parameters and data, a [CLI tool](https://app.harness.io/public/shared/tools/chaos/hce-cli/0.0.4/hce-cli-0.0.4-linux-amd64) is provided. Use this tool to create an appropriate API command to include in the pipeline script.

    Below is a sample launch script.

    ```
    #!/bin/bash

    set -e

    curl -sL https://app.harness.io/public/shared/tools/chaos/hce-cli/0.0.4/hce-cli-0.0.4-linux-amd64 -o hce-cli

    chmod +x hce-cli

    output=$(./hce-cli generate --api launch-experiment --account-id=${ACCOUNT_ID} \
    --project-id ${PROJECT_ID} --workflow-id ${WORKFLOW_ID} \
    --api-key ${API_KEY} --file-name hce-api.sh | jq -r '.data.runChaosExperiment.notifyID')

    echo ${output}
    ```

    <div data-gb-custom-block data-tag="hint" data-style="info" class="hint hint-info"><p><strong>DEMO</strong></p><p>Go to <a href="https://github.com/ksatchit/hce-jenkins-integration-demo">Jenkins demo</a> for a sample configuration of the chaos launch script. You can include this script in the Jenkins configuration file. This is a sample to include one single chaos experiment, but the same can be repeated so as to be included in multiple chaos experiments.</p></div>
3.  Insert chaos experiments into Jenkins config file. You can include the above-mentioned launch script in the Jenkins pipeline as a stage or a step. In the `script` section, add the scripts for **launching**, **monitoring** and **retrieving** results. An example is shown below.

    ```
    stage('Launch Chaos Experiment') {
                steps {
                     sh '''
                        sh scripts/launch-chaos.sh > n_id.txt
                     '''
                     script {
                         env.notify_id = sh(returnStdout: true, script: 'cat n_id.txt').trim()
                     }
                }
            }

            stage('Monitor Chaos Experiment') {
                steps {
                    sh '''
                       sh scripts/monitor-chaos.sh ${notify_id}
                    '''
                }
            }

            stage('Verify Resilience Score') {
                steps {
                    sh '''
                        sh scripts/verify-rr.sh ${notify_id} > r_s.txt
                    '''
                    script {
                        env.resilience_score = sh(returnStdout: true, script: 'cat r_s.txt').trim()
                     }
                }
            }

            stage('Take Rollback Decision') {
                steps {
                    sh '''
                        echo ${resilience_score}
                        sh scripts/rollback-deploy.sh ${resilience_score}
                    '''
                }
            }
    ```

    <div data-gb-custom-block data-tag="hint" data-style="info" class="hint hint-info"><p>The resilience score is the result of the experiment, and helps decide if a rollback job needs to be invoked.</p></div>
4.  Retrieve the resilience score using the Harness Chaos API and take appropriate action in the pipeline. An example of how to use the Harness Chaos API is shown below.

    ```
    #!/bin/bash

    set -e

    curl -sL https://app.harness.io/public/shared/tools/chaos/hce-cli/0.0.4/hce-cli-0.0.4-linux-amd64 -o hce-cli

    chmod +x hce-cli

    resiliencyScore=$(./hce-cli generate --api validate-resilience-score  --account-id=${ACCOUNT_ID} \
    --project-id ${PROJECT_ID} --notifyID=$1  \
    --api-key ${API_KEY} --file-name hce-api.sh)

    echo "${resiliencyScore}"
    ```

{% @harness-feedback/feedback %}
