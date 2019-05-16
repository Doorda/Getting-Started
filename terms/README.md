# Terms

## List of Products

### DoordaBiz
All tables can be joined by `company_number` column

1) Snapshots  
    Data at the point in time with each row identifying a unique entry.

    
    Example:  
    Company Profile table has a unique company per row of data.
    Each company can have their confirmation statement date updated or address updated for example.
    So the snapshot consists of the latest entry for each company on the fixed day of each month.
    
    Catalog = `doordabiz_snapshot`  
    Schema = `doordabiz_snapshot`


### DoordaStats
All tables can be joined by `postcode` column.

1) Snapshots  
    Data at the point in time with each row identifying a unique entry.
    
    Catalog = `doordastats_snapshot`  
    Schema = `doordastats_snapshot`


### DoordaProperty (Coming Soon)



Back to [Tables of Content](../README.md#getting-started-guide-to-host)