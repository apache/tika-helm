tika-helm
=========

[![Artifact HUB](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/apache-tika)](https://artifacthub.io/packages/helm/apache-tika/tika)
![Version: 3.2.2](https://img.shields.io/badge/Version-3.2.2-informational?style=flat-square)
![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square)
![AppVersion: 3.2.2.0-full](https://img.shields.io/badge/AppVersion-3.2.2.0--full-informational?style=flat-square)

<div class="artifacthub-widget" data-url="https://artifacthub.io/packages/helm/apache-tika/tika" data-theme="dark" data-header="true" data-stars="true" data-responsive="true"><blockquote><p lang="en" dir="ltr"><b>tika</b>: The official Helm chart for Apache Tika</p>&mdash; Open in <a href="https://artifacthub.io/packages/helm/apache-tika/tika">Artifact Hub</a></blockquote></div>

<img src="https://tika.apache.org/tika.png" width="300" />

We recommend that the Helm chart version is aligned to the version Tika (and subsequently the
version of the [Tika Docker image][]) you want to deploy.
This will ensure that you using a chart version that has been tested against the corresponding
production version. This will also ensure that the documentation and examples for the chart
will work with the version of Tika you are installing.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [tika-helm](#tika-helm)
  - [Installing](#installing)
    - [Install released version using Helm repository](#install-released-version-using-helm-repository)
    - [Install development version using master branch](#install-development-version-using-master-branch)
    - [Custom configuration for tika](#custom-configuration-for-tika)
  - [Upgrading](#upgrading)
  - [Configuration](#configuration)
    - [Deprecated](#deprecated)
  - [Contributing](#contributing)
  - [More Information](#more-information)
  - [Authors](#authors)
- [License](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
<!-- Use this to update TOC: -->
<!-- docker run --rm -it -v $(pwd):/usr/src jorgeandrada/doctoc --github -->

## Installing

### Install released version using Helm repository

**N.B.** You may or may not need/wish to install the chart into a specific **namespace**,
in which case you may need to augment the commands below.

* Add the Tika Helm charts repo:
`helm repo add tika https://apache.jfrog.io/artifactory/tika`

* Install it:
  - with Helm 3: `helm install tika tika/tika --set image.tag=${release.version} -n tika-test`, you will see something like
```
helm install tika tika/tika --set image.tag=latest-full -n tika-test

...
NAME: tika
LAST DEPLOYED: Mon Jan 24 13:38:01 2022
NAMESPACE: tika-test
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace tika-test -l "app.kubernetes.io/name=tika,app.kubernetes.io/instance=tika" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(kubectl get pod --namespace tika-test $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  echo "Visit http://127.0.0.1:9998 to use your application"
  kubectl --namespace tika-test port-forward $POD_NAME 9998:$CONTAINER_PORT
```
You may notice that the _kubectl port forwarding_ experiences a _timeout issue_ which ultimately kills the app. In this case you can run port formarding in a loop
```
while true; do kubectl --namespace tika-test port-forward $POD_NAME 9998:$CONTAINER_PORT ; done
```
... this should keep `kubectl` reconnecting on connection lost.

### Install development version using master branch

* Clone the git repo: `git clone git@github.com:apache/tika-helm.git`

* Install it:
  - with Helm 3: `helm install tika . --set image.tag=latest-full`

### Custom configuration for tika

To use custom [configuration]( https://tika.apache.org/2.9.1/configuring.html) values for apache tika, use the `tikaConfig` key in the `values.yaml`.
Example:
```
tikaConfig: |
  <?xml version="1.0" encoding="UTF-8"?>
  <properties>
    <parsers>
      <!-- Default Parser for most things, except for 2 mime types -->
      <parser class="org.apache.tika.parser.DefaultParser">
        <mime-exclude>image/jpeg</mime-exclude>
        <mime-exclude>application/pdf</mime-exclude>
      </parser>
    </parsers>
  </properties>
```
## Upgrading

Please check `artifacthub.io/changes` in `Chart.yaml` before upgrading.

## Values

<table>
	<thead>
		<th>Key</th>
		<th>Type</th>
		<th>Default</th>
		<th>Description</th>
	</thead>
	<tbody>
		<tr>
			<td>affinity</td>
			<td>object</td>
			<td><pre lang="json">
{}
</pre>
</td>
			<td>Affinity rules for pod scheduling</td>
		</tr>
		<tr>
			<td>autoscaling.apiVersion</td>
			<td>string</td>
			<td><pre lang="json">
"autoscaling/v2"
</pre>
</td>
			<td>API version for the HorizontalPodAutoscaler</td>
		</tr>
		<tr>
			<td>autoscaling.enabled</td>
			<td>bool</td>
			<td><pre lang="json">
false
</pre>
</td>
			<td>Enable autoscaling for Tika pods</td>
		</tr>
		<tr>
			<td>autoscaling.maxReplicas</td>
			<td>int</td>
			<td><pre lang="json">
100
</pre>
</td>
			<td>Maximum number of replicas</td>
		</tr>
		<tr>
			<td>autoscaling.minReplicas</td>
			<td>int</td>
			<td><pre lang="json">
1
</pre>
</td>
			<td>Minimum number of replicas</td>
		</tr>
		<tr>
			<td>autoscaling.targetCPUUtilizationPercentage</td>
			<td>int</td>
			<td><pre lang="json">
80
</pre>
</td>
			<td>Target CPU utilization percentage for autoscaling</td>
		</tr>
		<tr>
			<td>autoscaling.targetMemoryUtilizationPercentage</td>
			<td>int</td>
			<td><pre lang="json">
80
</pre>
</td>
			<td>Target memory utilization percentage for autoscaling</td>
		</tr>
		<tr>
			<td>config.base_url</td>
			<td>string</td>
			<td><pre lang="json">
"http://localhost/"
</pre>
</td>
			<td>Base URL for the Tika service</td>
		</tr>
		<tr>
			<td>fullnameOverride</td>
			<td>string</td>
			<td><pre lang="json">
""
</pre>
</td>
			<td>Override the full name of the release</td>
		</tr>
		<tr>
			<td>image.pullPolicy</td>
			<td>string</td>
			<td><pre lang="json">
"IfNotPresent"
</pre>
</td>
			<td>Image pull policy for the Tika container</td>
		</tr>
		<tr>
			<td>image.repository</td>
			<td>string</td>
			<td><pre lang="json">
"apache/tika"
</pre>
</td>
			<td>Docker image repository for Apache Tika</td>
		</tr>
		<tr>
			<td>image.tag</td>
			<td>string</td>
			<td><pre lang="json">
"3.2.2.0-full"
</pre>
</td>
			<td>Overrides the image tag whose default is the chart appVersion</td>
		</tr>
		<tr>
			<td>imagePullSecrets</td>
			<td>list</td>
			<td><pre lang="json">
[]
</pre>
</td>
			<td>Secrets for pulling images from a private registry</td>
		</tr>
		<tr>
			<td>ingress.annotations</td>
			<td>object</td>
			<td><pre lang="json">
{}
</pre>
</td>
			<td>Annotations for the ingress resource</td>
		</tr>
		<tr>
			<td>ingress.enabled</td>
			<td>bool</td>
			<td><pre lang="json">
false
</pre>
</td>
			<td>Enable ingress for the Tika service</td>
		</tr>
		<tr>
			<td>ingress.hosts[0]</td>
			<td>object</td>
			<td><pre lang="json">
{
  "host": "chart-example.local",
  "paths": []
}
</pre>
</td>
			<td>Hostnames for the ingress</td>
		</tr>
		<tr>
			<td>ingress.tls</td>
			<td>list</td>
			<td><pre lang="json">
[]
</pre>
</td>
			<td>TLS configuration for the ingress</td>
		</tr>
		<tr>
			<td>livenessProbe.failureThreshold</td>
			<td>int</td>
			<td><pre lang="json">
20
</pre>
</td>
			<td>Number of failed liveness probes before restarting the pod</td>
		</tr>
		<tr>
			<td>livenessProbe.initialDelaySeconds</td>
			<td>int</td>
			<td><pre lang="json">
15
</pre>
</td>
			<td>Initial delay before starting liveness probe (seconds)</td>
		</tr>
		<tr>
			<td>livenessProbe.periodSeconds</td>
			<td>int</td>
			<td><pre lang="json">
5
</pre>
</td>
			<td>Interval between liveness probes (seconds)</td>
		</tr>
		<tr>
			<td>livenessProbe.scheme</td>
			<td>string</td>
			<td><pre lang="json">
"HTTP"
</pre>
</td>
			<td>Scheme for liveness probe (HTTP or HTTPS)</td>
		</tr>
		<tr>
			<td>livenessProbe.timeoutSeconds</td>
			<td>int</td>
			<td><pre lang="json">
30
</pre>
</td>
			<td>Timeout for liveness probe (seconds)</td>
		</tr>
		<tr>
			<td>nameOverride</td>
			<td>string</td>
			<td><pre lang="json">
""
</pre>
</td>
			<td>Override the name of the chart</td>
		</tr>
		<tr>
			<td>namespaceOverride</td>
			<td>string</td>
			<td><pre lang="json">
""
</pre>
</td>
			<td>Override the namespace for the release</td>
		</tr>
		<tr>
			<td>networkPolicy.allowExternal</td>
			<td>bool</td>
			<td><pre lang="json">
false
</pre>
</td>
			<td>Allow external traffic without requiring a "-client" label</td>
		</tr>
		<tr>
			<td>networkPolicy.enabled</td>
			<td>bool</td>
			<td><pre lang="json">
false
</pre>
</td>
			<td>Create a network policy to restrict traffic to pods within the same namespace that include the label `<release>-client: true`.</td>
		</tr>
		<tr>
			<td>nodeSelector</td>
			<td>object</td>
			<td><pre lang="json">
{}
</pre>
</td>
			<td>Node selector for pod scheduling</td>
		</tr>
		<tr>
			<td>podAnnotations</td>
			<td>object</td>
			<td><pre lang="json">
{}
</pre>
</td>
			<td>Annotations to add to the Tika pods</td>
		</tr>
		<tr>
			<td>podSecurityContext</td>
			<td>object</td>
			<td><pre lang="json">
{}
</pre>
</td>
			<td></td>
		</tr>
		<tr>
			<td>readinessProbe.failureThreshold</td>
			<td>int</td>
			<td><pre lang="json">
20
</pre>
</td>
			<td>Number of failed readiness probes before marking pod as not ready</td>
		</tr>
		<tr>
			<td>readinessProbe.initialDelaySeconds</td>
			<td>int</td>
			<td><pre lang="json">
15
</pre>
</td>
			<td>Initial delay before starting readiness probe (seconds)</td>
		</tr>
		<tr>
			<td>readinessProbe.periodSeconds</td>
			<td>int</td>
			<td><pre lang="json">
5
</pre>
</td>
			<td>Interval between readiness probes (seconds)</td>
		</tr>
		<tr>
			<td>readinessProbe.scheme</td>
			<td>string</td>
			<td><pre lang="json">
"HTTP"
</pre>
</td>
			<td>Scheme for readiness probe (HTTP or HTTPS)</td>
		</tr>
		<tr>
			<td>readinessProbe.timeoutSeconds</td>
			<td>int</td>
			<td><pre lang="json">
30
</pre>
</td>
			<td>Timeout for readiness probe (seconds)</td>
		</tr>
		<tr>
			<td>replicaCount</td>
			<td>int</td>
			<td><pre lang="json">
1
</pre>
</td>
			<td>Number of Tika pod replicas to deploy</td>
		</tr>
		<tr>
			<td>resources.limits</td>
			<td>object</td>
			<td><pre lang="json">
{
  "cpu": "2",
  "memory": "2000Mi"
}
</pre>
</td>
			<td>Resource limits for the Tika container</td>
		</tr>
		<tr>
			<td>resources.requests</td>
			<td>object</td>
			<td><pre lang="json">
{
  "cpu": "1",
  "memory": "1500Mi"
}
</pre>
</td>
			<td>Resource requests for the Tika container</td>
		</tr>
		<tr>
			<td>securityContext.allowPrivilegeEscalation</td>
			<td>bool</td>
			<td><pre lang="json">
true
</pre>
</td>
			<td>Allow privilege escalation for the container</td>
		</tr>
		<tr>
			<td>securityContext.capabilities.drop</td>
			<td>list</td>
			<td><pre lang="json">
[
  "ALL"
]
</pre>
</td>
			<td>Capabilities to drop for the container</td>
		</tr>
		<tr>
			<td>securityContext.readOnlyRootFilesystem</td>
			<td>bool</td>
			<td><pre lang="json">
true
</pre>
</td>
			<td>Run container with read-only root filesystem</td>
		</tr>
		<tr>
			<td>securityContext.runAsGroup</td>
			<td>int</td>
			<td><pre lang="json">
35002
</pre>
</td>
			<td>Group ID to run the container</td>
		</tr>
		<tr>
			<td>securityContext.runAsNonRoot</td>
			<td>bool</td>
			<td><pre lang="json">
true
</pre>
</td>
			<td>Run container as non-root user</td>
		</tr>
		<tr>
			<td>securityContext.runAsUser</td>
			<td>int</td>
			<td><pre lang="json">
35002
</pre>
</td>
			<td>User ID to run the container</td>
		</tr>
		<tr>
			<td>service.port</td>
			<td>int</td>
			<td><pre lang="json">
9998
</pre>
</td>
			<td>Port for the Tika service</td>
		</tr>
		<tr>
			<td>service.type</td>
			<td>string</td>
			<td><pre lang="json">
"ClusterIP"
</pre>
</td>
			<td>Type of Kubernetes service to expose Tika</td>
		</tr>
		<tr>
			<td>serviceAccount.annotations</td>
			<td>object</td>
			<td><pre lang="json">
{}
</pre>
</td>
			<td>Annotations to add to the service account</td>
		</tr>
		<tr>
			<td>serviceAccount.create</td>
			<td>bool</td>
			<td><pre lang="json">
true
</pre>
</td>
			<td>Specifies whether a service account should be created</td>
		</tr>
		<tr>
			<td>serviceAccount.name</td>
			<td>string</td>
			<td><pre lang="json">
""
</pre>
</td>
			<td>The name of the service account to use; if not set and create is true, a name is generated</td>
		</tr>
		<tr>
			<td>tolerations</td>
			<td>list</td>
			<td><pre lang="json">
[]
</pre>
</td>
			<td>Tolerations for pod scheduling</td>
		</tr>
		<tr>
			<td>topologySpreadConstraints</td>
			<td>list</td>
			<td><pre lang="json">
[]
</pre>
</td>
			<td>Control how Pods are spread across the cluster</td>
		</tr>
	</tbody>
</table>

## Testing

```
helm plugin install https://github.com/helm-unittest/helm-unittest.git
helm unittest .
```

See [helm-unittest][] for canonical documentation.

## Contributing

Please check [CONTRIBUTING][] before any contribution or for any questions
about our development and testing process.

## More Information

For more infomation on Apache Tika Server, go to the [Apache Tika Server documentation][].

For more information on Apache Tika, go to the official [Apache Tika][] project website.

For more information on the Apache Software Foundation, go to the [Apache Software Foundation][] website.

# License
The code is licensed permissively under the [Apache License v2.0][].

## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| lewismc | <lewismc@apache.org> | <https://github.com/lewismc> |
| stijnbrouwers |  | <https://github.com/stijnbrouwers> |
| philipsoutham |  | <https://github.com/philipsoutham> |
| frascu |  | <https://github.com/frascu> |
| euven |  | <https://github.com/euven> |
| ps0uth |  | <https://github.com/ps0uth> |
| ahilmathew |  | <https://github.com/ahilmathew> |
| aidanthewiz |  | <https://github.com/aidanthewiz> |
| bartek |  | <https://github.com/bartek> |
| CiraciNicolo |  | <https://github.com/CiraciNicolo> |
| amalucelli |  | <https://github.com/amalucelli> |
| thatmlopsguy |  | <https://github.com/thatmlopsguy> |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.14.2](https://github.com/norwoodj/helm-docs/releases/v1.14.2)

[Apache License v2.0]: https://www.apache.org/licenses/LICENSE-2.0.html
[Apache Software Foundation]: http://apache.org
[Apache Tika]: https://tika.apache.org
[Apache Tika Server documentation]: https://cwiki.apache.org/confluence/display/TIKA/TikaServer
[BREAKING_CHANGES.md]: https://github.com/apache/tika-helm/blob/master/BREAKING_CHANGES.md
[CHANGELOG.md]: https://github.com/apache/tika-helm/blob/master/CHANGELOG.md
[CONTRIBUTING]: https://github.com/apache/tika#contributing-via-github
[helm-unittest]: https://github.com/helm-unittest/helm-unittest
[Tika Docker image]: https://hub.docker.com/r/apache/tika/tags?page=1&ordering=last_updated
