# SQL Syntax 
ANSI SQL is supported as standard.

## SELECT

### Synopsis

    [ WITH with_query [, ...] ]
    SELECT [ ALL | DISTINCT ] select_expr [, ...]
    [ FROM from_item [, ...] ]
    [ WHERE condition ]
    [ GROUP BY [ ALL | DISTINCT ] grouping_element [, ...] ]
    [ HAVING condition]
    [ { UNION | INTERSECT | EXCEPT } [ ALL | DISTINCT ] select ]
    [ ORDER BY expression [ ASC | DESC ] [, ...] ]
    [ LIMIT [ count | ALL ] ]

### Parameters

1) `WITH` clause

    The `WITH` clause defines named relations for use within a query.
    It allows flattening nested queries or simplifying subqueries.
    For example, the following queries are equivalent:

    ```sql
    SELECT a, b
    FROM (
      SELECT a, MAX(b) AS b FROM t GROUP BY a
    ) AS x;


    WITH x AS (SELECT a, MAX(b) AS b FROM t GROUP BY a)
    SELECT a, b FROM x;
    ```

    Works with multiple subqueries:

    ```sql
    WITH
    t1 AS (SELECT a, MAX(b) AS b FROM x GROUP BY a),
    t2 AS (SELECT a, AVG(d) AS d FROM y GROUP BY a)
    SELECT t1.*, t2.*
    FROM t1
    JOIN t2 ON t1.a = t2.a;
    ```

    And, relations within a `WITH` clause can be chained:

    ```sql
    WITH
    x AS (SELECT a FROM t),
    y AS (SELECT a AS b FROM x),
    z AS (SELECT b AS c FROM y)
    SELECT c FROM z;
    ```



2) `[ ALL | DISTINCT ] select_expr [, ...]`

    `select_expr` determines the rows to be selected.

    `ALL` is the default. Using `ALL` is treated the same as if it were omitted; all rows for all columns are selected and duplicates are kept.

    Use `DISTINCT` to return only distinct values when a column contains duplicate values.

3) `[ FROM from_item [, ...] ]`

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

4) `[ WHERE condition ]`

    Filters results according to `condition`

5) `[ GROUP BY [ ALL | DISTINCT ] grouping_element [, ...] ]`

    Divides the output of statement into rows with matching values.

6) `[ HAVING condition]`

    Used with `GROUP BY` clause to control which groups are selected

7) `[ { UNION | INTERSECT | EXCEPT } [ ALL | DISTINCT ] select ]`

    Combines the results of more than one `SELECT` query into a single query.

8) `[ ORDER BY expression [ ASC | DESC ] [, ...] ]`

    Sorts result by one or more expression.

9) `[ LIMIT [ count | ALL ] ]`

    Restricts the number of rows in result.

### Examples

```sql
// Gets crime from January 2018,
// Join with rental value by postcode
// Filter by postcode
// Order by crime period
// Limit to 10 results

SELECT crime.postcode, crime.period, rent.household_rental_value_1_bedroom
FROM doordastats_snapshot.doordastats_snapshot.eng_area_reported_crime crime
LEFT JOIN doordastats_snapshot.doordastats_snapshot.eng_household_rental_value rent
ON crime.postcode = rent.postcode
WHERE crime.period >= 201801 and crime.postcode = 'CA7 4DP'
ORDER BY crime.period
LIMIT 10;
```

### Misc

- Table Sample
    - `BERNOULLI`
        Each row is selected to be in the table sample with a probability of
        the sample percentage. When a table is sampled using the Bernoulli method,
        all physical blocks of the table are scanned and certain rows are skipped
        (based on a comparison between the sample percentage and a random value
        calculated at runtime).
        The probability of a row being included in the result is independent from
        any other row. This does not reduce the time required to read the sampled
        table from disk. It may have an impact on the total query time if the sampled
        output is processed further.

        QUERY FORMAT:
        ```sql
        SELECT *
        FROM table_name
        TABLESAMPLE BERNOULLI ({percent in integer});
        ```

        Table sample of `10%` of rows from `register_company_profile` in DoordaBiz:
        ```sql
        SELECT *
        FROM doordabiz_snapshot.doordabiz_snapshot.register_company_profile
        TABLESAMPLE BERNOULLI (10);
        ```


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
    DESCRIBE {catalog}.{schema}.{table};


## USE

Update session to use specific catalog and schema. If catalog is not specified, schema is resolved relative to current catalog.

    USE {catalog}.{schema};
    USE {schema};
    
    # Examples
    
    USE doordabiz_snapshot.doordabiz_snapshot;
    USE doordabiz_snapshot;



Back to [Tables of Content](../README.md#getting-started-guide-to-host)
