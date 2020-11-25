---
layout: "elasticsearch"
page_title: "Elasticsearch: elasticsearch_opendistro_monitor"
subcategory: "Elasticsearch Open Distro"
description: |-
  Provides an Elasticsearch Open Distro monitor.
---

# elasticsearch_opendistro_monitor

Provides an Elasticsearch Open Distro monitor.
Please refer to the Open Distro [monitor documentation][1] for details.

## Example Usage

```hcl
# Create an monitor
resource "elasticsearch_opendistro_monitor" "movies_last_hour" {
  body = <<EOF
{
  "name": "test-monitor",
  "type": "monitor",
  "enabled": true,
  "schedule": {
    "period": {
      "interval": 1,
      "unit": "MINUTES"
    }
  },
  "inputs": [{
    "search": {
      "indices": ["movies"],
      "query": {
        "size": 0,
        "aggregations": {},
        "query": {
          "bool": {
            "adjust_pure_negative":true,
            "boost":1,
            "filter": [{
              "range": {
                "@timestamp": {
                  "boost":1,
                  "from":"||-1h",
                  "to":"",
                  "include_lower":true,
                  "include_upper":true,
                  "format": "epoch_millis"
                }
              }
            }]
          }
        }
      }
    }
  }],
  "triggers": []
}
EOF
}
```

## Argument Reference

The following arguments are supported:

* `body` -
    (Required) The policy document.

## Attributes Reference

The following attributes are exported:

* `id` -
    The id of the monitor.

## Import

Elasticsearch Open Distro monitor can be imported using the `id`, e.g.

```
$ terraform import elasticsearch_opendistro_monitor.alert 123abc
```

<!-- External links -->
[1]: https://opendistro.github.io/for-elasticsearch-docs/docs/alerting/monitors/
