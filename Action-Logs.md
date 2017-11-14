The action logging is now a two-step process:
1. When a record is created, updated, or deleted, an action log entry is made into the local database - like it's always been. 
1. A background task periodically sends these records to our cloud datastore. 

**All read operations are performed from the cloud**, so it's possible you may not see a log right after an operation occurs. The task runs every minute and syncs 200 logs at a time, so it's generally no more than a minute behind. This obviously goes out the window for things like invoicing, but even then it should be no more than a day behind. 


