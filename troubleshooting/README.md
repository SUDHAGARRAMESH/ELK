
## UseCases

* Cluster is <b>yellow</b>
      Usually this happens when replica shards are not getting assigned to elasticsearch cluster
 
 * Cluster is <b>red</b>
      This would happen in below following cases
      1. Due to high ingestion rate at that time 
      2. Primary shards are in unassigned state
      3. Disk space reached its watermark/threshold and the indexing happens at that time
      
 * CPU consumption is <b>high</b>
     1. Check the Ongoing Tasks
     2. Check the Hot Threads
     3. Check the Thread Pool
     
     <b>FIX:</b> consider the heapspace of the cluster too by scaling this will be mitigated<br>
     
## API Queries

 * View the indices creation date
 ```console
 _cat/indices?h=i,creation.date.string
 ```
 * Check the Cluster Health 
 ```console
 _cluster/health?pretty=true 
 ```
 <p> To view more api queries . Click the following link <a href="https://github.com/SUDHAGARRAMESH/ELK/blob/master/API%20Queries/common%20troubleshooting%20queries.md" target="_blank">troubleshooting queries</a> 
   
   
 
