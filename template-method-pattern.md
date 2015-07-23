#模板方法模式
  
##问题
  
定义一个算法的结构，作为一系列的高层次的步骤，使每一个步骤的行为可以指定，使属于一个族的算法都具有相同的结构但是有不同的行为。
  
##解决方法
  
使用模板方法（ Template Method ）在父类中描述算法的结构，再授权一个或多个具体子类来具体地进行实现。
  
例如，想象你希望模拟各种类型的文件的生成，并且每个文件要包含一个标题和正文。
  
<pre><code>
class Document
    produceDocument: ->
        @produceHeader()
        @produceBody()

    produceHeader: ->
    produceBody: ->

class DocWithHeader extends Document
    produceHeader: ->
        console.log "Producing header for DocWithHeader"

    produceBody: ->
        console.log "Producing body for DocWithHeader"

class DocWithoutHeader extends Document
    produceBody: ->
        console.log "Producing body for DocWithoutHeader"

docs = [new DocWithHeader, new DocWithoutHeader]
doc.produceDocument() for doc in docs
</code></pre>
  
##讨论
  
在这个实例中，算法用两个步骤来描述文件的生成：其一是产生文件的标题，另一步是生成文件的正文。父类中是实现每一个步骤的空的方法，多态性使得每一个具体的子类可以通过重写一步步的方法来实现对方法不同的利用。在本实例中，DocWithHeader 实现了正文和标题的步骤， DocWithoutHeader 只是实现了正文的步骤。
  
不同类型文件的生成就是简单的将文档对象存储在一个数组中，简单的遍历每个文档对象并调用其 produceDocument 方法的问题。
