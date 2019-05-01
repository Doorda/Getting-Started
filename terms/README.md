# Terms

## List of Products

### DoordaBiz
All tables can be joined by `company_number` column

1) Snapshots  
    Data at the point in time. 
    
    Example:  
    Company profile table has a unique company per row of data. 
    Each company can have their confirmation statement date update or address updated for example. 
    So the snapshot consists of the latest entry for each company on the 1st day of each month. 
    
    Catalog = `doordabiz_snapshot`  
    Schema = `doordabiz_snapshot`

### DoordaStats
All tables can be joined by `postcode` column.

1) Snapshots  
    Data at the point in time. 
    
    Catalog = `doordastats_snapshot`  
    Schema = `doordastats_snapshot`


### DoordaProperty (Coming Soon)



Back to [Tables of Content](../README.md#getting-started-guide-to-host)