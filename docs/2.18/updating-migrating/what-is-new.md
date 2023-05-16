# What is new in Wallarm node 2.18

## Breaking change

Since version 2.16.0-8 of the Wallarm node Docker image, the environment variable `WALLARM_ACL_ENABLE` passed to the [NGINX-based Docker container](../admin-en/installation-docker-en.md#run-the-container-passing-the-environment-variables) only accepts the value `true` or `false`.

!!! warning "Values `on` / `enabled` / `ok` / `yes`"
    The values `on` / `enabled` / `ok` / `yes` assigned to the variable `WALLARM_ACL_ENABLE` disable the IP blocking functionality. We recommend deploying the latest image version as described in the [instructions on running the Docker container](../admin-en/installation-docker-en.md) and passing the value `true` or `false` in this variable.

## Changes in supported installation platforms

* Added Ubuntu 20.04 LTS (focal) support

[See the full list of supported platforms →](../installation/supported-deployment-options.md)

## New features

* New variable `wallarm_attack_type_list` in the extended Wallarm node logging format. Attack types detected in the request are saved in this variable in text format.
    
    [More details on the variable `wallarm_attack_type_list` →](../admin-en/configure-logging.md#filter-node-variables)
* New method for setting up the blocking page and error code returned in the response to the blocked request. Now, to return different responses to requests originated from different devices and applications, you can use the variable as the value of the directives `wallarm_block_page` and `wallarm_acl_block_page`.
    
    [Detailed instructions on setting up the response via the variable →](../admin-en/configuration-guides/configure-block-page-and-code.md#variable-and-error-code)
* New filtering node statistics parameter `startid`. This parameter stores the randomly-generated unique ID of the filtering node.
    
    [The full list of available statistics parameters →](../admin-en/configure-statistics-service.md#working-with-the-statistics-service)
* Support of new Wallarm Ingress controller annotation `nginx.ingress.kubernetes.io/wallarm-acl-block-page`. This annotation is used to set up the response to the request originated from a blocked IP address.
    
    [Example of response configuration via `nginx.ingress.kubernetes.io/wallarm-acl-block-page` →](../admin-en/configure-kubernetes-en.md#configuring-the-blocking-page-and-error-code)
* Decreased memory amount allocated for the postanalytics service in deployed Wallarm node cloud image by default.
    
    In previous Wallarm node versions, the default memory amount allocated for Tarantool was 75% of the total instance memory. In the filtering node version 2.18, 40% of the total instance memory is allocated for Tarantool.

## Update process

To update the filtering node, it is recommended to check the general recommendations for the process and follow the instructions for updating the installed modules:

* [General recommendations for a safe node upgrade process](general-recommendations.md)
* [Updating modules for NGINX, NGINX Plus, Kong](nginx-modules.md)
* [Updating the Docker container with the modules for NGINX or Envoy](docker-container.md)
* [Updating NGINX Ingress controller with integrated Wallarm modules](ingress-controller.md)
* [Cloud node image](cloud-image.md)

----------

[Other updates in Wallarm products and components →](https://changelog.wallarm.com/)
