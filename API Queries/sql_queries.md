* To view the documents inside an index (or) in SQL terms to view the rows of a table



   <p> Example1 </p>

 ```console
 POST _sql?format=txt
{
  "query": "SELECT * FROM \"sample-sql-events\""
}
 ```
 
 * To view the particular documents inside an index (or) in SQL terms to view the details inside particular column in a table

 
 <p> Example2 </p> 
 
```console

POST /_sql?format=txt
{
  "query": "select \"Name\" ,\"Designation\" from \"sample-sql-events\""
}
 ```
 
 NOTE: Here "Name" and "Designation" are columns and the table here is "sample-sql-events" 
