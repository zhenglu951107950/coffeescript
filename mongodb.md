#MongoDB
  
##问题
  
你需要与一个 MongoDB 数据库连接的接口。
  
##解决方法
  
**对于 Node.js**
  
**安装**

- 如果你的计算机中还没有 MongoDB ，需要安装。
  
- 安装本地 MongoDB 模块。

**保存记录**
  
<pre><code>
mongo = require 'mongodb'

server = new mongo.Server "127.0.0.1", 27017, {}

client = new mongo.Db 'test', server, {w:1}

# save() updates existing records or inserts new ones as needed
exampleSave = (dbErr, collection) ->
    console.log "Unable to access database: #{dbErr}" if dbErr
    collection.save { _id: "my_favorite_latte", flavor: "honeysuckle" }, (err, docs) ->
        console.log "Unable to save record: #{err}" if err
        client.close()

client.open (err, database) ->
    client.collection 'coffeescript_example', exampleSave
</code></pre>  
  
**查找记录**
  
<pre><code>
mongo = require 'mongodb'

server = new mongo.Server "127.0.0.1", 27017, {}

client = new mongo.Db 'test', server, {w:1}

exampleFind = (dbErr, collection) ->
    console.log "Unable to access database: #{dbErr}" if dbErr
    collection.find({ _id: "my_favorite_latte" }).nextObject (err, result) ->
        if err
            console.log "Unable to find record: #{err}"
        else
            console.log result # => {  id: "my_favorite_latte", flavor: "honeysuckle" }
        client.close()

client.open (err, database) ->
    client.collection 'coffeescript_example', exampleFind
</code></pre>
  
**对于浏览器**
  
一个基于 REST 的接口在工程中，会提供基于 AJAX 的访问通道。
  
##讨论
  
这个方法将 save 和 find 分开进单独的实例，其目的是分散 MongoDB 指定的连接任务的关注点以及回收任务。该模块可以帮助这样的异步调用。

