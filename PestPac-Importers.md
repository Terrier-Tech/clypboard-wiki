To import data from PestPac to Clypboard, we run an instance of SQL Server on the target production server and load the PestPac data from a backup. This data is queried directly by a set of importers in Clypboard to stream the data, convert it to Clypboard's format, and persist it to the Clypboard database. The PestPac database credentials are stored in the "import" object in the clyp configuration, although connecting to it is handled automatically by the importers.

## Importer Classes

Each importer should be a Ruby class (stored in `/app/importers`) that inherits from BaseImporter. All PestPac importers must implement this method:

`load_pestpac_record(data, options)` - This method gets called once for each record that is being imported. It should return the Clypboard object that's imported if successful, or nil if it's not. Exceptions should only be raised if the importer encounters an error that needs immediate fixing - as opposed to a malformed PestPac record.

Importers that import data directly from the PestPac database should also implement:

`build_pestpac_query(builder, options)` - Builds the query (using the SqlBuilder instance `builder`) with the provided options hash, which is provided when the importer is called (see Importer Tasks sections).

Instead of importing directly from the PestPac database, some importers read data from a local JSON, CSV, or YAML file. These only need to implement `load_pestpac_record`.

`BaseImporter` includes `Loggable`, so all logging tasks should be done using either `debug`, `info`, or `warn` instead of `puts`.

## Importer Tasks

All tasks that import base data (i.e. data that doesn't generally change like pests, chemicals, equipment, etc.) should be in `base_data.rake`. All tasks that import from PestPac should be in `import.rake`. 

Tasks for importers that import using a PestPac building should call the `load_pestpac_builder` method, passing in options for the builder. For example: 

```ruby
task serviceorders: :environment do
  importer = WorkOrderImporter.new
  importer.load_pestpac_builder table: 'serviceorders'
end
```

Tasks that import from local files should either call `load_local_csv`, `load_local_json`, or `load_local_yaml`, passing the name of the file to load and (optional) options hash. The files are read from `/import/<CLYP>`. For example:

```ruby
task load_employees: :environment do
  importer = EmployeeImporter.new
  importer.load_local_csv 'employees', force: true # reads /import/lloyd/employees.csv
end
```

The log level of importers defaults to 'info', However, it can be forced to 'debug' with the DEBUG environment variable:

```
DEBUG=true rake import:serviceorders
```



