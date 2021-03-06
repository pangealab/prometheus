![Intro](./docs/prometheus-ui.png)

This project describes deploying a Prometheus Pod into an OKD namespace to monitor Prometheus metric compatible services. Services should be able to collect telemetry from application internals as well as network requests in order to help SRE's measure performance and react when services are alarming. 

The OKD installed [Grafana](https://prometheus.io/docs/visualization/grafana) is configured with an additional Data Source to connect to the Prometheus Pod and ingest the metrics continuously. 

Prometheus will look in your namespace for Pods tagged using `monitor=true` Tags and collect their metrics. To experiment with other Prometheus configuratons you can edit the Prometheus `Config Map` in deployed in your namespace.

# Prerequisites

* OKD Cluster Admin Access
* OKD Namespace (e.g. *sandbox*)
* Deployed Pods ready to be monitored

# Installation

1. Login as Cluster Admin

    ```
    oc login -u system:admin
    ```

1. Grant Cluster Reader Role to your Namespace (e.g. *sandbox*) Service Account

    ```
    oc adm policy add-cluster-role-to-user cluster-reader system:serviceaccount:sandbox:default
    ```

1. Clone Project

    ```
    git clone git@github.com:advlab/prometheus.git
    cd prometheus
    ```

1. Deploy Pod to Namespace (e.g. *prometheus* pod in the *sandbox* namespace)

    ```
    oc new-app -f templates/deploy-prometheus.yml -p POD_NAME=prometheus -p NAMESPACE=sandbox
    ```

1. Test Access

    Browse to your Pod's URL (e.g. http://prometheus-sandbox.openshift.sytes.net)

# Teardown

1. To delete the Pod (e.g. *prometheus*) and related resources use:

    >NOTE: Use Caution

    ```
    oc delete all -l app=prometheus
    oc delete configmap -l app=prometheus
    ```

# Configure Grafana

1. Connect to [Grafana](https://grafana-openshift-monitoring.openshift.sytes.net)

1. Navigate to `Configuration > Datasources` and click on **Add a datasource** 

    Use the following field values:

    >NOTE: Leave all other fields defaulted

    * Name: `YOUR NAMESPACE` (e.g. *sandbox*)
    * Type: `Prometheus`
    * URL: `YOUR PROMETHEUS URL` (e.g. http://prometheus-sandbox.openshift.sytes.net)

    Lastly, press **Save and Test**



