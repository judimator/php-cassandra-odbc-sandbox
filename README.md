# Install

## Build the images

```bash
$ docker build -t php-cli docker/php-cli
$ docker build -t cassandra docker/cassandra
```

## Run Cassandra container
```bash
$ docker run --name cassandra -d cassandra
```

## Run PHP Script
```bash
$ docker run -it -v <VOLUME> --link cassandra:cassandra --rm --name php-cli php-cli php <PHP_SCRIPT>
```

## PHP Script Example
```php
$conn = odbc_connect('Cassandra', '', '');

$stmt = odbc_prepare($conn, 'SELECT * FROM system_schema.keyspaces;');
$success = odbc_execute($stmt);

$data = [];
while($row = odbc_fetch_array($stmt)){
    $data[]= $row;
}

print_r($data);
```
