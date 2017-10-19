# Configure & Manage Logstash

Unless you are using a cloud provider, you should only need to configure the logstash inputs.

Logstash configs exist in `/etc/logstash/conf.d`.

With our approach, we will add 1 input file per log file.

For example, here is a directory list for 3 environments, one for each hosting provider we are providing examples for:

```
010_acquia-proj_access_input.conf
010_pantheon-proj_access_input.conf
010_platform-proj_access_input.conf
050_filter_access.conf
100_output_elasticsearch.conf
```

Input files: Define log file source and add some additional project data
Filter file: Transformation patters and definitions
Output file: Defines where to send formatted data to

Refer to the [etc/logstash/conf.d](../etc/logstash/conf.d) directory of this repository for all examples.

## Input file example:

Create an input file for each project/environment.

Be sure to update `path`, `hp`, `project`, `env`, and `xhost` with values relevant to the project.

The `hp` field is where you define the hosting provider. Use `acquia`, `pantheon`, or `platform` accordingly.

`xhost` gets copied over to the `host` value of the document. This may be useful if you are receiving logs
from more than 1 host and you want to differentiate what server the log came from.

`/etc/logstash/conf.d/010_myproject`
```
input {
  file {
    path => "/home/ubuntu/projects/myproject/access.log"
    start_position => "beginning"
    type => "access"
    add_field => {
      "hp" => "platform"
      "project" => "myproject"
      "env" => "prod"
      "xhost" => "platform-proj-www01"
    }
  }
}
```

## Filter file example

This file is setup with grok patterns to match access logs by Acquia, Pantheon, and Platform.sh.

The pattern to match is determined by the `hp` field defined in the input file.

`/etc/logstash/conf.d/050_filter_access.conf`
```
filter {
  if [type] == "access" {
    if [hp] == "acquia" {
      grok {
        match => [
          "message", "(?:%{IPORHOST:ip}|-) - (?:%{USER:auth}|-) \[%{HTTPDATE:timestamp}\] \"(?:%{WORD:verb} %{NOTSPACE:request}(?: HTTP/%{NUMBER:httpversion})?|%{DATA:rawrequest})\" %{NUMBER:response} (?:%{NUMBER:bytes}|-) \"%{DATA:referrer}\" \"%{DATA:agent}\" vhost=%{IPORHOST:vhost} host=%{IPORHOST:domain} hosting_site=%{WORD} pid=%{NUMBER} request_time=%{NUMBER:request_time} forwarded_for=\"(?:%{IPORHOST:forwarded_for}|)(?:, %{IPORHOST}|)(?:, %{IPORHOST}|)\" request_id=\"%{NOTSPACE:request_id}\""
        ]
      }
    }
    else if [hp] == "platform" {
      grok {
        match => [
          "message", "(?:%{IPORHOST:ip}|-) - (?:%{USER:auth}|-) \[%{HTTPDATE:timestamp}\] \"(?:%{WORD:verb} %{NOTSPACE:request}(?: HTTP/%{NUMBER:httpversion})?|%{DATA:rawrequest})\" %{NUMBER:response} (?:%{NUMBER:bytes}|-) \"%{DATA:referrer}\" \"%{DATA:agent}\""
        ]
      }
    }
    else if [hp] == "pantheon" {
      grok {
        match => [
          "message", "(?:%{IPORHOST:ip}|-) - (?:%{USER:auth}|-) \[%{HTTPDATE:timestamp}\] (?: )\"(?:%{WORD:verb} %{NOTSPACE:request}(?: HTTP/%{NUMBER:httpversion}))\" %{NUMBER:response} (?:%{NUMBER:bytes}|-) \"%{DATA:referrer}\" \"%{DATA:agent}\" %{NUMBER:request_time} \"(?:%{IPORHOST:forwarded_for}|)(?:, %{IPORHOST}|)(?:, %{IPORHOST}|)\""
        ]
      }
    }

    mutate {
      update => { "host" => "%{xhost}" }
      replace => { "path" => "%{request}" }
    }
    if (![ip] and [forwarded_for]) {
      mutate {
        add_field => {
          "ip" => "%{forwarded_for}"
        }
      }
    }
    geoip { source => "ip" }
    date {
      locale => "en"
      match => [ "timestamp", "dd/MMM/yyyy:HH:mm:ss Z" ]
      target => "@timestamp"
    }
    mutate {
      remove_field => [ "forwarded_for", "message", "request", "timestamp", "xhost" ]
    }
  }
}

```

## Output file example

Set the host & port of your Elasticsearch instance in the `hosts` field.

For hosted ES instances, or if you are doing anything more serious than exploring, there should be
a user and password required.

Simple add `user` and `password` fields and their values in the `elasticsearch` block.

```
output {
  elasticsearch {
    hosts => [ "http://localhost:9200" ]
    index => "access-%{[project]}-%{[env]}"
  }
}
```

## Start Logstash

With inputs, filters, and outputs defined, you can now start Logstash.

```
systemctl start logstash
```

Once logs are successfully being transformed and sent to Elasticsearch, you can
[Create index patterns](ConfigureKibana.md) in Kibana and start analyzing your data.

## Troubleshooting
- Tail the logstash log file on startup: `tail -f /var/log/logstash/logstash-plain.log`
- Make sure log files are in the logstash group, or owned by logstash
- Debug grok patterns with [Grok Debugger](https://grokdebug.herokuapp.com/)

## Purge data and start over
- Stop Logstash
- Delete indices
- Delete `.syncdb_*` files
```
# Path may differ per method of install.
sudo rm /var/lib/logstash/plugins/inputs/file/.sincedb_*
```
- Start Logstash
