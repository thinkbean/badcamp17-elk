# Transport logs to Elasticsearch from Acquia, Pantheon, or Platform.sh.

This repo contains documentation, configuration, and strategy for transforming
 and transporting access logs from Acquia, Pantheon, and Platform.sh into Elasticsearch.
 
The documentation should allow you to start from scratch with Ubuntu 16.04, or get
a jump start with a VM provided which already has Elasticsearch, Logstash, and Kibana installed.

The goal here is to make installation and configuration approachable with thorough documentation
and example configurations for each of the mentioned hosting providers.

## The Strategy

We can't run Logstash directly on these hosting providers, so the next best thing is to sync the logs
onto a server we have full control over, and have Logstash format the data, add GeoIP information,
and send the data over to Elasticsearch.

We'll setup SSH keys so we can pull the logs down with rsync, then schedule the synchronization with cron.

Elasticsearch can run on the same system (as it is on the VM) or, you can use a managed instance of Elasticsearch
from a provider like elastic.co. The documentation here provides instruction for the case where you want to install
and manage your own Elasticsearch instance.
 
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
