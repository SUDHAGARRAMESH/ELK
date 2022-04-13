<h1> Protection against data loss </h1>

Some security use cases are more valuable to run seamless business. As part of that we have to strictly monitor all the endpoints for an abnormal volume of data egress 
(also across channels) and set alerts on any anomalies based on past behavior. 

By taking this particular use case onto the focus, will implement the alert


<h2> Implementation stategy </h2>
Using Auditbeat will monitor particular paths by which a transfer,deletion of files happened and if these below criterias met then alerts would be sent


Steps:

```console
auditbeat config (main lines to be added)

#check any files shared to ms teams or via mail

#if windows

#if linux related system

```

```console
logstash 

#check any files transferred/copied to any other location (main lines to be added in your config)

#check any files deleted/gone (main lines to be added in your config)

```

<h1> Once this has been done create index patterns and check in kibana whether the datas been received </h1>



```console
#watcher

PUT _xpack/watcher/watch/filepath_monitor
{
  "trigger" : { "schedule" : { "interval" : "1m" }},
  "input" : {
    "search" : {
      "request" : {
        "indices" : [ "<Name-of-your-beat>-*" ],
        "body" : {
          "query": {
    "bool": {
      "filter": [ 
        { "range":  { "docker.memory.usage.pct": {"gte": "0.95"} }}, 
        { "range": { "@timestamp": { "gte": "now-2m" }}} 
      ]
    }
  }
        }
      }
    }
  },
  "condition" : {
    "compare" : { "ctx.payload.hits.total" : { "gt" : 0 }}
  },
  "actions" : {
    "email_administrator" : {
      "throttle_period": "5m", 
      "email" : { 
        "to" : ["<recipient1>", "<recipient2>"],
        "subject" : "Data Loss alert",
        "body" : "{{#ctx.payload.hits.hits}} {{_source.container.name}} {{/ctx.payload.hits.hits}} data loss has been happened",
        "attachments" : {
          "attached_data" : {
            "data" : {
              "format" : "json"
            }
          }
        },
        "priority" : "high"
      }
    }
  }
}


```
