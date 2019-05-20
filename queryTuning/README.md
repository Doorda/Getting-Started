# Query Tuning

## Optimize ORDER BY
ORDER BY clause returns results of a query in a sort order.
To do so, DoordaHost consolidates the results from multiple worker nodes into a single node and then
sorts them. Although Spill to Disk feature is enabled to prevent out of memory error by
offloading intermediate operation results to NVME Storage, it could still cause memory pressure on the single
worker sorting the results. This may result in long execution time or failed queries.

Use a LIMIT clause to reduce the cost of sort significantly by pushing the sorting and limiting to individual worker nodes rather than being done by a single worker

Good:
```
SELECT * FROM table_name ORDER BY date_added LIMIT 1000;
```

Bad:
```SQLSQL
SELECT * FROM table_name ORDER BY date_added;
```

## Optimize JOINs
When joining 2 tables, specify the larger table on the left side of the join
and the smaller table on the right side of the join.
DoordaHost distributes the table on the right to multiple worker nodes and them streams the table on the left to do the join.
So if the right table is smaller, it results in less memory used and thus faster queries.



## Optimize GROUP BY

GROUP BY operator distributes rows based on GROUP BY columns to worker nodes,
which holds the values in memory. As rows are ingested, GROUP BY columns are looked
up in memory and the values compared. If the GROUP BY columns match, the values are then aggregated together.

1) Order GROUP BY columns from the highest cardinality to lowest cardinality (most unique to least unique)

2) Use numbers instead of strings within GROUP BY clause.
Numbers require less memory to store and faster to compare than strings.
The numbers represents the location of the grouped column in the SELECT statement.

```
SELECT urn, company_number, count(*)
FROM table_name
GROUP BY 1, 2;
```


## Optimize LIKE
When filtering for multiple values in a string column, it is generally better to use the
built in regular expression functions `regexp_like` instead of the `LIKE` clause.

Good:
```
SELECT count(*) FROM lineitem WHERE regexp_like(l_comment, 'wake|regular|express|sleep|hello')
```


Bad:
```
SELECT count(*) FROM lineitem WHERE l_comment LIKE '%wake%' OR l_comment LIKE '%regular%' OR l_comment LIKE '%express%' OR l_comment LIKE '%sleep%' OR l_comment LIKE '%hello%
```


## Column Selection
When running your queries, limit the final SELECT statement to only the columns that you
need instead of selecting all columns. Trimming the number of columns reduces the amount of
data that needs to be processed through the entire query execution pipeline. This especially
helps when you are querying tables that have large numbers of columns that are string-based,
and when you perform multiple joins or aggregations.


