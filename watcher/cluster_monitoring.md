```console
{
  "trigger": {
    "schedule": {
      "interval": "1m"
    }
  },
  "input": {
    "search": {
      "request": {
        "search_type": "query_then_fetch",
        "indices": [
          "filebeat*"
        ],
        "rest_total_hits_as_int": true,
        "body": {
          "query": {
            "bool": {
              "should": [
                {
                  "match_phrase": {
                    "message": "Could not index event to Elasticsearch"
                  }
                },
                {
                  "match_phrase": {
                    "message": "Attempted to send a bulk request to elasticsearch"
                  }
                },
                {
                  "match_phrase": {
                    "message": "java.lang.OutOfMemoryError"
                  }
                },
              }
              ],
              "minimum_should_match": 1,
              "filter": [
                {
                  "term": {
                    "log.file.path": "<YOUR PATH>xxx-json.log"
                  }
                },
                {
                  "range": {
                    "@timestamp": {
                      "gte": "now-2m"
                    }
                  }
                }
              ]
            }
          }
        }
      }
  },
  "condition": {
    "compare": {
      "ctx.payload.hits.total": {
        "gt": 0
      }
    }
  },
  "actions": {
    "email_administrator": {
      "throttle_period_in_millis": 300000,
      "email": {
        "profile": "standard",
        "attachments": {
          "attached_data": {
            "data": {
              "format": "json"
            }
          }
        },
        "priority": "high",
        "to": [
          "<ENTER YOUR MAIL ID HERE>"
        ],
        "subject": "Observed Errors in <NAME OF YOUR CHOICE> ",
        "body": {
          "text": " We have errors in this filebeat*.please find the attachment "
        }
      }
    }
  }
}
```
