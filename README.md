Hello ELK enthusiasts,

This blog will make you understand how to get interact with ELK and make use of its feature effectively.


#<p>Topics to be Covered:</p>
   1. [How to Trouble shoot Elastic Cluster Related issues](#troubleshooting)
   2. [How to interact with Kibana]
   3. [How to interact with logstash (metricbeat, filebeat, heartbeat)]
   4. [How to interact with SQL](#SQL)
   5. [How to interact with esRally]

<details>
<b><summary>What are you taking into consideration when reading this blog?</summary><br></b>

This blog will be of this below partition
   * use case
   * api query
   * automation/monitoring
</details>

## troubleshooting

### Use cases

* Cluster is <b>yellow</b>
      Usually this happens when replica shards are not getting assigned to elasticsearch cluster
 
 * Cluster is <b>red</b>
      This would happen in below following cases
      1. Due to high ingestion rate 
      2. Primary shards are in unassigned state
      3. Disk space reached its watermark/threshold and the indexing happens at that time
      
 * CPU consumption is <b>high</b>
     1. check the ongoing tasks
     2. Check the hot threads
     3. Check the thread pool
     
     FIX: consider the heapspace of the cluster too by scaling this will be mitigated

 
## SQL
 
