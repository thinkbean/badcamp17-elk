# Install Logstash

This documentation was written for Ubuntu 16.04.

For other Ubuntu versions, or flavors, visit the official [Elastic Stack documentation](https://www.elastic.co/guide/en/elastic-stack/current/installing-elastic-stack.html).

If you have already installed Java and the Elastic repo in the Elasticsearch setup,
skip ahead to the `Install Logstash` step.

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

## Install Logstash
```
sudo apt-get install logstash
```

## Set the group of log files to logstash
```
cd ~/
find ./projects -type d -exec chgrp logstash {} +
find ./projects -type d -exec chmod g+s {} +
```

## Configure Logstash

See the [Logstash Configuration](ConfigureLogstash.md) documentation to to configure Logstash inputs, filters,
and output.
