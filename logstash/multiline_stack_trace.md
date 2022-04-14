#Multiline Stack Trace in RSVP log




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
    grok {
	
      match => { "message" => "%{MONTHNUM:actual_month}/%{YEAR:year} %{TIME:time} %{LOGLEVEL:log-level}  :..%{GREEDYDATA:syslog_message}"
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
