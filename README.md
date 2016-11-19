lua-rocksdb
=
Lua binding for RocksDB

Installation
===
* Install RocksDB (make shared_lib, make static_lib), download from: https://github.com/facebook/rocksdb
* Update the Makefile if needed to point at the correct include/lib paths

Example
===

```lua
  local rocksdb = require("rocksdb")
  local options = rocksdb.options({
    create_if_missing = true
  })
  local db = rocksdb.open(options, "/tmp/test.rocksdb")
  local writeoptions = rocksdb.writeoptions()
  local readoptions = rocksdb.readoptions()
  db:put(writeoptions, "key", "value")
  print(db:get(readoptions, "key"))
  db:close()
```

Doc
===

rocksdb.options()
=====
Creates an options object, the options are passed as a table.
Currently only basic types (bool, int, uint64) types are supported.

rocksdb.readoptions()
=====
Creates a readoptions object.

rocksdb.writeoptions()
=====
Create a writeoptions object.

rocksdb.open(options, db_path)
=====
  * **options** - object (created using rocksdb.options())
  * **db_path** - string. path
  * returns *rocksdb* object (referenced below as **db**)

db:put(writeoptions, key, value)
=====
Execute a put command, key-value (both of string type).

  * **writeoptions** - write options object (created using rocksdb.writeoptions())
  * **key** - string
  * **value** - string

rocksdb:get(readoptions, key)
=====
Gets a value from the DB.

  * **readoptions** - read options object (created using rocksdb.readoptions())
  * **key** - string

rocksdb:write(writeoptions, writebatch)
=====
Executes _write_ operation on a batch.

  * **writeoptions** - created using rocksdb.writeoptions()
  * **writebatch** - created using rocksdb.writebatch()


rocksdb:close()
=====
Closes the instance (it doesn't destroy the DB!)

rocksdb.backup_engine(options, backup_path)
=====
Creates a backup engine object.

  * **options** options object (created using rocksdb.options())
  * **backup_path** - string - backup path
  * returns *backup_engine* object

backup_engine:create_new_backup(db)
=====
Creates a back from *db*.

  * **db** db object (rocksdb.open())
  * returns true on success, error on failure

backup_engine:purge_old_backups(number)
=====
Purge *number* of old backups.

backup_engine:get_backup_info_count()
=====
Returns the number of the available backups.

backup_engine:get_backup_info(index)
=====
Returns info of a backup at *index* (**note** indices start at 1, to keep with Lua 1-based array).
  * returns **table**

_keys_
  * **id** backup id
  * **timestamp**
  * **size**
  * **number_files**

backup_engine:close()
=====
Close/free the backup_engine instance

rocksdb.writebatch()
=====
Creates a *writebatch* instance.

writebatch:put(key, value)
=====
Puts a *key* with its *value* into the batch (both are strings).

writebatch:count()
=====
Returns the *count* of key-values pairs in the batch.

writebatch:clear()
=====
Clears the writebatch.

writebatch:destroy()
=====
Frees the writebatch instance.

