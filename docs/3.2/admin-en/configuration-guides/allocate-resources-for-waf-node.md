# Allocating Resources for Wallarm Node

The amount of memory allocated for the filtering node determines the quality and speed of request processing. These instructions describe the recommendations for filtering node memory allocation.

In a filtering node there are two main memory consumers:

* [Tarantool](#tarantool), also called **postanalytics module**. This is the local data analytics backend and the primary memory consumer in a filtering node.
* [NGINX](#nginx) is the main filtering node and reverse proxy component.

## Tarantool

--8<-- "../include/allocate-resources-for-waf-node/tarantool-memory.md"

### Allocating Resources in Kubernetes Ingress Controller

--8<-- "../include/allocate-resources-for-waf-node/tarantool-memory-ingress-controller.md"

### Allocating Resources in Other Deployment Options

The sizing of Tarantool memory is controlled using the `SLAB_ALLOC_ARENA` attribute in the `/etc/default/wallarm-tarantool` configuration file. To allocate memory:

<ol start="1"><li>Open for editing the configuration file of Tarantool:</li></ol>

=== "Debian 9.x (stretch/stretch-backports)"
    ```bash
    sudo vim /etc/default/wallarm-tarantool
    ```
=== "Debian 10.x (buster)"
    ```bash
    sudo vim /etc/default/wallarm-tarantool
    ```
=== "Ubuntu 18.04 LTS (bionic)"
    ```bash
    sudo vim /etc/default/wallarm-tarantool
    ```
=== "Ubuntu 20.04 LTS (focal)"
    ```bash
    sudo vim /etc/default/wallarm-tarantool
    ```
=== "CloudLinux OS 6.x"
    ```bash
    sudo vim /etc/sysconfig/wallarm-tarantool
    ```
=== "CentOS 7.x"
    ```bash
    sudo vim /etc/sysconfig/wallarm-tarantool
    ```
=== "Amazon Linux 2.0.2021x and lower"
    ```bash
    sudo vim /etc/sysconfig/wallarm-tarantool
    ```
=== "CentOS 8.x"
    ```bash
    sudo vim /etc/sysconfig/wallarm-tarantool
    ```

<ol start="2"><li>Set the <code>SLAB_ALLOC_ARENA</code> attribute to memory size. The value can be an integer or a float (a dot <code>.</code> is a decimal separator). For example:</li></ol>

```
SLAB_ALLOC_ARENA=10.4
```

<ol start="3"><li>Restart Tarantool:</li></ol>

=== "Debian 9.x (stretch/stretch-backports)"
    ```bash
    sudo systemctl restart wallarm-tarantool
    ```
=== "Debian 10.x (buster)"
    ```bash
    sudo systemctl restart wallarm-tarantool
    ```
=== "Ubuntu 18.04 LTS (bionic)"
    ```bash
    sudo service wallarm-tarantool restart
    ```
=== "Ubuntu 20.04 LTS (focal)"
    ```bash
    sudo service wallarm-tarantool restart
    ```
=== "CloudLinux OS 6.x"
    ```bash
    sudo systemctl restart wallarm-tarantool
    ```
=== "CentOS 7.x"
    ```bash
    sudo systemctl restart wallarm-tarantool
    ```
=== "Amazon Linux 2.0.2021x and lower"
    ```bash
    sudo systemctl restart wallarm-tarantool
    ```
=== "CentOS 8.x"
    ```bash
    sudo systemctl restart wallarm-tarantool
    ```

To learn how long a Tarantool instance is capable of keeping traffic details with the current level of filtering node load, you can use the [`wallarm-tarantool/gauge-timeframe_size`](../monitoring/available-metrics.md#time-of-storing-requests-in-the-postanalytics-module-in-seconds) monitoring metric.

## NGINX

NGINX memory consumption depends on many factors. On average it can be estimated as the following:

```
Number of concurrent request * Average request size * 3
```

For example:

* Filtering node is processing at peak 10000 concurrent requests,
* average request size is 5 kB.

The NGINX memory consumption can be estimated as follows:

```
10000 * 5 kB * 3 = 150000 kB (or ~150 MB)
```

**To allocate the amount of memory:**

* for the NGINX Ingress controller pod (`ingress-controller`), configure the following sections in the `values.yaml` file by using the `--set` option of `helm install` or `helm upgrade`:
    ```
    controller:
      resources:
        limits:
          cpu: 1000m
          memory: 1640Mi
        requests:
          cpu: 1000m
          memory: 1640Mi
    ```

    Example of commands changing the parameters:

    === "Ingress controller installation"
        ```bash
        helm install --set controller.resources.limits.cpu='2000m',controller.resources.limits.memory='3280Mi' <INGRESS_CONTROLLER_NAME> wallarm/wallarm-ingress -n <KUBERNETES_NAMESPACE>
        ```

        There are also [other parameters](../configure-kubernetes-en.md#additional-settings-for-helm-chart) required for correct Ingress controller installation. Please pass them in the `--set` option too.
    === "Updating Ingress controller parameters"
        ```bash
        helm upgrade --reuse-values --set controller.resources.limits.cpu='2000m',controller.resources.limits.memory='3280Mi' <INGRESS_CONTROLLER_NAME> wallarm/wallarm-ingress -n <KUBERNETES_NAMESPACE>
        ```

* for other deployment options, use the NGINX configuration files.

!!! info "Recommendations from the CPU utilization perspective"
    When running in production mode, it is recommended to allocate at least one CPU core for the NGINX process and one core for the Tarantool process.
    
    Actual NGINX CPU utilization depends on many factors like RPS level, average size of request and response, number of LOM rules handled by the node, types and layers of employed data encodings like Base64 or data compression, etc. On average, one CPU core can handle about 500 RPS. In the majority of cases it is recommended to initially over-provision a filtering node, see the actual CPU and memory usage for real production traffic levels, and gradually reduce allocated resources to a reasonable level (with at least 2x headroom for traffic spikes and node redundancy).
