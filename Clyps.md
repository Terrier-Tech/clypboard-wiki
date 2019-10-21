## Overview

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
