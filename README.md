# Redis Terraform module
This module to deploy [Redis] in k8s cluster on GCP.  
Module uses [stable/redis] chart.

## Configuration

The following table lists the configurable parameters of the Redis chart and their default values.

| Parameter                                  | Description                                                                                                    | Default                                              |
|--------------------------------------------|----------------------------------------------------------------------------------------------------------------|------------------------------------------------------|
| `global.imageRegistry`                     | Global Docker image registry                                                                                   | `nil`                                                |
| `image.registry`                           | Redis Image registry                                                                                           | `docker.io`                                          |
| `image.repository`                         | Redis Image name                                                                                               | `bitnami/redis`                                      |
| `image.tag`                                | Redis Image tag                                                                                                | `{VERSION}`                                          |
| `image.pullPolicy`                         | Image pull policy                                                                                              | `Always`                                             |
| `image.pullSecrets`                        | Specify docker-registry secret names as an array                                                               | `nil`                                                |
| `cluster.enabled`                          | Use master-slave topology                                                                                      | `true`                                               |
| `cluster.slaveCount`                       | Number of slaves                                                                                               | `1`                                                  |
| `existingSecret`                           | Name of existing secret object (for password authentication)                                                   | `nil`                                                |
| `usePassword`                              | Use password                                                                                                   | `true`                                               |
| `usePasswordFile`                              | Mount passwords as files instead of environment variables                                                                                                   | `false`                                               |
| `password`                                 | Redis password (ignored if existingSecret set)                                                                 | Randomly generated                                   |
| `configmap`                                | Redis configuration file to be used                                                                            | `nil`                                                |
| `networkPolicy.enabled`                    | Enable NetworkPolicy                                                                                           | `false`                                              |
| `networkPolicy.allowExternal`              | Don't require client label for connections                                                                     | `true`                                               |
| `serviceAccount.create`                    | Specifies whether a ServiceAccount should be created                                                           | `false`                                              |
| `serviceAccount.name`                      | The name of the ServiceAccount to create                                                                       | Generated using the fullname template                |
| `rbac.create`                              | Specifies whether RBAC resources should be created                                                             | `false`                                              |
| `rbac.role.rules`                          | Rules to create                                                                                                | `[]`                                                 |
| `metrics.enabled`                          | Start a side-car prometheus exporter                                                                           | `false`                                              |
| `metrics.image.registry`                   | Redis exporter image registry                                                                                  | `docker.io`                                          |
| `metrics.image.repository`                 | Redis exporter image name                                                                                      | `oliver006/redis_exporter`                           |
| `metrics.image.tag`                        | Redis exporter image tag                                                                                       | `v0.20.2`                                            |
| `metrics.image.pullPolicy`                 | Image pull policy                                                                                              | `IfNotPresent`                                       |
| `metrics.image.pullSecrets`                | Specify docker-registry secret names as an array                                                               | `nil`                                                |
| `metrics.extraArgs`                        | Extra arguments for the binary; possible values [here](https://github.com/oliver006/redis_exporter#flags)      | {}                                                   |
| `metrics.podLabels`                        | Additional labels for Metrics exporter pod                                                                     | {}                                                   |
| `metrics.podAnnotations`                   | Additional annotations for Metrics exporter pod                                                                | {}                                                   |
| `metrics.service.type`                     | Kubernetes Service type (redis metrics)                                                                        | `ClusterIP`                                          |
| `metrics.service.annotations`              | Annotations for the services to monitor  (redis master and redis slave service)                                | {}                                                   |
| `metrics.service.loadBalancerIP`           | loadBalancerIP if redis metrics service type is `LoadBalancer`                                                 | `nil`                                                |
| `metrics.resources`                        | Exporter resource requests/limit                                                                               | Memory: `256Mi`, CPU: `100m`                         |
| `metrics.serviceMonitor.enabled`           | if `true`, creates a Prometheus Operator ServiceMonitor (also requires `metrics.enabled` to be `true`)         | `false`                                              |
| `metrics.serviceMonitor.namespace`         | Optional namespace which Prometheus is running in                                                              | `nil`                                                |
| `metrics.serviceMonitor.interval`          | How frequently to scrape metrics (use by default, falling back to Prometheus' default)                         |  `nil`                                               |
| `metrics.serviceMonitor.selector`          | Default to kube-prometheus install (CoreOS recommended), but should be set according to Prometheus install     | `{ prometheus: kube-prometheus }`                    |
| `metrics.priorityClassName`                | Metrics exporter pod priorityClassName                                                                         | {}                                                   |
| `persistence.existingClaim`                | Provide an existing PersistentVolumeClaim                                                                      | `nil`                                                |
| `master.persistence.enabled`               | Use a PVC to persist data (master node)                                                                        | `true`                                               |
| `master.persistence.path`                  | Path to mount the volume at, to use other images                                                               | `/data`                                              |
| `master.persistence.subPath`               | Subdirectory of the volume to mount at                                                                         | `""`                                                 |
| `master.persistence.storageClass`          | Storage class of backing PVC                                                                                   | `generic`                                            |
| `master.persistence.accessModes`           | Persistent Volume Access Modes                                                                                 | `[ReadWriteOnce]`                                    |
| `master.persistence.size`                  | Size of data volume                                                                                            | `8Gi`                                                |
| `master.statefulset.updateStrategy`        | Update strategy for StatefulSet                                                                                | onDelete                                             |
| `master.statefulset.rollingUpdatePartition`| Partition update strategy                                                                                      | `nil`                                                |
| `master.podLabels`                         | Additional labels for Redis master pod                                                                         | {}                                                   |
| `master.podAnnotations`                    | Additional annotations for Redis master pod                                                                    | {}                                                   |
| `master.port`                              | Redis master port                                                                                              | `6379`                                               |
| `master.command`                           | Redis master entrypoint string. The command `redis-server` is executed if this is not provided.                  | `/run.sh`                                                   |
| `master.disableCommands`                   | Array of Redis commands to disable (master)                                                                    | `["FLUSHDB", "FLUSHALL"]`                                   |
| `master.extraFlags`                        | Redis master additional command line flags                                                                     | []                                                   |
| `master.nodeSelector`                      | Redis master Node labels for pod assignment                                                                    | {"beta.kubernetes.io/arch": "amd64"}                 |
| `master.tolerations`                       | Toleration labels for Redis master pod assignment                                                              | []                                                   |
| `master.affinity`                          | Affinity settings for Redis master pod assignment                                                              | {}                                                   |
| `master.schedulerName`                     | Name of an alternate scheduler                                                                                 | `nil`                                                |
| `master.service.type`                      | Kubernetes Service type (redis master)                                                                         | `ClusterIP`                                          |
| `master.service.port`                      | Kubernetes Service port (redis master)                                                                         | `6379`                                               |
| `master.service.nodePort`                  | Kubernetes Service nodePort (redis master)                                                                     | `nil`                                                |
| `master.service.annotations`               | annotations for redis master service                                                                           | {}                                                   |
| `master.service.loadBalancerIP`            | loadBalancerIP if redis master service type is `LoadBalancer`                                                  | `nil`                                                |
| `master.securityContext.enabled`           | Enable security context (redis master pod)                                                                     | `true`                                               |
| `master.securityContext.fsGroup`           | Group ID for the container (redis master pod)                                                                  | `1001`                                               |
| `master.securityContext.runAsUser`         | User ID for the container (redis master pod)                                                                   | `1001`                                               |
| `master.resources`                         | Redis master CPU/Memory resource requests/limits                                                               | Memory: `256Mi`, CPU: `100m`                         |
| `master.livenessProbe.enabled`             | Turn on and off liveness probe (redis master pod)                                                              | `true`                                               |
| `master.livenessProbe.initialDelaySeconds` | Delay before liveness probe is initiated (redis master pod)                                                    | `30`                                                 |
| `master.livenessProbe.periodSeconds`       | How often to perform the probe (redis master pod)                                                              | `30`                                                 |
| `master.livenessProbe.timeoutSeconds`      | When the probe times out (redis master pod)                                                                    | `5`                                                  |
| `master.livenessProbe.successThreshold`    | Minimum consecutive successes for the probe to be considered successful after having failed (redis master pod) | `1`                                                  |
| `master.livenessProbe.failureThreshold`    | Minimum consecutive failures for the probe to be considered failed after having succeeded.                     | `5`                                                  |
| `master.readinessProbe.enabled`            | Turn on and off readiness probe (redis master pod)                                                             | `true`                                               |
| `master.readinessProbe.initialDelaySeconds`| Delay before readiness probe is initiated (redis master pod)                                                   | `5`                                                  |
| `master.readinessProbe.periodSeconds`      | How often to perform the probe (redis master pod)                                                              | `10`                                                 |
| `master.readinessProbe.timeoutSeconds`     | When the probe times out (redis master pod)                                                                    | `1`                                                  |
| `master.readinessProbe.successThreshold`   | Minimum consecutive successes for the probe to be considered successful after having failed (redis master pod) | `1`                                                  |
| `master.readinessProbe.failureThreshold`   | Minimum consecutive failures for the probe to be considered failed after having succeeded.                     | `5`                                                  |
| `master.priorityClassName`                 | Redis Master pod priorityClassName                                                                             | {}                                                   |
| `volumePermissions.enabled`         | Enable init container that changes volume permissions in the registry (for cases where the default k8s `runAsUser` and `fsUser` values do not work)                                                               | `false`                                          |
| `volumePermissions.image.registry`         | Init container volume-permissions image registry                                                               | `docker.io`                                          |
| `volumePermissions.image.repository`       | Init container volume-permissions image name                                                                   | `bitnami/minideb`                                    |
| `volumePermissions.image.tag`              | Init container volume-permissions image tag                                                                    | `latest`                                             |
| `volumePermissions.image.pullPolicy`       | Init container volume-permissions image pull policy                                                            | `IfNotPresent`                                       |
| `slave.service.type`                       | Kubernetes Service type (redis slave)                                                                          | `ClusterIP`                                          |
| `slave.service.nodePort`                   | Kubernetes Service nodePort (redis slave)                                                                      | `nil`                                                |
| `slave.service.annotations`                | annotations for redis slave service                                                                            | {}                                                   |
| `slave.service.loadBalancerIP`             | LoadBalancerIP if Redis slave service type is `LoadBalancer`                                                   | `nil`                                                |
| `slave.port`                               | Redis slave port                                                                                               | `master.port`                                        |
| `slave.command`                            | Redis slave entrypoint array. The docker image's ENTRYPOINT is used if this is not provided.                   | `master.command`                                     |
| `slave.disableCommands`                    | Array of Redis commands to disable (slave)                                                                     | `master.disableCommands`                             |
| `slave.extraFlags`                         | Redis slave additional command line flags                                                                      | `master.extraFlags`                                  |
| `slave.livenessProbe.enabled`              | Turn on and off liveness probe (redis slave pod)                                                               | `master.livenessProbe.enabled`                       |
| `slave.livenessProbe.initialDelaySeconds`  | Delay before liveness probe is initiated (redis slave pod)                                                     | `master.livenessProbe.initialDelaySeconds`           |
| `slave.livenessProbe.periodSeconds`        | How often to perform the probe (redis slave pod)                                                               | `master.livenessProbe.periodSeconds`                 |
| `slave.livenessProbe.timeoutSeconds`       | When the probe times out (redis slave pod)                                                                     | `master.livenessProbe.timeoutSeconds`                |
| `slave.livenessProbe.successThreshold`     | Minimum consecutive successes for the probe to be considered successful after having failed (redis slave pod)  | `master.livenessProbe.successThreshold`              |
| `slave.livenessProbe.failureThreshold`     | Minimum consecutive failures for the probe to be considered failed after having succeeded.                     | `master.livenessProbe.failureThreshold`              |
| `slave.readinessProbe.enabled`             | Turn on and off slave.readiness probe (redis slave pod)                                                        | `master.readinessProbe.enabled`                      |
| `slave.readinessProbe.initialDelaySeconds` | Delay before slave.readiness probe is initiated (redis slave pod)                                              | `master.readinessProbe.initialDelaySeconds`          |
| `slave.readinessProbe.periodSeconds`       | How often to perform the probe (redis slave pod)                                                               | `master.readinessProbe.periodSeconds`                |
| `slave.readinessProbe.timeoutSeconds`      | When the probe times out (redis slave pod)                                                                     | `master.readinessProbe.timeoutSeconds`               |
| `slave.readinessProbe.successThreshold`    | Minimum consecutive successes for the probe to be considered successful after having failed (redis slave pod)  | `master.readinessProbe.successThreshold`             |
| `slave.readinessProbe.failureThreshold`    | Minimum consecutive failures for the probe to be considered failed after having succeeded. (redis slave pod)   | `master.readinessProbe.failureThreshold`             |
| `slave.podLabels`                          | Additional labels for Redis slave pod                                                                          | `master.podLabels`                                   |
| `slave.podAnnotations`                     | Additional annotations for Redis slave pod                                                                     | `master.podAnnotations`                              |
| `slave.schedulerName`                      | Name of an alternate scheduler                                                                                 | `nil`                                                |
| `slave.securityContext.enabled`            | Enable security context (redis slave pod)                                                                      | `master.securityContext.enabled`                     |
| `slave.securityContext.fsGroup`            | Group ID for the container (redis slave pod)                                                                   | `master.securityContext.fsGroup`                     |
| `slave.securityContext.runAsUser`          | User ID for the container (redis slave pod)                                                                    | `master.securityContext.runAsUser`                   |
| `slave.resources`                          | Redis slave CPU/Memory resource requests/limits                                                                | `master.resources`                                   |
| `slave.affinity`                           | Enable node/pod affinity for slaves                                                                            | {}                                                   |
| `slave.priorityClassName`                  | Redis Slave pod priorityClassName                                                                              | {}                                                   |
| `sysctlImage.enabled`                      | Enable an init container to modify Kernel settings                                                             | `false`                                              |
| `sysctlImage.command`                      | sysctlImage command to execute                                                                                 | []                                                   |
| `sysctlImage.registry`                     | sysctlImage Init container registry                                                                            | `docker.io`                                          |
| `sysctlImage.repository`                   | sysctlImage Init container name                                                                                | `bitnami/minideb`                                    |
| `sysctlImage.tag`                          | sysctlImage Init container tag                                                                                 | `latest`                                             |
| `sysctlImage.pullPolicy`                   | sysctlImage Init container pull policy                                                                         | `Always`                                             |
| `sysctlImage.mountHostSys`                 | Mount the host `/sys` folder to `/host-sys`                                                                    | `false`                                              |

You can override those settings by providing `values.yaml` file or simple use default values.

---
For more information about chart and how to configure it visit [stable/redis]


[stable/redis]: https://github.com/helm/charts/tree/master/stable/redis
[Redis]: https://redis.io