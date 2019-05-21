# Getting Started Guide to Host :robot:	


##  Table of Contents
1. [Concepts](#concepts)
2. [Tools](tools/README.md)
    1. [Python](tools/python.md#python)
    2. [R](tools/r.md#r)
    3. [Tableau](tools/tableau.md#tableau)
    4. [Qlik](tools/qlik.md#qlik-sense)
    5. [CLI](tools/cli.md#cli)
    6. [Database Client](tools/database-client.md#database-clients)
        1. [SQLWorkbenchJ](tools/database-client.md#sqlworkbenchj)
        2. [Jetbrains DataGrip](tools/database-client.md#jetbrains-datagrip)
    7. [JDBC Driver](tools/jdbc.md#jdbc-driver)
3. [Query Syntax](syntax/README.md#syntax)
4. [Data Types](dataTypes/README.md#supported-data-types)
5. [Functions and Operators](functionsOperators/README.md#functions-and-operators)
6. [Terminologies](terms/README.md#terms)
7. [Query Tuning](queryTuning/README.md#query-tuning)

## Concepts

DoordaHost is built on Presto DB and is designed to query large datasets between our products.
It can be used for:
1) Bulk data extracts into your own data warehouse/database/data lake or simple file download.   
    We support [Python](tools/python.md) and [R](tools/r.md) for script based extraction and other languages with JDBC libraries/packages

2) Online Analytical Processing   
    DoordaHost works with various BI tools including [Tableau](tools/tableau.md#tableau) and [Qlik](tools/qlik.md#qlik-sense).
    DoordaHost also includes various [Functions and Operators](functionsOperators/README.md#functions-and-operators) which enable 
    online analysis and reduces the need to store large tables in memory when processing. 

### Catalogs    
Catalogs contains the schemas. In DoordaHost, there is a schema for each catalog, each product has it's own schema. 

Example:   
    Catalog = `doordastats_snapshot`  
    Schema = `doordastats_snapshot`
    Query Format:  `SELECT {col} FROM doordastats_snapshot.doordastats_snapshot.{table_name}`


### Schemas
Each schema contains a set of queryable tables. In DoordaHost, each Schema identifies with a Product. 
Our list of products can be found [here](terms/README.md#list-of-products)


### Tables
A table is a set of unordered rows which are organized into named columns with types. 
This is the same as in any relational database. The mapping from source data to tables is defined by the connector.

Each table has a `urn` as the primary key column which can be used to identify unique rows.

### Metadata

The metadata for the tables can be viewed via the [API](https://dev.doorda.com/#d906db60-4ad8-4e1c-96e2-22057ea39372) or using `SHOW COLUMNS FROM table` when connected to DoordaHost using a tool which supports SQL Queries.

 
