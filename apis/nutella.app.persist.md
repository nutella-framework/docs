# nutella.app.persist
These APIs are the same ones that we find at the [run-level](nutella.persist.md) but the locaiton of the files and the database where the data is persisted changes.

## Where is stuff stored?
- **mongo, collection**: stored as MongoDB collection named `name` inside the database `app_id`. 
- **mongo, object**: stored as MongoDB document with `_id` `name`, inside the collection `app_persisted_hashes`, inside the database `app_id`. 
- **file, collection**: stored as a file in `bot_dir/data/coll/name.json`
- **file, object**:  stored as a file in `bot_dir/data/obj/name.json`

