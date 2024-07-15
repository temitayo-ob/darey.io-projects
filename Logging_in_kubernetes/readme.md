# LOGGING IN KUBERNETES

Logging in Kubernetes plays a critical role in capturing and analyzing application and infrastructure logs, allowing for effective troubleshooting, debugging, and gaining insights into the behavior of your containerized workloads within the Kubernetes cluster

### Deploying and configuring ELK stack

Intoduction

Are you curious about what's happening within your Kubernetes cluster? This hands-on project will equip you with the knowledge and tools to unlock the secrets hidden in your logs. Imagine yourself as a detective, wielding powerful commands to uncover the stories your pods are telling.

Prerequisites:
Kubectl: Your key to interacting with Kubernetes. Install kubectl

Kubeconfig: This acts as your access code to the cluster. Retrieve it from your cluster administrator.

Setting the stage:

Choosing the right Kubernetes cluster to explore is like picking the right path on a map. Let's tell kubectl which cluster you want to investigate.

Optional Setup (If using AWS EKS):

This guide walks you through setting up and interacting with your AWS EKS cluster using Terraform and kubectl.

AWS Account: With neccessary permissions.

Terraform: For infrastructure provisioning

AWS CLI: AWL CLI is use to interact with AWS services.

### Logging concepts and best practices

The Power of Logs: Why They Matter
Logs are essential for:

- Troubleshooting: Uncover and diagnose issues, bugs, and errors efficiently.
- Security Monitoring: Stay vigilant and detect suspicious activity or potential threats.
- Performance Optimization: Gain insights into your system's performance and optimize resource usage.
- Auditing and Compliance: Meet compliance requirements and track user activity effectively.

### Embracing Best Practices

- Standardize Your Logging Format: Utilize structured formats like JSON or XML for streamlined parsing and analysis.
- Choose the Right Storage: Pick a reliable and scalable storage solution based on your data volume and needs.
- Centralize Your Logs: Aggregate logs from diverse sources for comprehensive search and analysis.
- Enable Log Rotation: Implement automatic rotation to avoid disk space exhaustion.
- Secure Your Logs: Protect sensitive information through access control and encryption.
- Monitor Your Logs: Proactively set up alerts for critical events and errors.
- Analyze Your Logs: Extract valuable insights and trends from your log data.

### Centralized Logging with the ELK Stack

This section explores a common centralized logging approach utilizing the ELK Stack:

![A](./images/A.png)

Key Components:

- Fluentd: A powerful log collector, gathering logs from individual Kubernetes pods.
- Fluentd Configuration (fluentd.conf): Defines how Fluentd operates, including data sources and destinations.
- Elasticsearch: A scalable database storing and indexing your log data for efficient retrieval.
- Kibana: A user-friendly dashboard visualizing and analyzing logs from Elasticsearch.


Implementation Steps:

1. Deploy Fluentd as a DaemonSet: Ensure every worker node runs Fluentd for comprehensive log collection.
2. Configure Fluentd: Customize fluentd.conf to specify log sources and send them to Elasticsearch.
3. Deploy Elasticsearch: Set up Elasticsearch to store and index your log data efficiently.
4. Deploy Kibana: Configure Kibana to connect to Elasticsearch and provide insightful visualizations.


Connecting the Dots:

- Fluentd pods access Elasticsearch using service accounts for secure communication.
- Elasticsearch, Kibana, and Fluentd pods run within Kubernetes and communicate via ClusterIP addresses.


Benefits:

- Centralized log management for easy search and analysis.
- Streamlined troubleshooting and performance monitoring.
- Enhanced security and compliance through centralized control.


### Automating Kibana dashboards

### Deploying and Configuring the ELK Stack in Kubernetes

1. Create a directory: `elastic stack`

2. Configure Fluentd:

- Create a fluentd manifest file named `fluentd.yaml` paste the code snippet in it

```
kind: ConfigMap
apiVersion: v1
metadata:
  name: fluentd-es-config-v0.1.4
  namespace: kube-system
  labels:
    addonmanager.kubernetes.io/mode: Reconcile
data:
  system.conf: |-
    <system>
      root_dir /tmp/fluentd-buffers/
    </system>

  containers.input.conf: |-
    # This configuration file for Fluentd / td-agent is used
    # to watch changes to Docker log files. The kubelet creates symlinks that
    # capture the pod name, namespace, container name & Docker container ID
    # to the docker logs for pods in the /var/log/containers directory on the host.
    # If running this fluentd configuration in a Docker container, the /var/log
    # directory should be mounted in the container.
    #
    # These logs are then submitted to Elasticsearch which assumes the
    # installation of the fluent-plugin-elasticsearch & the
    # fluent-plugin-kubernetes_metadata_filter plugins.
    # See https://github.com/uken/fluent-plugin-elasticsearch &
    # https://github.com/fabric8io/fluent-plugin-kubernetes_metadata_filter for
    # more information about the plugins.
    #
    # Example
    # =======
    # A line in the Docker log file might look like this JSON:
    #
    # {"log":"2014/09/25 21:15:03 Got request with path wombat\n",
    #  "stream":"stderr",
    #   "time":"2014-09-25T21:15:03.499185026Z"}
    #
    # The time_format specification below makes sure we properly
    # parse the time format produced by Docker. This will be
    # submitted to Elasticsearch and should appear like:
    # $ curl 'http://elasticsearch-logging:9200/_search?pretty'
    # ...
    # {
    #      "_index" : "logstash-2014.09.25",
    #      "_type" : "fluentd",
    #      "_id" : "VBrbor2QTuGpsQyTCdfzqA",
    #      "_score" : 1.0,
    #      "_source":{"log":"2014/09/25 22:45:50 Got request with path wombat\n",
    #                 "stream":"stderr","tag":"docker.container.all",
    #                 "@timestamp":"2014-09-25T22:45:50+00:00"}
    #    },
    # ...
    #
    # The Kubernetes fluentd plugin is used to write the Kubernetes metadata to the log
    # record & add labels to the log record if properly configured. This enables users
    # to filter & search logs on any metadata.
    # For example a Docker container's logs might be in the directory:
    #
    #  /var/lib/docker/containers/997599971ee6366d4a5920d25b79286ad45ff37a74494f262e3bc98d909d0a7b
    #
    # and in the file:
    #
    #  997599971ee6366d4a5920d25b79286ad45ff37a74494f262e3bc98d909d0a7b-json.log
    #
    # where 997599971ee6... is the Docker ID of the running container.
    # The Kubernetes kubelet makes a symbolic link to this file on the host machine
    # in the /var/log/containers directory which includes the pod name and the Kubernetes
    # container name:
    #
    #    synthetic-logger-0.25lps-pod_default_synth-lgr-997599971ee6366d4a5920d25b79286ad45ff37a74494f262e3bc98d909d0a7b.log
    #    ->
    #    /var/lib/docker/containers/997599971ee6366d4a5920d25b79286ad45ff37a74494f262e3bc98d909d0a7b/997599971ee6366d4a5920d25b79286ad45ff37a74494f262e3bc98d909d0a7b-json.log
    #
    # The /var/log directory on the host is mapped to the /var/log directory in the container
    # running this instance of Fluentd and we end up collecting the file:
    #
    #   /var/log/containers/synthetic-logger-0.25lps-pod_default_synth-lgr-997599971ee6366d4a5920d25b79286ad45ff37a74494f262e3bc98d909d0a7b.log
    #
    # This results in the tag:
    #
    #  var.log.containers.synthetic-logger-0.25lps-pod_default_synth-lgr-997599971ee6366d4a5920d25b79286ad45ff37a74494f262e3bc98d909d0a7b.log
    #
    # The Kubernetes fluentd plugin is used to extract the namespace, pod name & container name
    # which are added to the log message as a kubernetes field object & the Docker container ID
    # is also added under the docker field object.
    # The final tag is:
    #
    #   kubernetes.var.log.containers.synthetic-logger-0.25lps-pod_default_synth-lgr-997599971ee6366d4a5920d25b79286ad45ff37a74494f262e3bc98d909d0a7b.log
    #
    # And the final log record look like:
    #
    # {
    #   "log":"2014/09/25 21:15:03 Got request with path wombat\n",
    #   "stream":"stderr",
    #   "time":"2014-09-25T21:15:03.499185026Z",
    #   "kubernetes": {
    #     "namespace": "default",
    #     "pod_name": "synthetic-logger-0.25lps-pod",
    #     "container_name": "synth-lgr"
    #   },
    #   "docker": {
    #     "container_id": "997599971ee6366d4a5920d25b79286ad45ff37a74494f262e3bc98d909d0a7b"
    #   }
    # }
    #
    # This makes it easier for users to search for logs by pod name or by
    # the name of the Kubernetes container regardless of how many times the
    # Kubernetes pod has been restarted (resulting in a several Docker container IDs).

    # Json Log Example:
    # {"log":"[info:2016-02-16T16:04:05.930-08:00] Some log text here\n","stream":"stdout","time":"2016-02-17T00:04:05.931087621Z"}
    # CRI Log Example:
    # 2016-02-17T00:04:05.931087621Z stdout F [info:2016-02-16T16:04:05.930-08:00] Some log text here
    <source>
      @id fluentd-containers.log
      @type tail
      path /var/log/containers/*.log
      pos_file /var/log/es-containers.log.pos
      time_format %Y-%m-%dT%H:%M:%S.%NZ
      tag raw.kubernetes.*
      read_from_head true
      <parse>
        @type multi_format
        <pattern>
          format json
          time_key time
          time_format %Y-%m-%dT%H:%M:%S.%NZ
        </pattern>
        <pattern>
          format /^(?<time>.+) (?<stream>stdout|stderr) [^ ]* (?<log>.*)$/
          time_format %Y-%m-%dT%H:%M:%S.%N%:z
        </pattern>
      </parse>
    </source>

    # Detect exceptions in the log output and forward them as one log entry.
    <match raw.kubernetes.**>
      @id raw.kubernetes
      @type detect_exceptions
      remove_tag_prefix raw
      message log
      stream stream
      multiline_flush_interval 5
      max_bytes 500000
      max_lines 1000
    </match>

  system.input.conf: |-
    # Example:
    # 2015-12-21 23:17:22,066 [salt.state       ][INFO    ] Completed state [net.ipv4.ip_forward] at time 23:17:22.066081
    <source>
      @id minion
      @type tail
      format /^(?<time>[^ ]* [^ ,]*)[^\[]*\[[^\]]*\]\[(?<severity>[^ \]]*) *\] (?<message>.*)$/
      time_format %Y-%m-%d %H:%M:%S
      path /var/log/salt/minion
      pos_file /var/log/salt.pos
      tag salt
    </source>

    # Example:
    # Dec 21 23:17:22 gke-foo-1-1-4b5cbd14-node-4eoj startupscript: Finished running startup script /var/run/google.startup.script
    <source>
      @id startupscript.log
      @type tail
      format syslog
      path /var/log/startupscript.log
      pos_file /var/log/es-startupscript.log.pos
      tag startupscript
    </source>

    # Examples:
    # time="2016-02-04T06:51:03.053580605Z" level=info msg="GET /containers/json"
    # time="2016-02-04T07:53:57.505612354Z" level=error msg="HTTP Error" err="No such image: -f" statusCode=404
    # TODO(random-liu): Remove this after cri container runtime rolls out.
    <source>
      @id docker.log
      @type tail
      format /^time="(?<time>[^)]*)" level=(?<severity>[^ ]*) msg="(?<message>[^"]*)"( err="(?<error>[^"]*)")?( statusCode=($<status_code>\d+))?/
      path /var/log/docker.log
      pos_file /var/log/es-docker.log.pos
      tag docker
    </source>

    # Example:
    # 2016/02/04 06:52:38 filePurge: successfully removed file /var/etcd/data/member/wal/00000000000006d0-00000000010a23d1.wal
    <source>
      @id etcd.log
      @type tail
      # Not parsing this, because it doesn't have anything particularly useful to
      # parse out of it (like severities).
      format none
      path /var/log/etcd.log
      pos_file /var/log/es-etcd.log.pos
      tag etcd
    </source>

    # Multi-line parsing is required for all the kube logs because very large log
    # statements, such as those that include entire object bodies, get split into
    # multiple lines by glog.

    # Example:
    # I0204 07:32:30.020537    3368 server.go:1048] POST /stats/container/: (13.972191ms) 200 [[Go-http-client/1.1] 10.244.1.3:40537]
    <source>
      @id kubelet.log
      @type tail
      format multiline
      multiline_flush_interval 5s
      format_firstline /^\w\d{4}/
      format1 /^(?<severity>\w)(?<time>\d{4} [^\s]*)\s+(?<pid>\d+)\s+(?<source>[^ \]]+)\] (?<message>.*)/
      time_format %m%d %H:%M:%S.%N
      path /var/log/kubelet.log
      pos_file /var/log/es-kubelet.log.pos
      tag kubelet
    </source>

    # Example:
    # I1118 21:26:53.975789       6 proxier.go:1096] Port "nodePort for kube-system/default-http-backend:http" (:31429/tcp) was open before and is still needed
    <source>
      @id kube-proxy.log
      @type tail
      format multiline
      multiline_flush_interval 5s
      format_firstline /^\w\d{4}/
      format1 /^(?<severity>\w)(?<time>\d{4} [^\s]*)\s+(?<pid>\d+)\s+(?<source>[^ \]]+)\] (?<message>.*)/
      time_format %m%d %H:%M:%S.%N
      path /var/log/kube-proxy.log
      pos_file /var/log/es-kube-proxy.log.pos
      tag kube-proxy
    </source>

    # Example:
    # I0204 07:00:19.604280       5 handlers.go:131] GET /api/v1/nodes: (1.624207ms) 200 [[kube-controller-manager/v1.1.3 (linux/amd64) kubernetes/6a81b50] 127.0.0.1:38266]
    <source>
      @id kube-apiserver.log
      @type tail
      format multiline
      multiline_flush_interval 5s
      format_firstline /^\w\d{4}/
      format1 /^(?<severity>\w)(?<time>\d{4} [^\s]*)\s+(?<pid>\d+)\s+(?<source>[^ \]]+)\] (?<message>.*)/
      time_format %m%d %H:%M:%S.%N
      path /var/log/kube-apiserver.log
      pos_file /var/log/es-kube-apiserver.log.pos
      tag kube-apiserver
    </source>

    # Example:
    # I0204 06:55:31.872680       5 servicecontroller.go:277] LB already exists and doesn't need update for service kube-system/kube-ui
    <source>
      @id kube-controller-manager.log
      @type tail
      format multiline
      multiline_flush_interval 5s
      format_firstline /^\w\d{4}/
      format1 /^(?<severity>\w)(?<time>\d{4} [^\s]*)\s+(?<pid>\d+)\s+(?<source>[^ \]]+)\] (?<message>.*)/
      time_format %m%d %H:%M:%S.%N
      path /var/log/kube-controller-manager.log
      pos_file /var/log/es-kube-controller-manager.log.pos
      tag kube-controller-manager
    </source>

    # Example:
    # W0204 06:49:18.239674       7 reflector.go:245] pkg/scheduler/factory/factory.go:193: watch of *api.Service ended with: 401: The event in requested index is outdated and cleared (the requested history has been cleared [2578313/2577886]) [2579312]
    <source>
      @id kube-scheduler.log
      @type tail
      format multiline
      multiline_flush_interval 5s
      format_firstline /^\w\d{4}/
      format1 /^(?<severity>\w)(?<time>\d{4} [^\s]*)\s+(?<pid>\d+)\s+(?<source>[^ \]]+)\] (?<message>.*)/
      time_format %m%d %H:%M:%S.%N
      path /var/log/kube-scheduler.log
      pos_file /var/log/es-kube-scheduler.log.pos
      tag kube-scheduler
    </source>

    # Example:
    # I1104 10:36:20.242766       5 rescheduler.go:73] Running Rescheduler
    <source>
      @id rescheduler.log
      @type tail
      format multiline
      multiline_flush_interval 5s
      format_firstline /^\w\d{4}/
      format1 /^(?<severity>\w)(?<time>\d{4} [^\s]*)\s+(?<pid>\d+)\s+(?<source>[^ \]]+)\] (?<message>.*)/
      time_format %m%d %H:%M:%S.%N
      path /var/log/rescheduler.log
      pos_file /var/log/es-rescheduler.log.pos
      tag rescheduler
    </source>

    # Example:
    # I0603 15:31:05.793605       6 cluster_manager.go:230] Reading config from path /etc/gce.conf
    <source>
      @id glbc.log
      @type tail
      format multiline
      multiline_flush_interval 5s
      format_firstline /^\w\d{4}/
      format1 /^(?<severity>\w)(?<time>\d{4} [^\s]*)\s+(?<pid>\d+)\s+(?<source>[^ \]]+)\] (?<message>.*)/
      time_format %m%d %H:%M:%S.%N
      path /var/log/glbc.log
      pos_file /var/log/es-glbc.log.pos
      tag glbc
    </source>

    # Example:
    # I0603 15:31:05.793605       6 cluster_manager.go:230] Reading config from path /etc/gce.conf
    <source>
      @id cluster-autoscaler.log
      @type tail
      format multiline
      multiline_flush_interval 5s
      format_firstline /^\w\d{4}/
      format1 /^(?<severity>\w)(?<time>\d{4} [^\s]*)\s+(?<pid>\d+)\s+(?<source>[^ \]]+)\] (?<message>.*)/
      time_format %m%d %H:%M:%S.%N
      path /var/log/cluster-autoscaler.log
      pos_file /var/log/es-cluster-autoscaler.log.pos
      tag cluster-autoscaler
    </source>

    # Logs from systemd-journal for interesting services.
    # TODO(random-liu): Remove this after cri container runtime rolls out.
    <source>
      @id journald-docker
      @type systemd
      filters [{ "_SYSTEMD_UNIT": "docker.service" }]
      <storage>
        @type local
        persistent true
      </storage>
      read_from_head true
      tag docker
    </source>

    <source>
      @id journald-container-runtime
      @type systemd
      filters [{ "_SYSTEMD_UNIT": "{{ container_runtime }}.service" }]
      <storage>
        @type local
        persistent true
      </storage>
      read_from_head true
      tag container-runtime
    </source>

    <source>
      @id journald-kubelet
      @type systemd
      filters [{ "_SYSTEMD_UNIT": "kubelet.service" }]
      <storage>
        @type local
        persistent true
      </storage>
      read_from_head true
      tag kubelet
    </source>

    <source>
      @id journald-node-problem-detector
      @type systemd
      filters [{ "_SYSTEMD_UNIT": "node-problem-detector.service" }]
      <storage>
        @type local
        persistent true
      </storage>
      read_from_head true
      tag node-problem-detector
    </source>

    <source>
      @id kernel
      @type systemd
      filters [{ "_TRANSPORT": "kernel" }]
      <storage>
        @type local
        persistent true
      </storage>
      <entry>
        fields_strip_underscores true
        fields_lowercase true
      </entry>
      read_from_head true
      tag kernel
    </source>

  forward.input.conf: |-
    # Takes the messages sent over TCP
    <source>
      @type forward
    </source>

  monitoring.conf: |-
    # Prometheus Exporter Plugin
    # input plugin that exports metrics
    <source>
      @type prometheus
    </source>

    <source>
      @type monitor_agent
    </source>

    # input plugin that collects metrics from MonitorAgent
    <source>
      @type prometheus_monitor
      <labels>
        host ${hostname}
      </labels>
    </source>

    # input plugin that collects metrics for output plugin
    <source>
      @type prometheus_output_monitor
      <labels>
        host ${hostname}
      </labels>
    </source>

    # input plugin that collects metrics for in_tail plugin
    <source>
      @type prometheus_tail_monitor
      <labels>
        host ${hostname}
      </labels>
    </source>

  output.conf: |-
    # Enriches records with Kubernetes metadata
    <filter kubernetes.**>
      @type kubernetes_metadata
    </filter>

    <match **>
      @id elasticsearch
      @type elasticsearch
      @log_level info
      include_tag_key true
      host elasticsearch-logging
      port 9200
      logstash_format true
      <buffer>
        @type file
        path /var/log/fluentd-buffers/kubernetes.system.buffer
        flush_mode interval
        retry_type exponential_backoff
        flush_thread_count 2
        flush_interval 5s
        retry_forever
        retry_max_interval 30
        chunk_limit_size 2M
        queue_limit_length 8
        overflow_action block
      </buffer>
    </match>
```

This fluentd yaml file is a configuration file that defines how Fluentd, a log aggregator and processor, collects and sends logs to Elasticsearch in a Kubernetes environment. Here's a breakdown of its key components:

Data sources (sources):

- system.conf: This section defines how Fluentd reads logs from Docker containers running on the host system. It uses the tail plugin to follow log files and parses them using specific formats based on the source (e.g., Docker, Kubernetes logs).
- journald-*: These sections collect logs from systemd journal for specific services like Docker, kubelet, and kernel.
- forward.input.conf: This defines a source for receiving log messages sent over TCP, but it's not configured with any details in this snippet.
- elasticsearch: This section defines how Fluentd sends logs to Elasticsearch on the elasticsearch-logging host at port 9200. It buffers logs before sending them in batches for efficiency and uses exponential backoff for retries in case of temporary issues.
- monitoring.conf: This section enables Prometheus metrics collection for various Fluentd components.
- system.input.conf: This section defines additional log sources like minion logs and startupscript logs.

Overall, this configuration file ensures that logs from various sources within a Kubernetes cluster are collected, parsed, enriched with metadata, and sent to Elasticsearch for centralized storage and analysis.

- Apply the configuration:

`kubectl apply -f fluentd.yaml`

- Verify deployment:

`kubectl get configmap -n kube-system`

![cm](./images/config-map.png)


### Deploying the ELK Stack

1. Create a Manifest file:

### Deploying the ELK Stack

1. Create a Manifest file:

- Create a file named `elastic-stack.yaml` and paste code snippet below

```
#
# Fluentd configuration - this installs onto all nodes and gathers all logs
# A separate yaml configures Fluentd
#
apiVersion: v1
kind: ServiceAccount
metadata:
  name: fluentd-es
  namespace: kube-system
  labels:
    k8s-app: fluentd-es
    kubernetes.io/cluster-service: "true"
    addonmanager.kubernetes.io/mode: Reconcile
---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: fluentd-es
  labels:
    k8s-app: fluentd-es
    kubernetes.io/cluster-service: "true"
    addonmanager.kubernetes.io/mode: Reconcile
rules:
- apiGroups:
  - ""
  resources:
  - "namespaces"
  - "pods"
  verbs:
  - "get"
  - "watch"
  - "list"
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: fluentd-es
  labels:
    k8s-app: fluentd-es
    kubernetes.io/cluster-service: "true"
    addonmanager.kubernetes.io/mode: Reconcile
subjects:
- kind: ServiceAccount
  name: fluentd-es
  namespace: kube-system
  apiGroup: ""
roleRef:
  kind: ClusterRole
  name: fluentd-es
  apiGroup: ""
---
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd-es-v2.2.0
  namespace: kube-system
  labels:
    k8s-app: fluentd-es
    version: v2.2.0
    kubernetes.io/cluster-service: "true"
    addonmanager.kubernetes.io/mode: Reconcile
spec:
  selector:
    matchLabels:
      k8s-app: fluentd-es
      version: v2.2.0
  template:
    metadata:
      labels:
        k8s-app: fluentd-es
        kubernetes.io/cluster-service: "true"
        version: v2.2.0
      # This annotation ensures that fluentd does not get evicted if the node
      # supports critical pod annotation based priority scheme.
      # Note that this does not guarantee admission on the nodes (#40573).
      annotations:
        scheduler.alpha.kubernetes.io/critical-pod: ''
        seccomp.security.alpha.kubernetes.io/pod: 'docker/default'
    spec:
      priorityClassName: system-node-critical
      serviceAccountName: fluentd-es
      containers:
      - name: fluentd-es
        image: k8s.gcr.io/fluentd-elasticsearch:v2.2.0
        env:
        - name: FLUENTD_ARGS
          value: --no-supervisor -q
        resources:
          limits:
            memory: 500Mi
          requests:
            cpu: 100m
            memory: 200Mi
        volumeMounts:
        - name: varlog
          mountPath: /var/log
        - name: varlibdockercontainers
          mountPath: /var/lib/docker/containers
          readOnly: true
        - name: config-volume
          mountPath: /etc/fluent/config.d
      # nodeSelector:
      #  beta.kubernetes.io/fluentd-ds-ready: "true"
      terminationGracePeriodSeconds: 30
      volumes:
      - name: varlog
        hostPath:
          path: /var/log
      - name: varlibdockercontainers
        hostPath:
          path: /var/lib/docker/containers
      - name: config-volume
        configMap:
          name: fluentd-es-config-v0.1.4


---

#
# Elastic Search configuration
#
apiVersion: v1
kind: Service
metadata:
  name: elasticsearch-logging
  namespace: kube-system
  labels:
    k8s-app: elasticsearch-logging
    kubernetes.io/cluster-service: "true"
    addonmanager.kubernetes.io/mode: Reconcile
    kubernetes.io/name: "Elasticsearch"
spec:
  ports:
  - port: 9200
    protocol: TCP
    targetPort: db
  selector:
    k8s-app: elasticsearch-logging
---
# RBAC authn and authz
apiVersion: v1
kind: ServiceAccount
metadata:
  name: elasticsearch-logging
  namespace: kube-system
  labels:
    k8s-app: elasticsearch-logging
    kubernetes.io/cluster-service: "true"
    addonmanager.kubernetes.io/mode: Reconcile
---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: elasticsearch-logging
  labels:
    k8s-app: elasticsearch-logging
    kubernetes.io/cluster-service: "true"
    addonmanager.kubernetes.io/mode: Reconcile
rules:
- apiGroups:
  - ""
  resources:
  - "services"
  - "namespaces"
  - "endpoints"
  verbs:
  - "get"
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: kube-system
  name: elasticsearch-logging
  labels:
    k8s-app: elasticsearch-logging
    kubernetes.io/cluster-service: "true"
    addonmanager.kubernetes.io/mode: Reconcile
subjects:
- kind: ServiceAccount
  name: elasticsearch-logging
  namespace: kube-system
  apiGroup: ""
roleRef:
  kind: ClusterRole
  name: elasticsearch-logging
  apiGroup: ""
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: elasticsearch-logging
  namespace: kube-system
  labels:
    k8s-app: elasticsearch-logging
    version: v6.2.5
    kubernetes.io/cluster-service: "true"
    addonmanager.kubernetes.io/mode: Reconcile
spec:
  serviceName: elasticsearch-logging
  replicas: 2
  selector:
    matchLabels:
      k8s-app: elasticsearch-logging
      version: v6.2.5
  template:
    metadata:
      labels:
        k8s-app: elasticsearch-logging
        version: v6.2.5
        kubernetes.io/cluster-service: "true"
    spec:
      serviceAccountName: elasticsearch-logging
      containers:
      - image: k8s.gcr.io/elasticsearch:v6.2.5
        name: elasticsearch-logging
        resources:
          # need more cpu upon initialization, therefore burstable class
          limits:
            cpu: 1000m
          requests:
            cpu: 100m
        ports:
        - containerPort: 9200
          name: db
          protocol: TCP
        - containerPort: 9300
          name: transport
          protocol: TCP
        env:
        - name: "NAMESPACE"
          valueFrom:
            fieldRef:
              fieldPath: metadata.namespace
      # Elasticsearch requires vm.max_map_count to be at least 262144.
      # If your OS already sets up this number to a higher value, feel free
      # to remove this init container.
      initContainers:
      - image: alpine:3.6
        command: ["/sbin/sysctl", "-w", "vm.max_map_count=262144"]
        name: elasticsearch-logging-init
        securityContext:
          privileged: true
---

#
# Finally, Kibana - for visualising the results
#

apiVersion: apps/v1
kind: Deployment
metadata:
  name: kibana-logging
  namespace: kube-system
  labels:
    k8s-app: kibana-logging
    kubernetes.io/cluster-service: "true"
    addonmanager.kubernetes.io/mode: Reconcile
spec:
  replicas: 1
  selector:
    matchLabels:
      k8s-app: kibana-logging
  template:
    metadata:
      labels:
        k8s-app: kibana-logging
      annotations:
        seccomp.security.alpha.kubernetes.io/pod: 'docker/default'
    spec:
      containers:
      - name: kibana-logging
        image: docker.elastic.co/kibana/kibana-oss:6.2.4
        resources:
          # need more cpu upon initialization, therefore burstable class
          limits:
            cpu: 1000m
          requests:
            cpu: 100m
        env:
          - name: ELASTICSEARCH_URL
            value: http://elasticsearch-logging:9200
        ports:
        - containerPort: 5601
          name: ui
          protocol: TCP

---
apiVersion: v1
kind: Service
metadata:
  name: kibana-logging
  namespace: kube-system
  labels:
    k8s-app: kibana-logging
    kubernetes.io/cluster-service: "true"
    addonmanager.kubernetes.io/mode: Reconcile
    kubernetes.io/name: "Kibana"
spec:
  ports:
  - port: 5601
    protocol: TCP
    targetPort: ui
  selector:
    k8s-app: kibana-logging
  type: NodePort

```
This file will deploy both Kibana and Elasticsearch.


2. Configure the manifest:

This file sets up a comprehensive logging and visualization system for your Kubernetes cluster, utilizing Fluentd, Elasticsearch, and Kibana. Here's a breakdown of the key components, incorporating the additional information you provided:

Fluentd DaemonSet:

- Deployment kind: DaemonSet ensures Fluentd runs on every node of your cluster, collecting logs from various sources like Docker containers, systemd journals, and more. ServiceAccount: fluentd-es provides secure communication by granting specific permissions to Fluentd pods.

- ClusterRole: Defines permissions for Fluentd to "list, get, watch" pods and namespaces within the cluster, enabling it to discover and access log sources effectively.

- ClusterRoleBinding: Links the fluentd-es service account with the ClusterRole, establishing the necessary authorization.


Elasticsearch:

- Pod replicas: Deploys a cluster of Elasticsearch instances using StatefulSets, providing distributed storage and fault tolerance for your logs. The exact number of replicas (2 in this example) can be adjusted based on your needs and cluster resources.

- **Image source: **The configuration references the official k8s.gcr.io/elasticsearch:v6.2.5 image for deploying Elasticsearch pods.


Kibana Deployment:

- Replicas: Runs a single Kibana instance, providing a web-based interface for visualizing and analyzing logs stored in Elasticsearch.

- Image source: Utilizes the official docker.elastic.co/kibana/kibana-oss:6.2.4 image to deploy the Kibana pod.


Kibana Service:

- Type: Leverages the NodePort service type to expose Kibana through a specific port on each node, allowing external access via the assigned NodePort.

3. Apply the configuration

`kubectl apply -f elastic-stack.yaml`

![es](./images/elastic-stack.png)

4. Verify deployment

```
kubectl get pods -n kube-system
kubectl get nodes
```
- Fluentd will be created for each nodes within your cluster.

. Access Kibana:

- Get the kibana service's NodePort:

Since this is a practice project, we're using a simpler approach to access Kibana called NodePort.

This exposes Kibana on a specific port on each node in the cluster, similar to opening a specific door on each building in a practice village. While convenient, keep in mind that NodePort might be less secure than using a load balancer in real-world scenarios.

For projects requiring scalability, reliability, and advanced routing, choose a load balancer instead.

`kubectl get service kibana-logging -n kube-system`

- Forward the port to your local machine (using caution for security):

Port forwarding is useful for accessing a private service from the outside world by directing external traffic to a specific device on your network. Think of it like opening a specific door in your house for someone outside to reach a hidden room. It's handy for running personal servers, gaming with friends, or remotely accessing devices when away from home. But remember, use it with caution as it creates an open pathway, so prioritize strong passwords and proper security measures.

`kubectl port-forward service/kibana-logging -n kube-system 5601:5601`

![port](./images/port.png)


- `http://localhost:5601 in your browser`

![done](./images/kibana.png)


### Setting Up Kibana

1. Go to Management > Advanced Settings

![advanced](./images/advanced.png)

2. create an index pattern:

  - In the "Index name" field, enter "logstash-2024.02.17" (or your desired pattern).

  ![index](./images/index.png)

  - Click "Next Step".

3. Configure settings:

  - Set "Time filter field name" to "@Timestamp".

  - Click "Create index pattern".

  ![time](./images/time.png)

### Exploring Kibana

  1. Click on "Discover"

  2. Search for logs with the word "error"

![error](./images/error.png)

3. Analyze the results:

  - See the number of generated logs and timestamps.
  - Identify containers reporting errors, types of errors, and timestamps.


4. Drill down for specific pods:

  - Search for logs based on pod names if you know which ones have issues.

5. View logs at different levels:

  - Explore logs at pod, namespace, and other levels.


### Additional Considerations

- Production deployments:
  - Use an internal load balancer instead of NodePort for better security.
  - Move Kibana to a dedicated namespace.
  - Avoid kubectl proxy due to potential vulnerabilities.

- Further options:
  - Explore external tools and integrations for advanced log analysis.

### Automating Kibana watchers

Building a Pie Chart in Kibana

This guide walks you through building a pie chart in Kibana to visualize your collected log data:

1. Access Kibana's Visualization Menu:

- Navigate to "Discover" within Kibana.
- Click on "Visualizations" in the top menu bar.
- Select "Create Visualization" to begin creating your custom chart.

2. Choose the Pie Chart Visualization Type:

- From the available visualization options, select "Pie" to create a circular representation of your data.

3. Select the Data Source:

- In the "Data source" dropdown, choose "logstash-2024.02.17" (or your desired index pattern).

4. Configure Chart Options:

- Uncheck the "Donut" box to display solid pie slices instead of a hollow ring.
- Check the "Show labels" box to display labels for each slice.

5. Define Slices and Aggregation:

- Under "Data" settings, locate the "Buckets" section.
- Choose "Split slices" to separate data into individual slices.
- Under "Aggregate," select "Terms" to group data by its values.
- In the "Field" dropdown, choose "@TimeStamps" to analyze timestamps.

6. Preview and Save:

- Click the "Play" symbol (>) to apply your configured settings and preview the updated chart.
- Adjust labels or further customizations as needed.
- Click "Save" to store your created visualization for future use.


![pie](./images/pie.png)

Additional Tips:

- Use filters to narrow down your data and focus on specific timeframes or events.
- Experiment with different visualization types like bar charts or line graphs for diverse insights.
- Explore Kibana's documentation for more advanced chart customization and data analysis techniques.

### Dashboard Management

1. Access Dashboard Management:

- Navigate to the "Dashboards" tab in Kibana.
- Click on "Create dashboard" to start building your personalized dashboard.

2. Add the Pie Chart Panel:

- Click the "+" icon in the panel section of your dashboard.
- Select "Visualization" from the available options.
- In the search bar, type the title or description of your saved pie chart (e.g., "Pie Chart - Timestamp Distribution").
- Click on the visualization to add it to your dashboard.


3. Customize the Panel:

- You can resize the panel by dragging its edges.
- Click the gear icon (⚙️) to access panel settings.
- Adjust the title, color scheme, or other settings according to your preferences.

4. Add Additional Panels (Optional):

- Repeat steps 2 and 3 to add other visualizations to your dashboard, such as:
- Line charts to track trends over time.
- Bar charts to compare different categories.
- Map visualizations to analyze geo-distributed data.
- Arrange your panels strategically for optimal information flow and insights.

5. Save and Share Your Dashboard:

- Click the "Save" button to store your personalized dashboard.
- Give it a descriptive name for easy identification.
- You can share your dashboard with other users who have access to Kibana.

### Tips for Effective Dashboards:

- Combine various visualization types to provide diverse perspectives on your data.
- Label panels clearly and use intuitive layouts for easy understanding.
- Filter data consistently across visualizations for meaningful comparisons.
- Consider using color schemes that are accessible and visually appealing.













