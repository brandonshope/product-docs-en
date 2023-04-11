[wallarm-status-instr]:             ../../admin-en/configure-statistics-service.md
[sqli-attack-docs]:                 ../../attacks-vulns-list.md#sql-injection
[xss-attack-docs]:                  ../../attacks-vulns-list.md#crosssite-scripting-xss
[attacks-in-ui-image]:           ../../images/admin-guides/test-attacks-quickstart-sqli-xss.png
[waf-mode-instr]:                   ../../admin-en/configure-wallarm-mode.md
[logging-instr]:                    ../../admin-en/configure-logging.md
[proxy-balancer-instr]:             ../../admin-en/using-proxy-or-balancer-en.md
[process-time-limit-instr]:         ../../admin-en/configure-parameters-en.md#wallarm_process_time_limit
[configure-selinux-instr]:          ../../admin-en/configure-selinux.md
[configure-proxy-balancer-instr]:   ../../admin-en/configuration-guides/access-to-wallarm-api-via-proxy.md
[install-postanalytics-instr]:      ../../admin-en/installation-postanalytics-en.md
[dynamic-dns-resolution-nginx]:     ../../admin-en/configure-dynamic-dns-resolution-nginx.md
[enable-libdetection-docs]:         ../../admin-en/configure-parameters-en.md#wallarm_enable_libdetection
[nginx-process-time-limit-docs]:    ../../admin-en/configure-parameters-en.md#wallarm_process_time_limit
[nginx-process-time-limit-block-docs]:  ../../admin-en/configure-parameters-en.md#wallarm_process_time_limit_block
[overlimit-res-rule-docs]:           ../../user-guides/rules/configure-overlimit-res-detection.md
[graylist-docs]:                     ../../user-guides/ip-lists/graylist.md

# Upgrading Wallarm NGINX modules 2.18 or lower

These instructions describe the steps to upgrade the Wallarm NGINX modules 2.18 or lower to version 3.6. Wallarm NGINX modules are the modules installed in accordance with one of the following instructions:

* [NGINX `stable` module](../../installation/nginx/dynamic-module.md)
* [Module for NGINX from CentOS/Debian repositories](../../installation/nginx/dynamic-module-from-distr.md)
* [NGINX Plus module](../../installation/nginx-plus.md)
* [Kong module](../../admin-en/installation-kong-en.md)

--8<-- "../include/waf/upgrade/warning-deprecated-version-upgrade-instructions.md"

## Upgrade procedure

* If filtering node and postanalytics modules are installed on the same server, then follow the instructions below to upgrade all packages.
* If filtering node and postanalytics modules are installed on different servers, **first** upgrade the postanalytics module following these [instructions](separate-postanalytics.md) and then perform the steps below for filtering node modules.

## Step 1: Inform Wallarm technical support that you are upgrading filtering node modules

Inform [Wallarm technical support](mailto:support@wallarm.com) that you are updating filtering node modules up to 3.6 and ask to enable new IP lists logic for your Wallarm account. When new IP lists logic is enabled, please open Wallarm Console and ensure that the section [**IP lists**](../../user-guides/ip-lists/overview.md) is available.

## Step 2: Disable the Active threat verification module (if upgrading node 2.16 or lower)

If upgrading Wallarm node 2.16 or lower, please disable the [Active threat verification](../../about-wallarm/detecting-vulnerabilities.md#active-threat-verification) module in Wallarm Console → **Scanner** → **Settings**.

The module operation can cause [false positives](../../about-wallarm/protecting-against-attacks.md#false-positives) during the upgrade process. Disabling the module minimizes this risk.

## Step 3: Upgrade NGINX to the latest version

Upgrade NGINX to the latest version using the relevant instructions:

=== "NGINX stable"

    DEB-based distributions:

    ```bash
    sudo apt update
    sudo apt install nginx
    ```

    RPM-based distributions:

    ```bash
    sudo yum update
    sudo yum install nginx
    ```
=== "NGINX Plus"
    For NGINX Plus, please follow the [official upgrade instructions](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-plus/#upgrading-nginx-plus).
=== "NGINX from Debian/CentOS repository"
    For NGINX [installed from Debian/CentOS repository](../../installation/nginx/dynamic-module-from-distr.md), please skip this step. The installed NGINX version will be upgraded [later](#step-6-upgrade-wallarm-packages) along with the Wallarm modules.

If your infrastructure needs to use a specific version of NGINX, please contact the [Wallarm technical support](mailto:support@wallarm.com) to build the Wallarm module for a custom version of NGINX.

## Step 4: Add new Wallarm repository

Delete the previous Wallarm repository address and add a repository with a new Wallarm node version package. Please use the commands for the appropriate platform.

**CentOS and Amazon Linux 2.0.2021x and lower**

=== "CloudLinux OS 6.x"
    ```bash
    sudo yum remove wallarm-node-repo
    sudo yum clean all
    sudo rpm -i https://repo.wallarm.com/centos/wallarm-node/6/3.6/x86_64/Packages/wallarm-node-repo-1-6.el6.noarch.rpm
    ```
=== "CentOS 7 and Amazon Linux 2.0.2021x and lower"
    ```bash
    sudo yum remove wallarm-node-repo
    sudo yum clean all
    sudo rpm -i https://repo.wallarm.com/centos/wallarm-node/7/3.6/x86_64/Packages/wallarm-node-repo-1-6.el7.noarch.rpm
    ```
=== "CentOS 8"
    !!! warning "Support for CentOS 8.x has been deprecated"
        Support for CentOS 8.x [has been deprecated](https://www.centos.org/centos-linux-eol/). You can install the Wallarm node 3.6 on the AlmaLinux, Rocky Linux or Oracle Linux 8.x operating system insted.

        * [Installation instructions for NGINX `stable`](../../installation/nginx/dynamic-module.md)
        * [Installation instructions for NGINX from CentOS/Debian repositories](../../installation/nginx/dynamic-module-from-distr.md)
        * [Installation instructions for NGINX Plus](../../installation/nginx-plus.md)

**Debian and Ubuntu**

1. Open the file with the Wallarm repository address in the installed text editor. In these instructions, **vim** is used.

    ```bash
    sudo vim /etc/apt/sources.list.d/wallarm.list
    ```
2. Comment out or delete the previous repository address.
3. Add a new repository address:

    === "Debian 9.x (stretch)"
        ``` bash
        deb https://repo.wallarm.com/debian/wallarm-node stretch/3.6/
        ```
    === "Debian 9.x (stretch-backports)"
        ```bash
        deb https://repo.wallarm.com/debian/wallarm-node stretch/3.6/
        deb https://repo.wallarm.com/debian/wallarm-node stretch-backports/3.6/
        ```
    === "Debian 10.x (buster)"
        ```bash
        deb https://repo.wallarm.com/debian/wallarm-node buster/3.6/
        ```
    === "Ubuntu 18.04 LTS (bionic)"
        ```bash
        deb https://repo.wallarm.com/ubuntu/wallarm-node bionic/3.6/
        ```
    === "Ubuntu 20.04 LTS (focal)"
        ```bash
        deb https://repo.wallarm.com/ubuntu/wallarm-node focal/3.6/
        ```

## Step 5: Migrate allowlists and denylists from the previous Wallarm node version to 3.6

[Migrate](../migrate-ip-lists-to-node-3.md) allowlist and denylist configuration from previous Wallarm node version to 3.6.

## Step 6: Upgrade Wallarm packages

### Filtering node and postanalytics on the same server

1. Execute the following command to upgrade the filtering node and postanalytics modules:

    === "Debian"
        ```bash
        sudo apt update
        sudo apt dist-upgrade
        ```

        --8<-- "../include/waf/upgrade/warning-expired-gpg-keys.md"
    === "Ubuntu"
        ```bash
        sudo apt update
        sudo apt dist-upgrade
        ```

        --8<-- "../include/waf/upgrade/warning-expired-gpg-keys.md"
    === "CentOS or Amazon Linux 2.0.2021x and lower"
        ```bash
        sudo yum update
        ```
2. If the package manager asks for confirmation to rewrite the content of the configuration file `/etc/cron.d/wallarm-node-nginx`:

    1. Ensure that the [IP lists migration](#step-4-migrate-allowlists-and-denylists-from-previous-wallarm-node-version-to-32) is completed.
    2. Confirm the file rewrite by using the option `Y`.

        The package manager would ask for the rewrite confirmation if the file `/etc/cron.d/wallarm-node-nginx` had been [changed in the previous Wallarm node versions](/2.18/admin-en/configure-ip-blocking-nginx-en/). Since IP list logic was changed in Wallarm node 3.x, the `/etc/cron.d/wallarm-node-nginx` content was updated accordingly. For the IP address denylist to operate correctly, the Wallarm node 3.x should use the updated configuration file.

        By default, the package manager uses the option `N` but the option `Y` is required for the correct IP address denylist operation in Wallarm node 3.x.

### Filtering node and postanalytics on different servers

!!! warning "Sequence of steps to upgrade the filtering node and postanalytics modules"
    If the filtering node and postanalytics modules are installed on different servers, then it is required to upgrade the postanalytics packages before updating the filtering node packages.

1. Upgrade postanalytics packages following these [instructions](separate-postanalytics.md).
2. Upgrade Wallarm node packages:

    === "Debian"
        ```bash
        sudo apt update
        sudo apt dist-upgrade
        ```

        --8<-- "../include/waf/upgrade/warning-expired-gpg-keys.md"
    === "Ubuntu"
        ```bash
        sudo apt update
        sudo apt dist-upgrade
        ```

        --8<-- "../include/waf/upgrade/warning-expired-gpg-keys.md"
    === "CentOS or Amazon Linux 2.0.2021x and lower"
        ```bash
        sudo yum update
        ```
3. If the package manager asks for confirmation to rewrite the content of the configuration file `/etc/cron.d/wallarm-node-nginx`:

    1. Ensure that the [IP lists migration](#step-4-migrate-allowlists-and-denylists-from-previous-wallarm-node-version-to-32) is completed.
    2. Confirm the file rewrite by using the option `Y`.

        The package manager would ask for the rewrite confirmation if the file `/etc/cron.d/wallarm-node-nginx` had been [changed in the previous Wallarm node versions](/2.18/admin-en/configure-ip-blocking-nginx-en/). Since IP list logic was changed in Wallarm node 3.x, the `/etc/cron.d/wallarm-node-nginx` content was updated accordingly. For the IP address denylist to operate correctly, the Wallarm node 3.x should use the updated configuration file.

        By default, the package manager uses the option `N` but the option `Y` is required for the correct IP address denylist operation in Wallarm node 3.x.

## Step 7: Update the Wallarm blocking page

In new node version, the Wallarm sample blocking page has [been changed](what-is-new.md#new-blocking-page). The logo and support email on the page are now empty by default.

If the page `&/usr/share/nginx/html/wallarm_blocked.html` was configured to be returned in response to blocked requests, [copy and customize](../../admin-en/configuration-guides/configure-block-page-and-code.md#customizing-sample-blocking-page) the new version of a sample page.

## Step 8: Rename deprecated NGINX directives

Rename the following NGINX directives if they are explicitly specified in configuration files:

* `wallarm_instance` → [`wallarm_application`](../../admin-en/configure-parameters-en.md#wallarm_application)
* `wallarm_local_trainingset_path` → [`wallarm_custom_ruleset_path`](../../admin-en/configure-parameters-en.md#wallarm_custom_ruleset_path)
* `wallarm_global_trainingset_path` → [`wallarm_protondb_path`](../../admin-en/configure-parameters-en.md#wallarm_protondb_path)

We only changed the names of the directives, their logic remains the same. Directives with former names will be deprecated soon, so you are recommended to rename them before.

## Step 9: Adjust Wallarm node filtration mode settings to changes released in the latest versions

1. Ensure that the expected behavior of settings listed below corresponds to the [changed logic of the `off` and `monitoring` filtration modes](what-is-new.md#filtration-modes):
      * [Directive `wallarm_mode`](../../admin-en/configure-parameters-en.md#wallarm_mode)
      * [General filtration rule configured in Wallarm Console](../../user-guides/settings/general.md)
      * [Low-level filtration rules configured in Wallarm Console](../../user-guides/rules/wallarm-mode-rule.md)
2. If the expected behavior does not correspond to the changed filtration mode logic, please adjust the filtration mode settings to released changes using the [instructions](../../admin-en/configure-wallarm-mode.md).

## Step 10: Transfer the `overlimit_res` attack detection configuration from directives to the rule

--8<-- "../include/waf/upgrade/migrate-to-overlimit-rule-nginx.md"

## Step 11: Update the `wallarm-status.conf` file contents

Update the `/etc/nginx/conf.d/wallarm-status.conf` contents as follows:

```
server {
  listen 127.0.0.8:80;
  server_name localhost;

  allow 127.0.0.0/8;   # Access is only available for loopback addresses of the filter node server  
  deny all;

  wallarm_mode off;
  disable_acl "on";   # Checking request sources is disabled, denylisted IPs are allowed to request the wallarm-status service. https://docs.wallarm.com/admin-en/configure-parameters-en/#disable_acl
  access_log off;

  location ~/wallarm-status$ {
    wallarm_status on;
  }
}
```

[More details on the statistics service configuration](../../admin-en/configure-statistics-service.md)

## Step 12: Restart NGINX

--8<-- "../include/waf/restart-nginx-2.16.md"

## Step 13: Test Wallarm node operation

--8<-- "../include/waf/installation/test-waf-operation-no-stats-for-deprecated.md"

## Step 14: Re-enable the Active threat verification module (if upgrading node 2.16 or lower)

Learn the [recommendation on the Active threat verification module setup](../../admin-en/attack-rechecker-best-practices.md) and re-enable it if required.

After a while, ensure the module operation does not cause false positives. If discovering false positives, please contact the [Wallarm technical support](mailto:support@wallarm.com).

## Settings customization

The Wallarm modules are updated to version 3.6. Previous filtering node settings will be applied to the new version automatically. To make additional settings, use the [available directives](../../admin-en/configure-parameters-en.md).

--8<-- "../include/waf/installation/common-customization-options-nginx.md"
