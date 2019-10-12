# nutella.f.persist
These APIs are the same ones that we find at the [run-level](nutella.persist.md) but the locaiton of the files and the database where the data is persisted changes.

## Where is stuff stored?
- **mongo, collection**: stored as MongoDB collection named `name` inside the database `nutella`. 
- **mongo, object**: stored as MongoDB document with `_id` `name`, inside the collection `fr_persisted_hashes`, inside the database `nutella`. 
- **file, collection**: stored as a file in `~/.nutella/data/bot_name/coll/name.json`
- **file, object**:  stored as a file in `~/.nutella/data/bot_name/obj/name.json`
