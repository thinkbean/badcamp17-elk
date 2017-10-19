# Elasticsearch & Kibana Installation and Setup

This documentation was written for Ubuntu 16.04.

For other Ubuntu versions, or flavors, visit the official 
[Elastic Stack documentation](https://www.elastic.co/guide/en/elastic-stack/current/installing-elastic-stack.html).

## Install Java
```
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
```

# Setup Elastic repo
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://artifacts.elastic.co/packages/5.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-5.x.list
```

# Install Elasticsearch
```
sudo apt-get update && sudo apt-get install elasticsearch
```

# Configure Elasticsearch

Set `network.host` to `0.0.0.0` in `/etc/elasticsearch/elasticsearch.yml`.
```
network.host: 0.0.0.0
```

# Set Elasticsearch to start automatically
```
sudo systemctl enable elasticsearch
```

# Start Elasticsearch
```
sudo systemctl start elasticsearch
```

## Install Kibana
```
sudo apt-get install kibana
```

# Configure Elasticsearch

Set `server.host` to `0.0.0.0` in `/etc/kibana/kibana.yml`.
```
network.host: 0.0.0.0
```

# Set Kibana to start automatically
```
sudo systemctl enable kibana
```

# Start Kibana
```
sudo systemctl start kibana
```
