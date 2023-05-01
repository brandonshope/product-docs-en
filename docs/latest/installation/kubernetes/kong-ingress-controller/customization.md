# Customizing Kong Ingress Controller with Integrated Wallarm Services

This article instructs you on the safe and effective customization of the [Kong Ingress Controller with integrated Wallarm services](deployment.md).

## Configuration area

Kong Ingress Controller with integrated Wallarm services is based on the standard Kubernetes components, thus the solution configuration is largely similar to the Kubernetes stack configuration.

You can configure the solution as follows:

* Globally via `values.yaml` - it allows setting up the general deployment configuration, the Kong API Gateway and some basic Wallarm settings. These settings apply to all Ingress resources the solution proxies traffic to.
* Via the Ingress annotations - it allows fine-tuning the Wallarm settings on a per-Ingress basis.

    !!! warning "Annotation support"
        Ingress annotation is supported only by the solution based on the Open-Source Kong Ingress controller. [The list of supported annotations is limited](#fine-tuning-of-traffic-analysis-via-ingress-annotations-only-for-the-open-source-edition).
* Via the Wallarm Console UI - it allows fine-tuning the Wallarm settings.

## Configuration of Kong API Gateway

Configuration of Kong Ingress Controller for Kong API Gateway is set by the [default Helm chart values](https://github.com/wallarm/kong-charts/blob/main/charts/kong/values.yaml). This configuration can be overridden by the `values.yaml` file provided by the user during `helm install` or `helm upgrade`.

To customize the default Helm chart values, learn the [official instructions on the Kong and Ingress Controller configuration](https://github.com/Kong/charts/tree/main/charts/kong#configuration).

## Configuration of the Wallarm layer

You can configure the Wallarm layer of the solution as follows:

* Set basic configuration via `values.yaml`: connection to the Wallarm Cloud, resource allocation, fallbacks.
* Fine-tune traffic analysis on a per-Ingress basis via annotations (only for the Open-Source edition): traffic filtration mode, application management, multitenancy configuration, etc.
* Fine-tune traffic analysis via the Wallarm Console UI: traffic filtration mode, notifications about security events, request source management, mask sensitive data, allow certain attack types, etc.

### Basic configuration via `values.yaml`

The default `values.yaml` file provides the following Wallarm configuration:

```yaml
wallarm:
  image:
    tag: "<WALLARM_NODE_IMAGE_TAG>"
  enabled: true
  apiHost: api.wallarm.com
  apiPort: 443
  apiSSL: true
  token: ""
  fallback: "on"
  tarantool:
    kind: Deployment
    service:
      annotations: {}
    replicaCount: 1
    arena: "0.2"
    livenessProbe:
      failureThreshold: 3
      initialDelaySeconds: 10
      periodSeconds: 10
      successThreshold: 1
      timeoutSeconds: 1
    resources: {}
  heartbeat:
    resources: {}
  wallarm-appstructure:
    resources: {}
  wallarm-antibot:
    resources: {}
  metrics:
    port: 18080
    enabled: false

    service:
      annotations:
        prometheus.io/scrape: "true"
        prometheus.io/path: /wallarm-metrics
        prometheus.io/port: "18080"

      # clusterIP: ""

      ## -- List of IP addresses at which the stats-exporter service is available
      ## Ref: https://kubernetes.io/docs/user-guide/services/#external-ips
      ##
      externalIPs: []

      # loadBalancerIP: ""
      loadBalancerSourceRanges: []
      servicePort: 18080
      type: ClusterIP
      # externalTrafficPolicy: ""
      # nodePort: ""
  addnode:
    resources: {}
  cron:
    jobs:
      exportEnvironment:
        schedule: "0 */1 * * *"
        timeout: 10m
      exportAttacks:
        schedule: "* * * * *"
        timeout: 3h
      exportCounters:
        schedule: "* * * * *"
        timeout: 11m
      bruteDetect:
        schedule: "* * * * *"
        timeout: 6m
      syncIpLists:
        schedule: "* * * * *"
        timeout: 3h
      exportMetrics:
        schedule: "* * * * *"
        timeout: 3h
      syncIpListsSource:
        schedule: "*/5 * * * *"
        timeout: 3h
      syncMarkers:
        schedule: "* * * * *"
        timeout: 1h
    resources: {}
  exportenv:
    resources: {}
  synccloud:
    wallarm_syncnode_interval_sec: 120
    resources: {}
  collectd:
    resources: {}
```

The main parameters you may need to change are:

| Parameter | Description | Default value |
| --- | --- | --- |
| `wallarm.enabled` | Allows you to enable or disable the Wallarm layer. | `true` |
| `wallarm.apiHost` | Wallarm API server:<ul><li>`us1.api.wallarm.com` for the US Cloud</li><li>`api.wallarm.com` for the EU Cloud</li></ul> | `api.wallarm.com` |
| `wallarm.token` | Wallarm node token. **Required**. | Empty |
| `wallarm.fallback` | Whether to run the Kong API Gateway services if the Wallarm service start failed. | `on`
| `wallarm.tarantool.replicaCount` | The number of running pods for the Wallarm postanalytics module that is the local data analytics backend for the solution. | `1`
| `wallarm.tarantool.arena` | Specifies the amount of memory allocated for the Wallarm postanalytics module. It is recommended to set up a value sufficient to store request data for the last 5-15 minutes. | `0.2`
| `wallarm.metrics.enabled` | This switch toggles information and metrics collection. If [Prometheus](https://github.com/helm/charts/tree/master/stable/prometheus) is installed in the Kubernetes cluster, no additional configuration is required. | `false`

Other parameters come with default values and rarely need to be changed.

### Fine-tuning of traffic analysis via Ingress annotations (only for the Open-Source edition)

Below is the list of annotations supported in the Open-Source Kong Ingress controller with integrated Wallarm services.

!!! info "Priorities of global and per-Ingress's settings"
    Per-Ingress's annotations take precedence over Helm chart values.

Before using an annotation, please add the `wallarm.com/` prefix to it, e.g.:

```bash
wallarm.com/wallarm-mode: block
```

| Annotation | Description | 
|----------- |------------ |
| `wallarm-mode` | [Traffic filtration mode](../../../admin-en/configure-wallarm-mode.md): `off` (default), `monitoring`, `safe_blocking`, or `block`. |
| `wallarm-application` | [Wallarm application ID](../../../user-guides/settings/applications.md). The value can be a positive integer except for `0`. |
| `wallarm-parse-response` | Whether to analyze the application responses for attacks: `true` (default) or `false`. Response analysis is required for vulnerability detection during [passive detection](../../../about-wallarm/detecting-vulnerabilities.md#passive-detection) and [active threat verification](../../../about-wallarm/detecting-vulnerabilities.md#active-threat-verification). |
| `wallarm-parse-websocket` | Wallarm has full WebSockets support. By default, the WebSockets' messages are not analyzed for attacks. To force the feature, activate the API Security [subscription plan](../../../about-wallarm/subscription-plans.md#subscription-plans) and use this annotation: `true` or `false` (default). |
| `wallarm-unpack-response` | Whether to decompress compressed data returned in the application response: `true` (default) or `false`. |
| `wallarm-partner-client-uuid` | Unique identifier of the tenant for the [multi-tenant](../../multi-tenant/overview.md) Wallarm node. The value should be a string in the UUID format, e.g. `123e4567-e89b-12d3-a456-426614174000`.<br><br>Know how to:<ul><li>[Get the UUID of the tenant during tenant creation](../../multi-tenant/configure-accounts.md#step-3-create-the-tenant-via-the-wallarm-api)</li><li>[Get the list of UUIDs of existing tenants](../../../updating-migrating/older-versions/multi-tenant.md#get-uuids-of-your-tenants)</li></ul> |

### Fine-tuning of traffic analysis via the Wallarm Console UI

The Wallarm Console UI enables you to fine-tune the traffic analysis performed by the Wallarm layer as follows:

* Configure the traffic filtration mode
    
    Once the [solution is deployed](deployment.md), it starts filtering all incoming requests in the **monitoring** [mode](../../../admin-en/configure-wallarm-mode.md#available-filtration-modes).

    The Wallarm Console UI enables you to change the mode:

    * [Globally for all incoming requests](../../../user-guides/settings/general.md)
    * On a per-Ingress basis using the [rule](../../../user-guides/rules/wallarm-mode-rule.md)

    !!! info "Priorities of per-Ingress' settings and the ones specified in the Wallarm Console UI"
        If the mode for the Kong Open-Source based solution is specified via the `wallarm-mode` annotation and the Wallarm Console UI, the last will take precedence over the annotation.
* Set up [notifications on security events](../../../user-guides/settings/integrations/integrations-intro.md)
* [Manage access to APIs by the request sources](../../../user-guides/ip-lists/overview.md)
* [Customize traffic filtration rules](../../../user-guides/rules/intro.md)
