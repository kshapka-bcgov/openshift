## Inputs / Outputs
https://docs.fluentbit.io/manual/pipeline/inputs
-
* Container logs
* Kubernetes metadata
  * labels
  * annotations

https://docs.fluentbit.io/manual/pipeline/outputs
-
* Prometheus
* PostgreSQL


## Deployment
### Helm chart
* One Fluentbit per node
  * ~650kB each

To install the default helm chart:
```
helm repo add fluent https://fluent.github.io/helm-charts
helm search repo fluent
helm upgrade --install fluent-bit fluent/fluent-bit
```

> **Note for OpenShift**
> 
> If you are using Red Hat OpenShift you will also need to set up security context constraints (SCC) using the relevant option in the helm chart.

#### Details
The default configuration of Fluent Bit makes sure of the following:
* Consume all containers logs from the running Node and parse them with either the docker or cri multiline parser.
* Persist how far it got into each file it is tailing so if a pod is restarted it picks up from where it left off.
* The Kubernetes filter will enrich the logs with Kubernetes metadata, specifically labels and annotations. The filter only goes to the API Server when it cannot find the cached info, otherwise it uses the cache.
* The default backend in the configuration is Elasticsearch set by the Elasticsearch Output Plugin. It uses the Logstash format to ingest the logs. If you need a different Index and Type, please refer to the plugin option and do your own adjustments.
* There is an option called Retry_Limit set to False, that means if Fluent Bit cannot flush the records to Elasticsearch it will re-try indefinitely until it succeed.