# elasticsearch

[![Build Status](https://travis-ci.com/iroquoisorg/ansible-role-elasticsearch.svg?branch=master)](https://travis-ci.com/iroquoisorg/ansible-role-memcached)

Ansible role for elasticsearch

This role was prepared and tested for Ubuntu 16.04.

# Installation

`$ ansible-galaxy install iroquoisorg.elasticsearch`

# Default settings

```

# .. vim: foldmarker=[[[,]]]:foldmethod=marker

# debops.elasticsearch default variables [[[
# ==========================================

# .. contents:: Sections
#    :local:


# Basic configuration [[[
# --------------------

# .. envvar:: elasticsearch_version
#
# Elasticsearch version. Supported are the 1.x.y and 2.x.y releases from
# the upstream Apt repository. To install the latest minor release, set
# this to e.g. ``2.x``.
elasticsearch_version: '6.4'

# .. envvar:: elasticsearch_cluster_name
#
# Cluster name.
elasticsearch_cluster_name: 'elasticsearch'

                                                                   # ]]]
# Node configuration [[[
# -------------------

# .. envvar:: elasticsearch_node_name
#
# Node name. An empty string will let Elasticsearch randomly generate one.
elasticsearch_node_name: ''


# .. envvar:: elasticsearch_node_rack
#
# Add a node attribute ``rack`` which can be used for shard allocation
# awareness.
elasticsearch_node_rack: 'nicerack'


# .. envvar:: elasticsearch_node_max_local_storage_nodes
#
# Number of nodes per server which can share the same data path. For more
# information see the upstream documentation of `max_local_storage_nodes
# <https://www.elastic.co/guide/en/elasticsearch/reference/2.4/modules-node.html#max-local-storage-nodes>`.
elasticsearch_node_max_local_storage_nodes: 1

                                                                   # ]]]
# Index configuration [[[
# --------------------

# .. envvar:: elasticsearch_index_shards
#
# Number of shards to which an index is distributed.
elasticsearch_index_shards: 5


# .. envvar:: elasticsearch_index_replicas
#
# Number of replicas that an index must have.
elasticsearch_index_replicas: 1

                                                                   # ]]]
# Plugins and libraries [[[
# =========================

# .. envvar:: elasticsearch_plugins
#
# A list of Elasticsearch plugins to install or delete. For more information
# see :ref:`elasticsearch__ref_plugins`.
elasticsearch_plugins: []


# .. envvar:: elasticsearch_libs
#
# A list of Java libraries which will be loaded into the Elasticsearch
# classpath. For more information see :ref:`elasticsearch__ref_libs`.
elasticsearch_libs: []

                                                                   # ]]]
# Resource configuration [[[
# -----------------------

# .. envvar:: elasticsearch_memory_mlockall
#
# Lock the process address space into RAM, preventing any Elasticsearch memory
# from being swapped out.
elasticsearch_memory_mlockall: false


# .. envvar:: elasticsearch_memory_heap_size_multiplier
#
# The heap size should be about 50% of your total RAM on a dedicated instance.
# If you are running ES with a bunch of other services don't be afraid to
# drastically lower this but be aware of performance issues if it's too low.
elasticsearch_memory_heap_size_multiplier: 0.5


# .. envvar:: elasticsearch_memory_heap_newsize
#
# The size of the new generation heap .
elasticsearch_memory_heap_newsize: ''


# .. envvar:: elasticsearch_memory_direct_size
#
# The maximum size of the direct memory.
elasticsearch_memory_direct_size: ''


# .. envvar:: elasticsearch_memory_locked_size
#
# Maximum locked memory size.
elasticsearch_memory_locked_size: 'unlimited'


# .. envvar:: elasticsearch_memory_vma_max_map_count
#
# Maximum locked memory size.
elasticsearch_memory_vma_max_map_count: 262144


# .. envvar:: elasticsearch_fs_max_open_files
#
# Maximum number of open files.
elasticsearch_fs_max_open_files: 65535


# .. envvar:: elasticsearch_fs_java_opts
#
# Force ES to use ipv4, set this to an empty string if you want to use ipv6.
elasticsearch_fs_java_opts: '-Djava.net.preferIPv4Stack=true'

                                                                   # ]]]
# Network configuration [[[
# ----------------------

# .. envvar:: elasticsearch_bind_host
#
# This specifies which network interface(s) a node should bind to in order to
# listen for incoming requests.
elasticsearch_bind_host: 'localhost'


# .. envvar:: elasticsearch_publish_host
#
# The publish host is the single interface that the node advertises to other
# nodes in the cluster, so that those nodes can connect to it.
elasticsearch_publish_host: 'localhost'


# .. envvar:: elasticsearch_node_port
#
# A bind port range for TCP protocol connections.
elasticsearch_node_port: '9300-9400'


# .. envvar:: elasticsearch_compress
#
# Enable TCP protocol compression (LZF) between all nodes.
elasticsearch_compress: false


# .. envvar:: elasticsearch_http_enabled
#
# Enable the HTTP module.
elasticsearch_http_enabled: true


# .. envvar:: elasticsearch_jsonp_enabled
#
# Enable JSONP via HTTP. Do not enable this unless you have a very good reason
# to do so. This variable is only used with ES 1.x.
elasticsearch_jsonp_enabled: false


# .. envvar:: elasticsearch_http_port
#
# A bind port range for HTTP connections.
elasticsearch_http_port: '9200-9300'


# .. envvar:: elasticsearch_http_max_content_length
#
# The max content of an HTTP request.
elasticsearch_http_max_content_length: '100mb'


# .. envvar:: elasticsearch_node_allow
#
# List of IP addresses or CIDR subnets which will be allowed via
# :command:`ip(6)tables` to connect to the Elasticsearch TCP service. If it's
# empty, remote connections are not allowed.
elasticsearch_node_allow: []


# .. envvar:: elasticsearch_multicast_allow
#
# List of IP addresses or CIDR subnets which will be allowed via
# :command:`ip(6)tables` to communicate via Elasticsearch multicast protocol
# (inter-node communication). If it's empty, remote connections are not allowed.
elasticsearch_multicast_allow: '{{ elasticsearch_node_allow }}'


# .. envvar:: elasticsearch_http_allow
#
# List of IP addresses or CIDR subnets which will be allowed via
# :command:`ip(6)tables` to access the Elasticsearch HTTP API. If it's empty,
# remote connections are not allowed.
elasticsearch_http_allow: []


# .. envvar:: elasticsearch_gateway_type
#
# Gateway type for (re)storing the state of the cluster meta data across full
# cluster restarts. If an index should not persist its state, set this to
# ``none``. This variable is only used with ES 1.x. For more information see
# `Gateway Module
# <https://www.elastic.co/guide/en/elasticsearch/reference/1.7/modules-gateway.html>`.
elasticsearch_gateway_type: 'local'


# These get dynamically set by ES, make sure you know what you're doing.
#elasticsearch_gateway_recover_after_time: ?
#elasticsearch_gateway_recover_after_nodes: ?
#elasticsearch_gateway_expected_nodes: ?

                                                                   # ]]]
# Index recovery policy [[[
# ----------------------

# .. envvar:: elasticsearch_recovery_max_bytes_per_sec
#
# Max bytes per second for index recovery data transfer.
elasticsearch_recovery_max_bytes_per_sec: '20mb'


# These get dynamically set by ES, make sure you know what you're doing.
#elasticsearch_recovery_node_initial_primaries_recoveries: ?
#elasticsearch_recovery_node_concurrent_recoveries: ?
#elasticsearch_recovery_concurrent_streams: ?

                                                                   # ]]]
# Cluster discovery [[[
# ------------------

# .. envvar:: elasticsearch_discovery_minimum_master_nodes
#
# Sets the minimum number of master eligible nodes a node should "see" in order
# to win a master election. It must be set to a quorum of your master eligible
# nodes. Consider raising this once you have > 2 nodes.
elasticsearch_discovery_minimum_master_nodes: 1


# .. envvar:: elasticsearch_discovery_ping_timeout
#
# How long to wait for a ping response.
elasticsearch_discovery_ping_timeout: '3s'


# .. envvar:: elasticsearch_discovery_multicast_enabled
#
# Whether multicast ping discovery is enabled. In ES 1.x this feature is
# supported natively. In ES 2.x the ``discovery-multicast`` plugin will be
# installed automatically if this feature is enabled.
elasticsearch_discovery_multicast_enabled: true


# .. envvar:: elasticsearch_discovery_ping_unicast_hosts
#
# List of hosts in the cluster when using unicast cluster discovery.
elasticsearch_discovery_ping_unicast_hosts: []

                                                                   # ]]]
# Logging configuration [[[
# =========================

# .. envvar:: elasticsearch_slowlog_query_warn
#
# Shard level query time threshold for slow log warning level.
elasticsearch_slowlog_query_warn: '10s'


# .. envvar:: elasticsearch_slowlog_query_info
#
# Shard level query time threshold for slow log info level.
elasticsearch_slowlog_query_info: '5s'


# .. envvar:: elasticsearch_slowlog_query_debug
#
# Shard level query time threshold for slow log debug level.
elasticsearch_slowlog_query_debug: '2s'


# .. envvar:: elasticsearch_slowlog_query_trace
#
# Shard level query time threshold for slow log trace level.
elasticsearch_slowlog_query_trace: '500ms'


# .. envvar:: elasticsearch_slowlog_fetch_warn
#
# Shard level fetch time threshold for slow log warning level.
elasticsearch_slowlog_fetch_warn: '1s'


# .. envvar:: elasticsearch_slowlog_fetch_info
#
# Shard level fetch time threshold for slow log info level.
elasticsearch_slowlog_fetch_info: '800ms'


# .. envvar:: elasticsearch_slowlog_fetch_debug
#
# Shard level fetch time threshold for slow log debug level.
elasticsearch_slowlog_fetch_debug: '500ms'


# .. envvar:: elasticsearch_slowlog_fetch_trace
#
# Shard level fetch time threshold for slow log trace level.
elasticsearch_slowlog_fetch_trace: '200ms'


# .. envvar:: elasticsearch_slowlog_index_warn
#
# Shard level index time threshold for slow log warning level.
elasticsearch_slowlog_index_warn: '10s'


# .. envvar:: elasticsearch_slowlog_index_info
#
# Shard level index time threshold for slow log info level.
elasticsearch_slowlog_index_info: '5s'


# .. envvar:: elasticsearch_slowlog_index_debug
#
# Shard level index time threshold for slow log debug level.
elasticsearch_slowlog_index_debug: '2s'


# .. envvar:: elasticsearch_slowlog_index_trace
#
# Shard level index time threshold for slow log trace level.
elasticsearch_slowlog_index_trace: '500ms'


# .. envvar:: elasticsearch_monitor_gc_young_warn
#
# Java VM young space garbage collection time threshold for warning log level.
elasticsearch_monitor_gc_young_warn: '1000ms'


# .. envvar:: elasticsearch_monitor_gc_young_info
#
# Java VM young space garbage collection time threshold for info log level.
elasticsearch_monitor_gc_young_info: '700ms'


# .. envvar:: elasticsearch_monitor_gc_young_debug
#
# Java VM young space garbage collection time threshold for debug log level.
elasticsearch_monitor_gc_young_debug: '400ms'


# .. envvar:: elasticsearch_monitor_gc_old_warn
#
# Java VM old space garbage collection time threshold for warning log level.
elasticsearch_monitor_gc_old_warn: '10s'


# .. envvar:: elasticsearch_monitor_gc_old_info
#
# Java VM old space garbage collection time threshold for info log level.
elasticsearch_monitor_gc_old_info: '5s'


# .. envvar:: elasticsearch_monitor_gc_old_debug
#
# Java VM old space garbage collection time threshold for debug log level.
elasticsearch_monitor_gc_old_debug: '2s'


# .. envvar:: elasticsearch_logger_level
#
# Global Elasticsearch log level.
elasticsearch_logger_level: 'INFO'


# .. envvar:: elasticsearch_logger_output
#
# List of output log appenders. The individual appenders are defined in
# :envvar:`elasticsearch_logger_appender`.
elasticsearch_logger_output: '{{ elasticsearch_logger_level }}, console, file'


# .. envvar:: elasticsearch_logger
#
# Log levels for individual Elasticsearch components.
elasticsearch_logger:
  action: 'DEBUG'
  amazon_aws: 'WARN'
  gateway: 'DEBUG'
  index_gateway: 'DEBUG'
  indices_recovery: 'DEBUG'
  deprecation: 'DEBUG, deprecation_log_file'
  discovery: 'TRACE'
  index_search_slowlog: 'TRACE, index_search_slow_log_file'
  index_indexing_slowlog: 'TRACE, index_indexing_slow_log_file'


# .. envvar:: elasticsearch_logger_additivity
#
# Log levels for individual Elasticsearch components.
elasticsearch_logger_additivity:
  index_search_slowlog: false
  index_indexing_slowlog: false


# .. envvar:: elasticsearch_logger_appender
#
# Log appender definitions.
elasticsearch_logger_appender:
  console:
    type: console
    layout:
      type: consolePattern
      conversionPattern: '[%d{ISO8601}][%-5p][%-25c] %m%n'
  file:
    type: dailyRollingFile
    file: ${path.logs}/${cluster.name}.log
    datePattern: "'.'yyyy-MM-dd"
    layout:
      type: pattern
      conversionPattern: '[%d{ISO8601}][%-5p][%-25c] %m%n'
  index_search_slow_log_file:
    type: dailyRollingFile
    file: ${path.logs}/${cluster.name}_index_search_slowlog.log
    datePattern: "'.'yyyy-MM-dd"
    layout:
      type: pattern
      conversionPattern: '[%d{ISO8601}][%-5p][%-25c] %m%n'
  index_indexing_slow_log_file:
    type: dailyRollingFile
    file: ${path.logs}/${cluster.name}_index_indexing_slowlog.log
    datePattern: "'.'yyyy-MM-dd"
    layout:
      type: pattern
      conversionPattern: '[%d{ISO8601}][%-5p][%-25c] %m%n'

                                                                   # ]]]
# DebOps environment [[[
# -------------------

# .. envvar:: elasticsearch_group_master
#
# Ansible inventory group name of general purpose nodes which can act as master
# but also persistently store index data.
elasticsearch_group_master: 'debops_service_elasticsearch_master'


# .. envvar:: elasticsearch_group_workhorse
#
# Ansible inventory group name of Elasticsearch worker nodes which shouldn't
# become master nodes.
elasticsearch_group_workhorse: 'debops_service_elasticsearch_workhorse'


# .. envvar:: elasticsearch_group_coordinator
#
# Ansible inventory group name of Elasticsearch master nodes which shouldn't
# store persistent data.
elasticsearch_group_coordinator: 'debops_service_elasticsearch_coordinator'


# .. envvar:: elasticsearch_group_loadbalancer
#
# Ansible inventory group name of Elasticsearch nodes which shouldn't become
# master and shouldn't store persistent data but act as load balancer or
# traffic aggregators.
elasticsearch_group_loadbalancer: 'debops_service_elasticsearch_loadbalancer'


# .. envvar:: elasticsearch__etc_services__dependent_list
#
# Configuration for ``debops.etc_services`` Ansible role.
elasticsearch__etc_services__dependent_list:
  - name: 'elasticsearch-multicast'
    port: '{{ elasticsearch_discovery_multicast_port }}'


# .. envvar:: elasticsearch__ferm__dependent_rules
#
# Configuration for ``debops.ferm`` Ansible role.
elasticsearch__ferm__dependent_rules: '{{ elasticsearch__ferm_cluster_rules +
  (elasticsearch__ferm_http_rules if elasticsearch_http_enabled|d(False) else []) }}'


# .. envvar:: elasticsearch__ferm_cluster_rules
#
# Configuration for ``debops.ferm`` Ansible rule relating to the regular
# Elasticsearch cluster traffic.
elasticsearch__ferm_cluster_rules:
  - type: 'accept'
    dport: [ '{{ elasticsearch_node_port | replace("-", ":") }}' ]
    saddr: '{{ elasticsearch_node_allow }}'
    accept_any: False
    weight: '50'
    role: 'elasticsearch_tcp'
  - type: 'accept'
    dport: [ 'elasticsearch-multicast' ]
    saddr: '{{ elasticsearch_multicast_allow }}'
    protocol: [ 'udp' ]
    accept_any: False
    weight: '50'
    role: 'elasticsearch_multicast'


# .. envvar:: elasticsearch__ferm_http_rules
#
# Configuration for ``debops.ferm`` Ansible rule relating to the Elasticsearch
# HTTP traffic.
elasticsearch__ferm_http_rules:
  - type: 'accept'
    dport: [ '{{ elasticsearch_http_port | replace("-", ":") }}' ]
    saddr: '{{ elasticsearch_http_allow }}'
    accept_any: False
    weight: '50'
    role: 'elasticsearch_http'
                                                                   # ]]]
                                                                   # ]]]

```
