# Python

## Description

Wrapper for `PyHive` with simplified functions.

## Installation


### PyPi

```bash
$ pip install doorda-sdk
```


### Source
#### Download from:
1) https://github.com/Doorda/doorda-python-sdk/releases

```bash
wget https://github.com/Doorda/doorda-python-sdk/archive/1.0.4.zip

unzip 1.0.4.zip

```

#### Install
```bash
python setup.py install
```

## Usage

### DoordaHost

1) Connect to database
    ```python
    from doorda_sdk.host import client

    conn = client.connect(user="username",
                          password="password",
                          catalog="catalog_name",
                          schema="schema_name")
    cursor = conn.cursor()
    ```

2) Execute Queries
    ```python
    cursor.execute("SELECT * FROM table_name")
    
    # Fetch all results
    rows = cursor.fetchall()
    
    # Fetch one results
    rows = cursor.fetchone()
    
    # Fetch multiple results
    rows = cursor.fetchmany(size=10)
    
    # Get list of column names
    cursor.col_names()
    
    # Get column names mapped to data types
    cursor.col_types()
    ```

3) Simplified Functions

    ```python
    # Check database connection
    results = cursor.is_connected()
    
    # List all catalogs
    rows = cursor.show_catalogs()
    
    # Get number of rows
    rows = cursor.table_stats(catalog="catalog_name", 
                              schema="schema_name",
                              table="table_name")
    ```


Back to [List of Tools](README.md#list-of-supported-tools)