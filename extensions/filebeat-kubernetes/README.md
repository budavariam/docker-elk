# Filebeat on kubernetes

You can use filebeat to collect logs from kubernetes pods. Then send them to either elasticsearch directly or to logstash.

Elasticsearch [documented](https://www.elastic.co/blog/shipping-kubernetes-logs-to-elasticsearch-with-filebeat) how to configure with their product.

They manage a configuration yaml file for the popular versions in their repository.

```bash
# Download Filebeat DaemonSet manifest
curl -L -O https://raw.githubusercontent.com/elastic/beats/7.5/deploy/kubernetes/filebeat-kubernetes.yaml
```

Then configure the necessary variables.

```yml
# Update Elasticsearch connection details
- name: ELASTICSEARCH_HOST
 value: elasticsearch
- name: ELASTICSEARCH_PORT
 value: "9200"
- name: ELASTICSEARCH_USERNAME
 value: elastic
- name: ELASTICSEARCH_PASSWORD
 value: changeme
```

Later deploy the configurations.

```bash
# Deploy it to Kubernetes
kubectl create -f filebeat-kubernetes.yaml
```

## Use logstash

I decided to use logstash before forwarding the logs to the running elasticsearch. In order to achieve that, I added this configuration to the `filebeat.yml` configMap entry, to match the logstash that is configured in this repository.

```config
output.logstash:
    hosts: ["${LOGSTASH_HOST:logstash}:${LOGSTASH_PORT:5044}"]
```

Also filled `LOGSTASH_HOST` and `LOGSTASH_HOST` environment variables in the `filebeat` container.

Note: Since I've used a local docker-for-desktop kubernetes cluster I needed to set the network to `docker.for.mac.host.internal` in order to be able to communicate with my docker-compose app.
