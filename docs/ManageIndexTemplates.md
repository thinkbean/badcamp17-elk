# Managing the Index Template

You need to install index templates before any documents are added to indices which 
are to inherit the template settings.

The code on this page is intended to run in the `DevTools` interface of Kibana.

## List Index Templates
```
GET _template
```

## Add/Update the `access` template

```
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

## List the `access` template.

```
GET _template/access
```

## Delete the `access` template.

```
DELETE _template/access
```