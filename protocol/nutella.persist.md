
# nutella.persist
This module deals with data storage on the bots side. It provides a set of APIs to simplify storing and retrieving data. Why we need a module for this? Because the data for each run need to stay separate. The APIs in this module provide a 'safe sandbox' that allow each run to only access their data. This way developers don't have to think about keeping data separate.

This module provides 4 APIs:
1. `nutella.persist.get_json_collection_store(name)`
2. `nutella.persist.get_json_object_store(name)`
3. `nutella.persist.get_mongo_collection_store(name)`
4. `nutella.persist.get_mongo_object_store(name)`

Each one of these methods returns a "store" which is automatically persisted to a `.json` file in the filesystem (for the first two APIs) or to MongoDB (for the last two). Also, the second and fourth methods provide APIs which are better suited for dealing with a single object that is updated/changed frequently while the first and third method provide APIs to add/remove relatively static objects to/from a collection.

The APIs offered by the "stores" are extremely idiomatic of the language so please refer to the language specific documentation.


## Where is stuff stored?
Depending on the type of "store" used, data is persisted with different methods and in different locations.

- **mongo, collection**: stored as MongoDB collection named `run_id/name` inside the database `app_id`.
- **mongo, object**: stored as MongoDB document with `_id` `run_id/name`, inside the collection `run_persisted_hashes`, inside the database `app_id`. 
- **file, collection**: stored as a file in `bot_dir/data/run_id/coll/name.json`
- **file, object**:  stored as a file in `bot_dir/data/run_id/obj/name.json`




