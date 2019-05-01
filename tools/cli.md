# CLI


## Requirements

Jave 8


## Download CLI

CLI [here](https://github.com/Doorda/drivers-cli/releases/download/309d/doordahost.jar)


## Usage

### Unix
```bash
./doordahost.jar \
--server https://host.doorda.com \
--user user1 \
--password \
--catalog doordastats-snapshot \
--schema doordastats-snapshot
```

### Windows

Open Powershell
```
java -jar c:\..\doordahost.jar \
--server https://host.doorda.com \
--user user1 \
--password \
--catalog doordastats-snapshot \
--schema doordastats-snapshot

```

### Export table to file (UNIX)

Modify `username`, `PRESTO_PASSWORD`, `catalog`, `schema`, `query_string`, `output_format` and `output_location`

```bash
cat <<EOT >> test_export.sh
#!/bin/bash

export PRESTO_PASSWORD={password}

./doordahost \ // Change path to location of cli
--server https://host.doorda.com \
--user {username}
--password \
--catalog {catalog} \
--schema {schema}
--execute "{query_string}" \
--output-format {output_format} > {output_location}
EOT
```
**Output Options**
1) CSV(without header)
2) TSV (without header)
3) CSV_HEADER (with header)
4) TSV_HEADER (with header)
5) ALIGNED
6) VERTICAL


Back to [List of Tools](README.md#list-of-supported-tools)
