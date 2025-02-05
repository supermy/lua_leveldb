Lua LevelDB
===========

This is a FFI library, provide some simple interface.

## Requirements

* LuaJIT 

busted.runner

## APIs

__DB__

* LevelDB.new(name, options)
* LevelDB.destroy_db(name)
* LevelDB:get(key)
* LevelDB:set(key, val)
* LevelDB:del(key)
* LevelDB:batchSet({k1 = 'v', k2 = 'v2'})
* LevelDB:batchDel({'k1', 'k2'})
* LevelDB:newIteratorWith(options, function(iter))
* LevelDB:each(options, function(key, val, iter)) // return false to stop the each
* LevelDB:close()


__Iterator__

* Iterator.new(leveldb, options)
* Iterator:first()
* Iterator:last()
* Iterator:seek(key)
* Iterator:next()
* Iterator:prev()
* Iterator:read() // return key, value of current iterator location
* Iterator:destroy()


## Example

Example

```
LevelDB = require 'leveldb'
db = LevelDB.new('./tmp')

print('version: ' .. db.version .. "\n")

print('Get k1 => ' .. (db:get('k1') or "'nil'"))
print('Get k2 => ' .. (db:get('k2') or "'nil'"))
print('Get unset_key => ' .. (db:get('unset_key') or "'nil'"))
print("")

function print_db_data()
  print('Iterator all keys')
  db:each(nil, function(k, v) print(k, v) end)
  print("")
end

print("Set k3")
db:set('k3', tostring(os.time()))
print("")

print("Batch set k1 k2 k4 k5")
db:batchSet({k1 = tostring(os.time()), k2 = '321321', k4 = '111', k5 = '222'})
print("")

print_db_data()

print("Del k3")
db:del('k3')
print("")

print_db_data()

print("Batch del k4, k5")
db:batchDel({'k4', 'k5'})
print("")

print_db_data()

print('close')
db:close()

print('destory')
LevelDB.destroy_db('./tmp')

print('end')
```


## Test

```

luarocks --lua-dir=/usr/local/opt/luajit  list
luarocks --lua-dir=/usr/local/opt/luajit install busted
luarocks --lua-dir=/usr/local/opt/luajit install penlight
luarocks --lua-dir=/usr/local/opt/luajit install lua-term
luarocks --lua-dir=/usr/local/opt/luajit install luasystem
luarocks --lua-dir=/usr/local/opt/luajit install mediator_lua
luarocks --lua-dir=/usr/local/opt/luajit install lua_cliargs
luarocks --lua-dir=/usr/local/opt/luajit  install luassert

bin/busted
```
