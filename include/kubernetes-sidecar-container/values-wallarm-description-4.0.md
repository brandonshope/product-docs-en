```
wallarm:
  image:
     repository: wallarm/node
     tag: 4.0.2-1
     pullPolicy: Always
  # Wallarm API endpoint: 
  # "api.wallarm.com" for the EU Cloud
  # "us1.api.wallarm.com" for the US Cloud
  wallarm_host_api: "api.wallarm.com"
  # Wallarm node token
  wallarm_api_token: "token"
  # Port on which the container accepts incoming requests,
  # the value must be identical to ports.containerPort
  # in definition of your main app container
  app_container_port: 80
  # Request filtration mode:
  # "off" to disable request processing
  # "monitoring" to process but not block requests
  # "safe_blocking" to block malicious requests originated from graylisted IPs
  # "block" to process all requests and block the malicious ones
  mode: "block"
  # Amount of memory in GB for request analytics data
  tarantool_memory_gb: 2
```