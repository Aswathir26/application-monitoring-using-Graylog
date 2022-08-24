# INTRODUCTION

Deployment of Graylog

Graylog is a fully integrated open source log management platform for collecting, indexing, and analyzing both structured and unstructured data from almost any source. Fluntbit is used to send kubernetes logs to Graylog. By integrating metricbeat we can monitor metrics from kubernetes cluster too.

This is a simple example of Application/Kubernetes monitoring using graylog,Fluentbit and metricbeat

# Notes:

**values.yaml configuration of Graylog**

change number of replica of elasticsearch as needed.

graylog.transportEmail is not enabled. configure it if needed.

**values.yaml configuration of Fluentbit**

change hostname in config.outputs as needed (it changes with namespace)

**configuration of metricbeat**

Kubernetes cluster has some default metricsets available. Metricbeat requires kube-state-metrics to be deployed and running to gather the cluster metrics.

value of LOGSTASH HOST env name changes with namespace (metricbeat-kubernetes.yaml). Change it accordingly.

**contentpacks**

Here content packs are created using basic configurations.

change notification configuration accordingly in events.json (line no:194, 225). Here HTTP notification type is used. So add your webhook url. You can use any notification type like email, slack, ...

references: https://docs.graylog.org/v1/docs/notifications   
            https://docs.graylog.org/docs/content-packs
--------------------------------------------------------------------------------------------------------------------------


# STEP 1:

 $ kubectl create namespace logging
 
# STEP 2:

 $ git clone https://github.com/Aswathir26/application-monitoring-using-Graylog.git
 $ cd application-monitoring-using-Graylog/

# STEP 3:

 $ helm repo add kongz https://charts.kong-z.com

 $ helm install -f values.yaml graylog kongz/graylog -n logging

# STEP 4:

 $ cd fluent-bit

 $ helm repo add fluent https://fluent.github.io/helm-charts

 $ helm install -f values.yaml fluent-bit fluent/fluent-bit -n logging

# STEP 5:

 $ cd metricbeat

 $ kubectl apply -f ./kube-state-metrics

 $ kubectl apply -f metricbeat-kubernetes.yaml

# STEP 6:

login to graylog

navigate to system/content packs

upload and install content packs one by one (json files available in contentpacks directory):

1. input.json

2. dashboards.json

3. events.json

or create them manually
