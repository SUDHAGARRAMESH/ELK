<h1> Protection against data loss </h1>

Some security use cases are more valuable to run seamless business. As part of that we have to strictly monitor all the endpoints for an abnormal volume of data egress 
(also across channels) and set alerts on any anomalies based on past behavior. 

By taking this particular use case onto the focus, will implement the alert


<h2> Implementation stategy </h2>
Using Auditbeat will monitor particular paths by which a transfer,deletion of files happened and if these below criterias met then alerts would be sent


Steps:
1. Monitor the filepath using Auditbeat config 
2. Check any files transferred/copied to any other location or deleted using logstash
3. Get that path location and filename details
4. Using filebeat checking the contents of the file
5. Create index pattern in kibana and check the incoming events and test those
6. create a watcher based on the conditions (mentioned in step2)



```console
auditbeat config (main lines to be added)

#check any files shared to ms teams or via mail

#if windows

- module: file_integrity
  paths:
  - C:/windows
  - C:/windows/system32
  - C:/Program Files
  - C:/Program Files (x86)
  - G:/AuditBeat_test
  - C:\Users\Sudhagar\Downloads

#if linux related system
- module: file_integrity
paths:
  - /var/log/syslog
  - /var/log/messages
  - /var/log/secure
  - /var/log/boot.log
  - /var/log/mail*
  - /var/log/faillog*
  - /var/log/httpd/*


```

```console
logstash 

#check any files transferred/copied to any other location (main lines to be added in your config)

#check any files deleted/gone (main lines to be added in your config)

```

<h1> Once this has been done create index patterns and check in kibana whether the datas been received </h1>

```
<h1>Pipeline code which will take the filename and location details </h1>

```console
filter {
   if [fields][log_type] == "obapp-dotnet" {
    grok {
      #break_on_match => true
      match => {"[log][file][path]" => "%{GREEDYDATA}/%{GREEDYDATA:filename}\.log"}
    }
      #if [log][file][path] == "*.log"{
      grok{
        match => { 
	"message" => [
	   "%{DATESTAMP:timestamp}%{SPACE}%{NONNEGINT:code}%{GREEDYDATA}%{LOGLEVEL:loglevel}%{SPACE}%{NONNEGINT:anum}%{SPACE}%{GREEDYDATA:logmessage}" 
 	]
       }
      }
    #}
  }
```


<h1> alert setup if a file added or deleted or shared </h1>

```console
#watcher

PUT _xpack/watcher/watch/file_monitor
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
        {  
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
        "subject" : "Data Breach alert",
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
