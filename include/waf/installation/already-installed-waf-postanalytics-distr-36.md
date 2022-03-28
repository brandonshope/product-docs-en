!!! info "If you deploy several Wallarm nodes"
    All Wallarm nodes deployed to your environment must be of the **same versions**. The postanalytics modules installed on separated servers must be of the **same versions** too.

    Before installation of the additional node, please ensure its version matches the version of already deployed modules. If the deployed module version is [deprecated or will be deprecated soon (`3.2` or lower)][versioning-policy], upgrade all modules to the latest version.

    To check the version of the filtering node and the postanalytics module deployed on the same server:

    === "Debian"
        ```bash
        apt list wallarm-node
        ```
    === "CentOS"
        ```bash
        yum list wallarm-node
        ```
    === "AlmaLinux, Rocky Linux or Oracle Linux 8.x"
        ```bash
        yum list wallarm-node
        ```

    To check the version of the filtering node and the postanalytics module deployed on different servers:

    === "Debian"
        ```bash
        # run from the server with installed Wallarm filtering node
        apt list wallarm-node-nginx
        # run from the server with installed postanalytics
        apt list wallarm-node-tarantool
        ```
    === "CentOS"
        ```bash
        # run from the server with installed Wallarm filtering node
        yum list wallarm-node-nginx
        # run from the server with installed postanalytics
        yum list wallarm-node-tarantool
        ```
    === "AlmaLinux, Rocky Linux or Oracle Linux 8.x"
        ```bash
        # run from the server with installed Wallarm filtering node
        yum list wallarm-node-nginx
        # run from the server with installed postanalytics
        yum list wallarm-node-tarantool
        ```
