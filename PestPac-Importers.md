To import data from PestPac to Clypboard, we run an instance of SQL Server on the target production server and load the PestPac data from a backup. This data is queried directly by a set of importers in Clypboard to stream the data, convert it to Clypboard's format, and persist it to the Clypboard database. The PestPac database credentials are stored in the "import" object in the clyp configuration, although connecting to it is handled automatically by the importers.

## Importer Classes

Each importer should be a Ruby class (stored in /app/importers) that inherits from BaseImporter. All PestPac importers must implement this method:

```ruby
load_pestpac_record(data, options)
```
This method gets called once for each record that is being imported. It should return the Clypboard object that's imported if successful, or nil if it's not. Exceptions should only be raised if the importer encounters an error that needs immediate fixing - as opposed to a malformed PestPac record.

Importers that import data directly from the PestPac database should also implement:

```ruby
build_pestpac_query(builder, options)
```
Builds the query (using the SqlBuilder instance `builder`) with the provided options hash, which is provided when the importer is called (see Import Tasks sections).

Instead of importing directly from the PestPac database, some importers read data from a local JSON or YAML file. These only need to implement `load_pestpac_record`.


