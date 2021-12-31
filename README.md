Hello ELK enthusiasts,

This blog will make you understand how to get interact with ELK and make use of its feature effectively.


<p>Topics to be Covered:</p>
   1. [How to Troubleshoot Elastic Cluster Related issues](#Troubleshooting) <br>
   2. [How to interact with Kibana] <br>
   3. [How to interact with logstash (metricbeat, filebeat, heartbeat)] <br>
   4. [How to interact with SQL](#SQL) <br>
   5. [How to interact with esRally] <br>
   6. [Sample watcher creations with code] (#watcher) <br>
<br><br>
<details>
<b>What needs to be considered when you are reading this blog?<br></b>

This blog has been divided into below partition
   1. Use Cases
   2. API Queries
   3. Automation/Monitoring
</details>

## Troubleshooting

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
   
   
## SQL
  <p> To view more api queries. Click the following link  <a href="https://github.com/SUDHAGARRAMESH/ELK/blob/master/API%20Queries/sql_queries.md" target="_blank">sql_queries</a> 

## Watcher
     <p> To view more about watcher codes. Click the following link  <a href="https://github.com/SUDHAGARRAMESH/ELK/tree/master/watcher" target="_blank">watcher examples</a> 
      1. <a href="https://github.com/SUDHAGARRAMESH/ELK/blob/master/watcher/cluster_monitoring.md" target="_blank"> Cluster Monitoring using filebeat </a>
      2. <a href="https://github.com/SUDHAGARRAMESH/ELK/blob/master/watcher/Container_Memory_Monitoring.md" target="_blank"> Containers Memory Monitoring using metricbeat </a>
