# JDBC Driver


## Class name
`io.prestosql.jdbc.PrestoDriver`

## Structure
The following JDBC URL formats are supported:

```text
jdbc:doordahost://host:port
jdbc:doordahost://host:port/catalog
jdbc:doordahost://host:port/catalog/schema
```

For example, use the following URL to connect to Presto running on example.net port 8080 with the catalog `doordastats_snapshot` 
and the schema `doordastats_snapshot`:

```text
jdbc:doordahost://host.doorda.com:443/doordastats_snapshot/doordastats_snapshot
```

## Parameters


| Name | Description           |
|----------|-----------------------|
| user        | Username to use for authentication and authorization.            |
|password        | Password to use for LDAP authentication.          |



Back to [List of Tools](README.md#list-of-supported-tools)