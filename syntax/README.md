# SQL Syntax 
ANSI SQL is supported as standard.

## SELECT

### Synopsis

    SELECT [ ALL | DISTINCT ] select_expr [, ...]
    [ FROM from_item [, ...] ]
    [ WHERE condition ]
    [ GROUP BY [ ALL | DISTINCT ] grouping_element [, ...] ]
    [ HAVING condition]
    [ { UNION | INTERSECT | EXCEPT } [ ALL | DISTINCT ] select ]
    [ ORDER BY expression [ ASC | DESC ] [, ...] ]
    [ LIMIT [ count | ALL ] ]

### Parameters

1) `[ ALL | DISTINCT ] select_expr [, ...]`

`select_expr` determines the rows to be selected.

`ALL` is the default. Using `ALL` is treated the same as if it were omitted; all rows for all columns are selected and duplicates are kept.

Use `DISTINCT` to return only distinct values when a column contains duplicate values.

2) `[ FROM from_item [, ...] ]`

Indicates the input of the query, where `from_item` can be:

- `{catalog_name}.{schema_name}.{table_name}`

OR

- `join_type from_item [ ON join_condition | USING ( join_column [, ...] ) ]`

where `join_type` is one of:

    [ INNER ] JOIN
    LEFT [ OUTER ] JOIN
    RIGHT [ OUTER ] JOIN
    FULL [ OUTER ] JOIN
    CROSS JOIN

3) `[ WHERE condition ]`

Filters results according to `condition`

4) `[ GROUP BY [ ALL | DISTINCT ] grouping_element [, ...] ]`

Divides the output of statement into rows with matching values. 

5) `[ HAVING condition]`

Used with `GROUP BY` clause to control which groups are selected

6) `[ { UNION | INTERSECT | EXCEPT } [ ALL | DISTINCT ] select ]`

Combines the results of more than one `SELECT` query into a single query. 

7) `[ ORDER BY expression [ ASC | DESC ] [, ...] ]`

Sorts result by one or more expression.

8) `[ LIMIT [ count | ALL ] ]`

Restricts the number of rows in result.

### Examples

    // Gets crime from January 2018, 
    // Join with rental value by postcode
    // Filter by postcode 
    // Order by crime period
    // Limit to 10 results
    
    SELECT crime.postcode, crime.period, rent.household_rental_value_1_bedroom
    FROM doordastats_snapshot.doordastats_snapshot.eng_area_reported_crime crime
    LEFT JOIN doordastats_snapshot.doordastats_snapshot.eng_household_rental_value rent
    ON crime.postcode = rent.postcode
    WHERE crime.period >= 201801 and crime.postcode = 'CA7 4DP';
    ORDER BY crime.period
    LIMIT 10;

## SHOW

List available catalogs. `LIKE` clause can be used to restrict catalog names

    SHOW CATALOGS [ LIKE pattern ];

List schemas in catalog. `LIKE` clause can be used to restrict schema names

    SHOW SCHEMAS [ FROM {catalog} ] [ LIKE pattern ]

List tables in schema. `LIKE` clause can be used to restrict table names 

If {catalog} is not specified, schema is resolved relative to current catalog.

    SHOW TABLES [ FROM {catalog}.{schema} ] [ LIKE pattern ]

List columns in table along with data type and other attributes.

    SHOW COLUMNS FROM {catalog}.{schema}.{table};

## USE

Update session to use specific catalog and schema. If catalog is not specified, schema is resolved relative to current catalog.

    USE {catalog}.{schema};
    USE {schema};
    
    # Examples
    
    USE doordabiz_snapshot.doordabiz_snapshot;
    USE doordabiz_snapshot;


Back to [Tables of Content](../README.md#getting-started-guide-to-host)