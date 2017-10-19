# Ship access logs to Elasticsearch from Pantheon, Platform, Acquia, and other providers.

This repo contains strategy, configuration, and documentation for transporting
 access logs from Acquia, Pantheon, and Platform.sh into Elasticsearch.
 
A Ubuntu 16.04 based VM with Elasticsearch, Logstash, & Kibana (ELK) along with configuration examples,
 is available to learn and test with.
 
Detailed instructions on how the VM was created is also available for you
to spin up your own instance.

# The ELK Stack

- Elasticsearch: A highly performant document store with a robust search capability
- Logstash: The application used to transform log strings into structured data
- Kibana: A front-end used to interface with Elasticsearch

Our goal is to sync, transform, and transport logs from our hosting provider
to Elasticsearch.

You can use a hosted version of Elasticsearch and Kibana, however you will still need
to setup a server to pull down the logs (with rsync) and and transform/transport them
with Logstash.

## Use Cases

Besides having all your logs in a central location, the ability to analyze with explicit queries is powerful.

- Find pages that are regularly causing errors
- Find most or least visited pages
- Trace access paths by IP
- Find largest pages either in bytes or time
- Programmatically monitor access patterns (i.e. write a script to alert when password/recover has been accessed more than 10 times per hour by a single IP)

## The Strategy

- Rsync logs in from the remote hosts each minute via cron
- Transform the log data with Logstash
- Send the formatted logs over to Elasticsearch

## High Level Setup Overview

Below are the general steps involved with installing and configuring a system to sync, transform, and ship logs.

- Setup a server (Ubuntu 16.04 will be used for all examples)
- Install Elasticsearch and Kibana (Optional if you prefer to use a hosted instance of Elasticsearch)
- Install index templates into your Elasticsearch instance
- Install Logstash
- Generate an SSH key and add it to the hosting providers you are accessing
- Create a script to rsync logs from one or more remote hosts
- Configure logstash and start shipping logs
 
## Documentation

If you want to use the VM, [Start Here](docs/VirtualBoxVM.md) and then skip to the SSH/Rsync Setup below.

- [Elasticsearch & Kibana Installation](docs/InstallElasticsearchKibana.md) (Already done on the VM)
- [Manage Index Template](docs/ManageIndexTemplates.md) (Already done on the VM)
- [Logstash Install](docs/InstallLogstash.md) (Already done on the VM)

- [SSH/Rsync Setup](docs/SetupLogSync.md)
- [Logstash Configuration](docs/ConfigureLogstash.md)

Note: Logs need to exist in Elasticsearch before proceeding defining index patterns in Kibana.

- [Index Patterns in Kibana](docs/ConfigureKibana.md)

## Reference
- [Access Log Format by Host](docs/AccessLogFormatByHost.md)
- [Kibana DevTools](docs/KibanaDevTools.md)
