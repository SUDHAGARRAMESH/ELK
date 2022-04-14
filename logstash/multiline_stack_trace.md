#Multiline Stack Trace in RSVP log

# The aim is to ingest only the "TRACE" level logs . This can be achieved using filter plugin


```console
input {


#for testing this below file has been created
	#file{
	#	path => "../logstash/RSVP_agent_logs.txt"
	#	start_position =>"beginning"
	#}
	
	beats{
		port => 5044
	}
	
	#any line that begins with whitespace up to the previous line
	stdin {
    codec => multiline {
      pattern => "^\s"
      what => "previous"
    }
  }
}


filter {

#THIS will exclude the lines starts with space
if [message] == "" {
    drop { }
  }
  
if [headers] [request_path] = ~ "TRACE"
{
mutate{
replace => "TRACE"
}

else if [headers] [request_path] = ~ "ERROR" or [request_path] = ~ "WARN"  {
mutate{
replace => "ERROR"
}
}
}
    grok {
      match => { "message" => "%{MONTHNUM:actual_month}/%{YEAR:year} %{TIME:time} %{LOGLEVEL:log-level}  :..%{GREEDYDATA:syslog_message}"
	  }
#if any issues with grok filter that will be tagged with grokparse failure which we are eliminating here below

    if "_grokparsefailure" in [tags]
    {
    drop{}
    }
    
    }
}

output{

#for testing this below file has been created before ingesting it to elasticsearch  
	#file
	#{
	#path => "G:/Logstash/logstash-7.16.1/config/rsvp_output.txt"
	#}
  
	elasticsearch{
	hosts => ["http://localhost:9200"]
	index => "rsvp"
	user => "elastic"
	password => "<enter your elastic root password>"
	}
	
	stdout{
		codec => rubydebug
	}
}
```
