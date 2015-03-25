
# nutella.persist
This module of the protocol deals with storage data storage for bots while `nutella.store` helps designers of interfaces deal with their data.

Support files natively but has mongo extension

## Where is stuff stored?

JSON - Hash => File
JSON - Coll => File
Mong - Hash => item
Mong - Coll => coll

Mongo-backed persistence
- RUN COLLECTIONS: stored as mongo collection inside DB:app_id. Collection name: run_id/name
- APP COLLECTIONS: stored as mongo collection inside DB:app_id. Collection name: name
- FRA COLLECTIONS: stored as mongo collection inside DB:nutella. Collection name: name

- RUN OBJECTS: stored as mongo docs inside coll:run_persisted_hashes, inside DB:app_id. id:run_id/name
- APP OBJECTS: stored as mongo docs inside coll:app_persisted_hashes, inside DB:app_id. id: name
- FRA OBJECTS: stored as mongo docs inside coll:fr_persisted_hashes, inside DB:nutella. id: name


JSON-files-backed persistence
- RUN COLL/OBJ: file is bot_dir/data/run_id/name.json
- APP COLL/OBJ: file is bot_dir/data/name.json
- FRA COLL/OBJ: file is in ~/.nutella/data/bot_name/name.json

