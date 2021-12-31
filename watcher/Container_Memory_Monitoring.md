**The below code will check the container memory usage every 1 minute using metricbeat with the memory threshold more than 85%
**

```console
PUT _xpack/watcher/watch/Container_Memory_Monitoring
{
  "trigger" : { "schedule" : { "interval" : "1m" }},
  "input" : {
    "search" : {
      "request" : {
        "indices" : [ "container-metricbeat-*" ],
        "body" : {
          "query": {
    "bool": {
      "filter": [ 
        { "range":  { "docker.memory.usage.pct": {"gte": "0.85"} }}, 
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
  }
 }
```
