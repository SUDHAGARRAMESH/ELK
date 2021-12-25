<p>NOTE: This below queries will do the <b>Read</b> operation hence use this "GET" method</p>

* To view the indices creation date
 ```console
 _cat/indices?h=i,creation.date.string
 ```
 
 * To check the Cluster Health status
    *  output will be viewed  
 ```console
 _cluster/health?pretty=true 
 ```
```console
 _cluster/health?format=yaml
 ```
 * To check the Cluster Health 
 ```console
 _cluster/health?pretty=true 
 ```
  
 * To check the index health status 
 
 ```console
 _cat/indices?v
 ```
 
 ```console
 _cat/indices?v&h=health,status,index,pri.store.size
 ```
 
 * To check the particular index settings 
  <b>SYNTAX</b> GET <INDEX_NAME>/_settings?pretty
  
 * In this below console , <b>simpleindex</b> is the name of the index
  
```console
simpleindex/_settings?pretty
```

 
