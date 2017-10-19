# Kibana Dev Console

Kibana DevTools is a UI that allows you to run commands against Elasticsearch.

You can access DevTools by opening Kibana in your browser and clicking on the DevTools tab on the left.

http://192.168.56.101:5601/app/kibana#/dev_tools/console

Below are examples of useful commands that pertain to this project.

Copy & paste the text below in its entirety into DevTools.

You will see a "Play" arrow to the right of each command. Click the icon to execute each query.

```
# Replace `access-myproject` in the examples below with the index you want to operate on.

# List indices
GET _cat/indices

# Delete all access indices
DELETE access-*

# Delete indices explicitly
DELETE access-myproject

# View the mapping of an index
GET access-myproject/_mapping

# List templates
GET _template

# Get access template
GET _template/access

# Delete access template
DELETE _template/access

PUT _template/access
{
  "template": "access-*",
  "settings": {
    "index": {
      "number_of_shards": "4",
      "number_of_replicas": "0",
      "refresh_interval": "10s"
    }
  },
  "mappings": {
    "access": {
      "_meta": {
        "version": "1.0"
      },
      "_all": {
        "enabled": false
      },
      "dynamic": true,
      "properties": {
        "@timestamp": {
          "type": "date"
        },
        "@version": {
          "type": "keyword"
        },
        "type": {
          "type": "keyword"
        },
        "project": {
          "type": "keyword"
        },
        "env": {
          "type": "keyword"
        },
        "host": {
          "type": "keyword"
        },
        "vhost": {
          "type": "keyword"
        },
        "domain": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword"
            }
          }
        },
        "auth": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword"
            }
          }
        },
        "httpversion": {
          "type": "keyword"
        },
        "verb": {
          "type": "keyword"
        },
        "path": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword"
            }
          }
        },
        "referrer": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword"
            }
          }
        },
        "response": {
          "type": "short"
        },
        "request_time": {
          "type": "long"
        },
        "bytes": {
          "type": "long"
        },
        "request_id": {
          "type": "keyword"
        },
        "ip": {
          "type": "ip",
          "doc_values": true
        },
        "geoip"  : {
          "dynamic": true,
          "properties" : {
            "ip": { "type": "ip" },
            "location" : { "type" : "geo_point" },
            "latitude" : { "type" : "half_float" },
            "longitude" : { "type" : "half_float" }
          }
        },
        "agent": {
          "type": "text"
        }
      }
    }
  }
}
```
