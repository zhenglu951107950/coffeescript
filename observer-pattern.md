#观察者模式
  
##问题
  
当一个事件发生时你不得不向一些对象发布公告。
  
##解决方法
  
使用观察者模式（Observer Pattern）。
  
<pre><code>
class PostOffice
    constructor: () ->
        @subscribers = []
    notifyNewItemReleased: (item) ->
        subscriber.callback(item) for subscriber in @subscribers when subscriber.item is item
    subscribe: (to, onNewItemReleased) ->
        @subscribers.push {'item':to, 'callback':onNewItemReleased}

class MagazineSubscriber
    onNewMagazine: (item) ->
        alert "I've got new "+item

class NewspaperSubscriber
    onNewNewspaper: (item) ->
        alert "I've got new "+item

postOffice = new PostOffice()
sub1 = new MagazineSubscriber()
sub2 = new NewspaperSubscriber()
postOffice.subscribe "Mens Health", sub1.onNewMagazine
postOffice.subscribe "Times", sub2.onNewNewspaper
postOffice.notifyNewItemReleased "Times"
postOffice.notifyNewItemReleased "Mens Health"
</code></pre>
  
##讨论
  
这里你有一个观察者对象（ PostOffice ）和可观察对象( MagazineSubscriber, NewspaperSubscriber ）。为了通报发布新的周期性可观察对象的事件，应该对 PostOffice 进行订阅。每一个被订阅的对象都存储在 PostOffice 的内部订阅数组中。当新的实体周期发布时每一个订阅者都会收到通知。
