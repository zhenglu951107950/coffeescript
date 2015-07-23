#SQLite
  
##问题
  
你需要 Node.js 内部与 SQLite 数据库连接的接口。
  
##解决方法
  
使用 SQLite 模块。
  
<pre><code>
sqlite = require 'sqlite'

db = new sqlite.Database

# The module uses asynchronous methods,
# so we chain the calls the db.execute
exampleCreate = ->
    db.execute "CREATE TABLE snacks (name TEXT(25), flavor TEXT(25))",
        (exeErr, rows) ->
            throw exeErr if exeErr
            exampleInsert()

exampleInsert = ->
    db.execute "INSERT INTO snacks (name, flavor) VALUES ($name, $flavor)",
        { $name: "Potato Chips", $flavor: "BBQ" },
        (exeErr, rows) ->
            throw exeErr if exeErr
            exampleSelect()

exampleSelect = ->
    db.execute "SELECT name, flavor FROM snacks",
        (exeErr, rows) ->
            throw exeErr if exeErr
            console.log rows[0] # => { name: 'Potato Chips', flavor: 'BBQ' }

# :memory: creates a DB in RAM
# You can supply a filepath (like './example.sqlite') to create/open one on disk
db.open ":memory:", (openErr) ->
    throw openErr if openErr
    exampleCreate()
</code></pre>
  
##讨论
  
你也可以提前准备你的 SQL 查询语句。
  
<pre><code>
sqlite = require 'sqlite'
async = require 'async' # Not required but added to make the example more concise

db = new sqlite.Database

createSQL = "CREATE TABLE drinks (name TEXT(25), price NUM)"

insertSQL = "INSERT INTO drinks (name, price) VALUES (?, ?)"

selectSQL = "SELECT name, price FROM drinks WHERE price < ?"

create = (onFinish) ->
    db.execute createSQL, (exeErr) ->
        throw exeErr if exeErr
        onFinish()
    
prepareInsert = (name, price, onFinish) ->
    db.prepare insertSQL, (prepErr, statement) ->
        statement.bindArray [name, price], (bindErr) ->
            statement.fetchAll (fetchErr, rows) -> # Called so that it executes the insert
                onFinish()

prepareSelect = (onFinish) ->
    db.prepare selectSQL, (prepErr, statement) ->
        statement.bindArray [1.00], (bindErr) ->
            statement.fetchAll (fetchErr, rows) ->
                console.log rows[0] # => { name: "Mia's Root Beer", price: 0.75 }
                onFinish()

db.open ":memory:", (openErr) ->
    async.series([
        (onFinish) -> create onFinish,
        (onFinish) -> prepareInsert "LunaSqueeze", 7.95, onFinish,
        (onFinish) -> prepareInsert "Viking Sparkling Grog", 4.00, onFinish,
        (onFinish) -> prepareInsert "Mia's Root Beer", 0.75, onFinish,
        (onFinish) -> prepareSelect onFinish
    ])
</code></pre>
  
SQL 的 SQLite 版本的以及 node-SQLite 模块文档提供了更完整的信息。
