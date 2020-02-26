Clyps are Clypboard's mechanism for storing company-specific configuration information. 
When Clypboard loads, it will read the CLYP environment variable to determine which configuration to load (defaults to 'plunketts' if CLYP is not specified).

The configuration is read from two different JSON files:

```
config/clyps/<CLYP>.json
config/clyps/<CLYP>.<RAILS_ENV>.json
```

Keys from the second file are recursively merged with those from the first.

The name of the current clyp is available in Ruby and Javascript with:

```
CLYP # ruby
clyp.name() // javascript
```

The clyp configuration value are also available:

```
CLYP_CONFIG['key'] # ruby
clyp.config()['key'] // javascript
```

Public files specific to the clyp are served from:

```
public/clyps/<CLYP>/
```

## Features

The 'features' object in the clyp config determines which optional features are enabled for the clyp. 
There are convenience functions to make querying this object easier:

```
clyp_has_feature?('name') # ruby
clyp.hasFeature('name') // javascript
```

## Production Enabled?

There are situations - such as when a new company is being on-boarded - where you want to run the production environment but don't actually want the sharp edges (like processing credits cards or emailing clients). 
In this case, we have a special clyp config value called 'production' that must be set to 'enabled' for the real production bits to run.

So, instead of checking such cases with the Rails environment, there are helper functions which check both the Rails environment as well as the 'production' config:

```
is_production_enabled? # ruby
clyp.isProductionEnabled() // javascript
```

## Get Clyp Value

Because accessing the clyp for a value can create silent failures in the code a couple methods have been introduced to raise an error and let you know the value you're requesting isn't accessible. 

```
get_clyp_value?(path_str) # ruby - returns nil if value doesn't exist
get_clyp_value!(path_str) # ruby - raises error if value doesn't exist
clyp.getValue(pathStr) // javascript
```

the input variable is a string of the path you're trying to access.  It can be delimited by , / ; or white space.  Eg. get_clyp_value('time_entry production_rate holiday') is the same as get_clyp_value('time_entry/production_rate/holiday').  This method will raise an error in ruby and return null in javascript and log the error to the console.


