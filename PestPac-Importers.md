To import data from PestPac to Clypboard, we run an instance of SQL Server on the target production server and load the PestPac data from a backup. This data is queried directly by a set of importers in Clypboard to stream the data, convert it to Clypboard's format, and persist it to the Clypboard database. The PestPac database credentials are stored in the "import" object in the clyp configuration, although connecting to it is handled automatically by the importers.

## Importer Classes

Each importer should be a Ruby class (stored in /app/importers) that inherits from BaseImporter. The importer should implement a specific set of methods depending on the source of the data.

### PestPac Importers

Importers that import data directly from the PestPac database should implement these two methods:

`build_pestpac_query(builder, options)` - builds the query (using `builder`) using the provided options hash, which is provided when the importer is called (see Import Tasks sections).

`load_pestpac_record(data, options)` - this method gets called once for each record that is retrieved from the query built with `build_pestpac_query`. 
