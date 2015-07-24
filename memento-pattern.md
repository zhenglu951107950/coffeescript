#备忘录模式
  
##问题
  
你想预测对一个对象做出改变后的反应。
  
##解决方法
  
使用备忘录模式（Memento Pattern）来跟踪一个对象的变化。使用这个模式的类会输出一个存储在其他地方的备忘录对象。
  
如果你的应用程序可以让用户编辑文本文件，例如，他们可能想要撤销上一个动作。你可以在用户改变文件之前保存文件现有的状态，然后回滚到上一个位置。
  
<pre><code>
class PreserveableText
    class Memento
        constructor: (@text) ->

    constructor: (@text) ->
    save: (newText) ->
        memento = new Memento @text
        @text = newText
        memento
    restore: (memento) ->
        @text = memento.text

pt = new PreserveableText "The original string"
pt.text # => "The original string"

memento = pt.save "A new string"
pt.text # => "A new string"

pt.save "Yet another string"
pt.text # => "Yet another string"

pt.restore memento
pt.text # => "The original string"
</code></pre>
 
##讨论
  
备忘录对象由 PreserveableText#save 返回，为了安全保护，分别地存储着重要的状态信息。你可以序列化备忘录以便来保证硬盘中的“撤销”缓冲或者是那些被编辑的图片等数据密集型对象。
