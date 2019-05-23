# Terminologies

## Table Types

Dependent on the source some of our datasets will have two variants. **Snapshot**, most recent view of the data, and **Ledger** list of transactions up to the snapshot view. Snapshots are best used to view `Current` status and Ledgers are best used to track historical changes.

Currently, only DoordaBiz product has a Ledger offering. 

### DoordaBiz
DoordaBiz contains information on companies and associated appointments. All tables can be joined by `company_number` column

1) **Snapshots**

    Data at the point in time with each row identifying a unique entry.

    
    Example:  
    Company Profile table has a unique company per row of data.
    Each company can have their confirmation statement date updated or address updated for example.
    So the snapshot consists of the latest entry for each company on the fixed day of each month.
    
    Catalog = `doordabiz_snapshot`  
    Schema = `doordabiz_snapshot`  
    Query Format: `SELECT {col} FROM doordabiz_snapshot.doordabiz_snapshot.{table_name}`

2) **Ledgers**

    Ledgers are records of transactions for each unique entry. Similar to the function of an accounting ledger, it is meant to maintain a verifiable, immutable history of the changes of each tables over time.
    
    Each unique entry will have an `insert` action into the ledger followed subsequently by `update`/`delete`
    (deletes are often not done unless it is an error made by the source provider)

    All tables in Snapshots will have an equivalent Ledger table to track changes.
    (Currently the Ledger for ICO Register is not available, as the data is no longer published)

    Example:

    In the Company Profile table, an incorporation will be shown as a `insert` and
    subsequent change of address, status, sic codes etc will be shown as `update`.


    Key Columns:

    - `values`

        Given as a key value pair and shows how the Snapshot entry looks at the indicated date.
        Key maps to column names of the Snapshot table.

    - `urn`

        Fixed unique row identifier of Snapshot

    - `action`

        Description of data manipulation type. Can be insert/update/delete

    - `change_date`

        Same as `change_date` column in snapshot tables.
        Refers to the date that the entry was added/amended by the source provider.

    - `date_added`

        Date the entry was added into the Ledger.
        Typically the date after the source provider release the data.

    - `orders`

        The order that the entry was amended, if multiple amendments were made to an entry on the same day.

        For example, if `ABC LIMITED` filed for a change of address and change of company name on 2019-01-01,
        the `date_added` will be the same for both entries in the ledger but the change of address wil have `orders=1`
        and change_of_name will have `orders=2`, depending on which entry was filed with the source provider first.


    **Use Case Query Examples**:

    1) Return how the Company Profile snapshot entry will look for company 09231049 on 2019-01-01

    ```sql
   SELECT urn, date_added, change_date, orders, action, "values"
    FROM (SELECT *, rank() over (partition by company_number order by date_added desc, change_date desc, orders desc) as rnk
            FROM register_company_profile_ledger
            WHERE company_number = '09231049' and date_added <= date '2019-01-01') as iq
    WHERE (rnk = 1 or rand() < 0);
    ```


    2) Recreate Snapshot for Company Profile table on 2019-03-01

        **Warning**:

        Recreating the Snapshot from the Ledger is a process intensive task.
        Hence large tables like the Company Filings (157 million rows), Company Profile (12 million rows)
        may take a some time to process.

    ```sql
    SELECT A."values"
    FROM register_company_profile_ledger as A
    INNER JOIN (SELECT urn, max(date_added) AS date_added,
                max_by(change_date, date_added) AS change_date,
                max_by(orders, date_added) AS orders,
                max_by(action, date_added) AS action
                FROM register_company_profile_ledger
                WHERE date_added <= date '2019-03-01' GROUP BY 1) as B
    ON A.urn = B.urn
    AND A.date_added = B.date_added
    AND A.orders = B.orders
    AND A.change_date = B.change_date
    AND A.action = B.action
    ```

    Catalog = `doordabiz_ledger`  
    Schema = `doordabiz_ledger`  
    Query Format:  `SELECT {col} FROM doordabiz_ledger.doordabiz_ledger.{table_name}`




### DoordaStats
DoordaStats contains location based data. All tables can be joined by `postcode` column.

1) Snapshots  
    Data at the point in time with each row identifying a unique entry.
    
    Catalog = `doordastats_snapshot`  
    Schema = `doordastats_snapshot`  
    Query Format:  `SELECT {col} FROM doordastats_snapshot.doordastats_snapshot.{table_name}`





Back to [Tables of Content](../README.md#getting-started-guide-to-host)
