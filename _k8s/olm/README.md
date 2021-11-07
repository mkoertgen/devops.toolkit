# Operator Lifecycle Manager (OLM)

> [OLM](https://github.com/operator-framework/operator-lifecycle-manager) extends Kubernetes to provide a declarative way to install, manage, and upgrade Operators and their dependencies in a cluster. ...

## Getting Started

See [QuickStart](https://olm.operatorframework.io/docs/getting-started/)

Install OLM operator from GitHub Releases [operator-lifecycle-manager/releases](https://github.com/operator-framework/operator-lifecycle-manager/releases), e.g.

```shell
kubectl apply -f https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.18.3/crds.yaml
kubectl apply -f https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.18.3/olm.yaml
```

## Installing Operators using OLM

Example to install the [tektoncd-operator](https://github.com/tektoncd/operator)

```shell
# Get the operators channels & current cluster source version (csv)
$ kubectl get packagemanifest/tektoncd-operator -n olm -o=jsonpath="{range .status.channels[*]}{@.name}{'\t'}{@.currentCSV}{'\n'}"
alpha   tektoncd-operator.v0.23.0-2

# Install directly from operatorhub, https://operatorhub.io/operator/tektoncd-operator
# This Operator will be installed in the "operators" namespace and will be usable from all namespaces in the cluster.
$ kubectl create -f https://operatorhub.io/install/tektoncd-operator.yaml
subscription.operators.coreos.com/my-tektoncd-operator created

# Get subscriptions
$ kubectl get sub -n operators
NAME                   PACKAGE             SOURCE                  CHANNEL
my-tektoncd-operator   tektoncd-operator   operatorhubio-catalog   alpha

# After install, watch your operator come up using next command.
$ kubectl get csv -n operators
NAME                          DISPLAY             VERSION    REPLACES   PHASE
tektoncd-operator.v0.23.0-2   Tektoncd Operator   0.23.0-2              Succeeded
```

When building images with tekton & kaniko we will need a (private) registry, e.g.

- [GitLab Container Registry](https://docs.gitlab.com/ee/user/packages/container_registry/)
- [GitHub](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry)

## Available operators

List available operators (review e.g. by [capability level](https://sdk.operatorframework.io/docs/advanced-topics/operator-capabilities/operator-capabilities/))

```shell
$ kubectl get packagemanifest -n olm --sort-by=.metadata.name
NAME                                       CATALOG               AGE
api-operator                               Community Operators   8m58s # wso2: https://github.com/wso2/k8s-api-operator/tree/v1.2.2#quick-start-guide
apicast-community-operator                 Community Operators   8m58s # apicast (api gateway built on top of nginx): https://github.com/3scale/apicast-operator
...
appsody-operator                           Community Operators   8m58s # Superseded by odo.dev: https://odo.dev/, https://odo.dev/https://github.com/openshift/odo, https://github.com/appsody/appsody
aqua                                       Community Operators   8m58s # https://www.aquasec.com/
argocd-operator                            Community Operators   8m58s # https://argocd-operator.readthedocs.io/
...
azure-service-operator                     Community Operators   8m58s # https://github.com/Azure/azure-service-operator
banzaicloud-kafka-operator                 Community Operators   8m58s
camel-k                                    Community Operators   8m58s # https://github.com/apache/camel-k
cassandra-operator                         Community Operators   8m58s # https://github.com/datastax/cass-operator
chaosblade-operator                        Community Operators   8m58s # Chaosblade-operator: A Chaos Engineering Tool for Cloud-native, cf.:
cloud-native-postgresql                    Community Operators   8m58s
cluster-manager                            Community Operators   8m58s
cockroachdb                                Community Operators   8m58s # unstable, https://github.com/cockroachdb/cockroach, https://github.com/cockroachdb/cockroach-operator
community-kubevirt-hyperconverged          Community Operators   8m58s
couchbase-enterprise                       Community Operators   8m58s
couchdb-operator                           Community Operators   8m58s
druid-operator                             Community Operators   8m58s # Apache Druid (realtime analytics), https://github.com/apache/druid, https://github.com/druid-io/druid-operator
dynatrace-operator                         Community Operators   8m58s # https://github.com/Dynatrace/dynatrace-operator
eclipse-che                                Community Operators   8m58s # https://www.eclipse.org/che/ (k8s native ide), https://github.com/eclipse-che/che-operator
elastic-cloud-eck                          Community Operators   8m58s # https://www.elastic.co/guide/en/cloud-on-k8s
elastic-phenix-operator                    Community Operators   8m58s # based on eck, ilm/index management: https://github.com/Carrefour-Group/elastic-phenix-operator
enmasse                                    Community Operators   8m58s # messaging platform, https://github.com/EnMasseProject/enmasse
esindex-operator                           Community Operators   8m58s
etcd                                       Community Operators   8m58s # https://github.com/coreos/etcd-operator
eunomia                                    Community Operators   8m58s # GitOps operator, https://github.com/KohlsTechnology/eunomia
falco                                      Community Operators   8m58s # Archived. SysDig Falco operator: https://github.com/falcosecurity/falco-operator
federatorai                                Community Operators   8m58s
flagsmith                                  Community Operators   8m58s # immature. Feature Flags Service
flux                                       Community Operators   8m58s # GitOps, flux v2, https://github.com/fluxcd/flux
grafana-operator                           Community Operators   8m58s # https://github.com/integr8ly/grafana-operator
halkyon                                    Community Operators   8m58s
ham-deploy                                 Community Operators   8m58s
hawkbit-operator                           Community Operators   8m58s
hazelcast-enterprise-operator              Community Operators   8m58s
hazelcast-jet-enterprise-operator          Community Operators   8m58s
hazelcast-jet-operator                     Community Operators   8m58s
hazelcast-operator                         Community Operators   8m58s
hedvig-operator                            Community Operators   8m58s
hive-operator                              Community Operators   8m58s
hpa-operator                               Community Operators   8m58s # https://github.com/banzaicloud/hpa-operator
hpe-csi-operator                           Community Operators   8m58s
hpe-ezmeral-csi-operator                   Community Operators   8m58s
ibm-application-gateway-operator           Community Operators   8m58s
ibm-block-csi-operator-community           Community Operators   8m58s
ibm-quantum-operator                       Community Operators   8m58s
ibm-spectrum-scale-csi-operator            Community Operators   8m58s
ibmcloud-iam-operator                      Community Operators   8m58s
ibmcloud-operator                          Community Operators   8m58s
infinispan                                 Community Operators   8m58s
instana-agent                              Community Operators   8m58s
integrity-shield-operator                  Community Operators   8m58s
intel-device-plugins-operator              Community Operators   8m58s
iot-simulator                              Community Operators   8m58s # https://github.com/ctron/iot-simulator-operator
istio                                      Community Operators   8m58s # https://github.com/istio/istio/tree/master/operator
istio-workspace-operator                   Community Operators   8m58s
jaeger                                     Community Operators   8m58s # https://github.com/jaegertracing/jaeger-operator
jenkins-operator                           Community Operators   8m58s
joget-tomcat-operator                      Community Operators   8m58s
keda                                       Community Operators   8m58s # event-driven pod autoscaler, https://github.com/kedacore/keda
keycloak-operator                          Community Operators   8m58s # Mature! https://github.com/keycloak/keycloak-operator, https://operatorhub.io/operator/keycloak-operator
kiali                                      Community Operators   8m58s # Service Mesh Management for Istio, https://kiali.io/
klusterlet                                 Community Operators   8m58s
knative-eventing-operator                  Community Operators   8m58s
knative-operator                           Community Operators   8m58s # COmbined operator https://github.com/knative/operator
knative-serving-operator                   Community Operators   8m58s
kogito-operator                            Community Operators   8m58s
kong                                       Community Operators   8m58s # Api gateway, https://github.com/Kong/kong-operator, https://github.com/Kong/kubernetes-ingress-controller/
kube-arangodb                              Community Operators   8m58s # Graph Database, kube-arangodb
kubefed-operator                           Community Operators   8m58s # k8s cluster federation, https://github.com/kubernetes-sigs/kubefed, https://github.com/openshift/kubefed-operator
kubeflow                                   Community Operators   8m58s # Machine Learning (ML) platform for Kubernetes, https://github.com/kubeflow/kfctl, https://www.kubeflow.org/
kubemod                                    Community Operators   8m58s
kubemq-operator                            Community Operators   8m58s # message queue broker for Kubernetes, https://github.com/kubemq-io/kubemq
kubernetes-imagepuller-operator            Community Operators   8m58s
kubernetes-nmstate-operator                Community Operators   8m58s
kubestone                                  Community Operators   8m58s # benchmarking Operator, https://operatorhub.io/operator/kubestone, https://github.com/xridge/kubestone
kubeturbo                                  Community Operators   8m58s # Turbonomics, App Service Management
lib-bucket-provisioner                     Community Operators   8m58s # Deprecated (k8s native bucket storage proposal), https://github.com/kube-object-storage/lib-bucket-provisioner
lightbend-console-operator                 Community Operators   8m58s # visualization and basic monitoring for Akka, Play, and Lagom, https://developer.lightbend.com/docs/console/current/
litmuschaos                                Community Operators   8m58s # k8s chaos engineering, https://github.com/litmuschaos/chaos-operator, https://github.com/litmuschaos/litmus
logging-operator                           Community Operators   8m58s # banzai cloud: fluentd logging operator, https://github.com/banzaicloud/logging-operator
mariadb-operator-app                       Community Operators   8m58s
mattermost-operator                        Community Operators   8m58s # MatterMost - OSS Alternative to Slack, Teams, https://github.com/mattermost/mattermost-operator, https://mattermost.com/
mcad-operator                              Community Operators   8m58s # IBM
metering-upstream                          Community Operators   8m58s # Deprecated
microcks                                   Community Operators   8m58s # k8s-native api mock & testing, https://github.com/microcks/microcks, https://microcks.io/documentation/installing/operator/
minio-operator                             Community Operators   8m58s # k8s-native S3-storage, https://github.com/minio/operator, https://github.com/minio/minio
mongodb-atlas-kubernetes                   Community Operators   8m58s # MongoDB Atlas (trial version): https://github.com/mongodb/mongodb-atlas-kubernetes
mongodb-enterprise                         Community Operators   8m58s # https://github.com/mongodb/mongodb-enterprise-kubernetes
multicluster-operators-subscription        Community Operators   8m58s # RedHat OSS multicluster and multicloud scenarios, https://open-cluster-management.io/
mysql                                      Community Operators   8m58s
myvirtualdirectory                         Community Operators   8m58s # LDAP, https://www.tremolosecurity.com/products/myvirtualdirectory, https://github.com/TremoloSecurity/MyVirtualDirectory
neuvector-operator                         Community Operators   8m58s # Kubernetes security platform, https://github.com/neuvector/neuvector-operator, https://neuvector.com/
nexus-operator-m88i                        Community Operators   8m58s # SonaType Nexus OSS Kubernetes
nfd-operator                               Community Operators   8m58s # OpenShift, Node Feature Discovery Operator, https://github.com/openshift/cluster-nfd-operator
node-healthcheck-operator                  Community Operators   8m58s
noobaa-operator                            Community Operators   8m58s # RedHat S3-Umbrella, Cloud Data Service, https://github.com/noobaa/noobaa-core, https://www.noobaa.io/
nsm-operator-registry                      Community Operators   8m58s # Service Mesh
nuxeo-operator                             Community Operators   8m58s # Nuxeo Content Management Platform, https://www.nuxeo.com/
oneagent                                   Community Operators   8m58s # DynaTrace OneAgent, https://github.com/Dynatrace/dynatrace-oneagent-operator
open-liberty                               Community Operators   8m58s # IBM, Open Liberty, Java MicroService Framework
openebs                                    Community Operators   8m58s # OpenEBS, CNCF storage solution for Kubernetes, https://openebs.io/, https://github.com/openebs/openebs
openshift-qiskit-operator                  Community Operators   8m58s # IBM, QiskitPlayground is a Jupyter notebook with the pre installed qiskit libraries
opentelemetry-operator                     Community Operators   8m58s # OpenTelemetry, https://github.com/open-telemetry/opentelemetry-operator
opsmx-spinnaker-operator                   Community Operators   8m58s # OpsMx, CD for OpenShift & Spinnaker, https://www.opsmx.com/
ovms-operator                              Community Operators   8m58s # Intel OpenVINO™ Machine Learning Model Server, https://github.com/openvinotoolkit/model_server
percona-server-mongodb-operator            Community Operators   8m58s # open-source enterprise MongoDB
percona-xtradb-cluster-operator            Community Operators   8m58s
planetscale                                Community Operators   8m58s # Serverless Database (commercial), https://www.planetscale.com/
pmem-csi-operator                          Community Operators   8m58s # Intel PMEM-CSI is a CSI storage driver for containers
poison-pill-operator                       Community Operators   8m58s # Kubernetes Self Remediation (AKA Poison Pill), https://www.openshift.com/blog/kubernetes-self-remediation-aka-poison-pill
portworx                                   Community Operators   8m58s # PortWorx Storage / Backup, https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/
portworx-essentials                        Community Operators   8m58s
postgres-operator                          Community Operators   8m58s
postgresql                                 Community Operators   8m58s
postgresql-operator                        Community Operators   8m58s
postgresql-operator-dev4devs-com           Community Operators   8m58s
prisma-cloud-compute-console-operator      Community Operators   8m58s # Prisma™ Cloud is a cloud native security platform, https://docs.paloaltonetworks.com/prisma/prisma-cloud.html
project-quay                               Community Operators   8m58s # RedHat, Project Quay, Image Registry, https://github.com/quay/quay, https://github.com/quay/quay-operator
project-quay-container-security-operator   Community Operators   8m58s
prometheus                                 Community Operators   8m58s # Beta, Prometheus Operator, https://github.com/prometheus-operator/prometheus-operator
prometheus-exporter-operator               Community Operators   8m58s # 3Scale, centralize the setup of 3rd party prometheus exporters (e.g. elasticsearch), https://github.com/3scale-ops/prometheus-exporter-operator
pystol                                     Community Operators   8m58s # Pystol, cloud-native fault injection platform,
radanalytics-spark                         Community Operators   8m58s # Managing Apache Spark clusters, https://operatorhub.io/operator/radanalytics-spark
redis-enterprise                           Community Operators   8m58s
redis-operator                             Community Operators   8m58s
ripsaw                                     Community Operators   8m58s # Ripsaw k8s benchmarking, https://github.com/cloud-bulldozer/benchmark-operator
robin-operator                             Community Operators   8m58s # Robin Cloud-Storage, https://robin.io/
rocketmq-operator                          Community Operators   8m58s # Apache RocketMQ is a distributed messaging and streaming platform, https://github.com/apache/rocketmq
rook-ceph                                  Community Operators   8m58s # Rook Cloud Storage https://rook.io/
rook-edgefs                                Community Operators   8m58s
rqlite-operator                            Community Operators   8m58s # rqlite is a lightweight, distributed relational database, which uses SQLite
runtime-component-operator                 Community Operators   8m58s
sap-btp-operator                           Community Operators   8m58s
sealed-secrets-operator-helm               Community Operators   8m58s # Bitnami Sealed Secrets, https://github.com/bitnami-labs/sealed-secrets
seldon-operator                            Community Operators   8m58s # Seldon Core: Blazing Fast, Industry-Ready ML, https://github.com/SeldonIO/seldon-core, https://www.seldon.io/
sematext                                   Community Operators   8m58s # SemaText Monitoring Agent, https://sematext.com/
service-binding-operator                   Community Operators   8m58s # RedHat https://github.com/redhat-developer/service-binding-operator
shipwright-operator                        Community Operators   8m58s # ShipWright (Tekton, Kaniko), k8s-native container building, https://github.com/shipwright-io/build, https://shipwright.io/
siddhi-operator                            Community Operators   8m58s # Siddhi is a cloud native Streaming and Complex Event Processing, https://github.com/siddhi-io/siddhi
skydive-operator                           Community Operators   8m58s # Skydive, oss real-time network topology and protocols analyzer, https://github.com/skydive-project/skydive
snapscheduler                              Community Operators   8m58s # scheduled snapshots for Kubernetes, https://github.com/backube/snapscheduler
snyk-operator                              Community Operators   8m58s # Security, snyk/kubernetes-monitor, https://github.com/snyk/kubernetes-monitor
spark-gcp                                  Community Operators   8m58s # Apache Spark on GCP
spinnaker-operator                         Community Operators   8m58s # Spinnaker, Cloud-Native Continuous Delivery, https://spinnaker.io/
splunk                                     Community Operators   8m58s # Splunk, https://github.com/splunk/splunk-operator
starboard-operator                         Community Operators   8m58s # Starboard k8s Security, https://aquasecurity.github.io/starboard
steerd-presto-operator                     Community Operators   8m58s # Facebook Presto SQL, https://github.com/falarica/steerd-presto-operator, https://prestodb.io/
storageos                                  Community Operators   8m58s # StorageOS storage platform, https://storageos.com/
strimzi-kafka-operator                     Community Operators   8m58s # Kafka on k8s, https://strimzi.io/
submariner                                 Community Operators   8m58s # Submariner cross-cluster networking, https://submariner.io/
syndesis                                   Community Operators   8m58s # Syndesis, low-code, data integration as a service, https://syndesis.io/, seems to be OpenShift only
sysdig                                     Community Operators   8m58s # Sysdig Agent Operator, Security, https://sysdig.com/
t8c                                        Community Operators   8m58s # Turbonomic Platform Operator, IBM, AI-Powered Application Management, https://www.turbonomic.com/
tektoncd-operator                          Community Operators   8m58s # Tekton.Dev, Cloud-Native CI/CD https://tekton.dev/,  https://github.com/tektoncd/operator
tidb-operator                              Community Operators   8m58s # TiDB (TitaniumDB), oss, Hybrid Transactional and Analytical Processing (HTAP), https://docs.pingcap.com/tidb/stable
topolvm-operator                           Community Operators   8m58s # TopoLVM Storage,
traefikee-operator                         Community Operators   8m58s # Traefik Enterprise, https://traefik.io/traefik-enterprise/
universal-crossplane                       Community Operators   8m58s # Crossplane Enterprise, API Composability
varnish-operator                           Community Operators   8m58s # IBM Varnish
vault                                      Community Operators   8m58s # Banzai Cloud, Vault operator, https://operatorhub.io/operator/vault
vault-helm                                 Community Operators   8m58s
victoriametrics-operator                   Community Operators   8m58s # Similar to Prometheus, VictoriaMetrics is a fast, cost-effective and scalable monitoring solution and time series database
virt-gateway-operator                      Community Operators   8m58s # https://github.com/yaacov/kube-gateway-operator
wavefront                                  Community Operators   8m58s # VmWare, Wavefront is a high-performance streaming analytics platform for monitoring and optimizing
wildfly                                    Community Operators   8m58s # Java WildFly, https://github.com/wildfly/wildfly-operator
wso2am-operator                            Community Operators   8m58s # wso2 API Manager, https://github.com/wso2/k8s-wso2am-operator
xrootd-operator                            Community Operators   8m58s # XRootD, Data Access Service, https://xrootd.slac.stanford.edu/
yaks                                       Community Operators   8m58s # Yaks - Cloud Native BDD testing, https://github.com/citrusframework/yaks
yugabyte-operator                          Community Operators   8m58s # Yugabyte - Distributed SQL Database, https://github.com/yugabyte/yugabyte-operator
zoperator                                  Community Operators   8m58s # Zadara Storage, https://www.zadara.com/
```
