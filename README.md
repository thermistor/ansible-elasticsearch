# elasticsearch

Install a single node [Elasticsearch](https://www.elastic.co/elasticsearch) from the Elastic apt repo, pinned to an exact version.

The package is held with `dpkg`, so only this role changes the Elasticsearch version. Change `elasticsearch_version` and re-run the role to upgrade.

By default Elasticsearch listens on `localhost:9200` with security (auth and TLS) turned off, so only processes on the same host can reach it.

## Vars

Here are the defaults:

    elasticsearch_version: '9.5.6'
    elasticsearch_repo_key_url: https://artifacts.elastic.co/GPG-KEY-elasticsearch
    elasticsearch_repo_uri: "https://artifacts.elastic.co/packages/{{ elasticsearch_version.split('.') | first }}.x/apt"
    elasticsearch_heap_size: 1g
    elasticsearch_cluster_name: elasticsearch
    elasticsearch_node_name: "{{ inventory_hostname }}"
    elasticsearch_network_host: localhost
    elasticsearch_http_port: 9200
    elasticsearch_data_dir: /var/lib/elasticsearch
    elasticsearch_log_dir: /var/log/elasticsearch
    elasticsearch_security_enabled: false
    elasticsearch_config: {}

`elasticsearch_config` is merged over the settings the role writes to `elasticsearch.yml`, so it can add or override any setting.

Setting `elasticsearch_security_enabled: true` also needs the TLS settings to be supplied through `elasticsearch_config`.

## Upgrading major versions

Elasticsearch can only read indices created by the previous major version. Going from 7.x straight to 9.x needs the indices rebuilt or reindexed, not just a package upgrade.

## Examples

Here is the basic usage:

    - hosts: servers
      roles:
        - role: thermistor.elasticsearch
          elasticsearch_version: '9.5.6'
          elasticsearch_heap_size: 2g
