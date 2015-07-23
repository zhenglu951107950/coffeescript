#单件模式
  
##问题
  
许多时候你只想要一个，只要一个类的实例。比如，你可能需要一个创建服务器资源的类，并且你想要保证一个对象就可以控制这些资源。但是使用时要小心，因为单件模式可以很容易被滥用来模拟不必要的全局变量。
  
##解决方法
  
公有类只包含获得一个实例的方法。实例被保存在该公共对象的闭包中，并且总是有返回值。
  
这很奏效因为 CoffeeScript 允许你在一个类的声明中定义可执行的状态。但是，因为大多数 CoffeeScript 编译成一个 IIFE 包，如果这个方式适合你，你就不需要在类的声明中放置私有的类。之后的内容可能对开发模块化代码有所帮助，例如 CommonJS（Node.js）或 Require.js 中可见（见实例讨论）。
  
<pre><code>
class Singleton
  # You can add statements inside the class definition
  # which helps establish private scope (due to closures)
  # instance is defined as null to force correct scope
  instance = null
  # Create a private class that we can initialize however
  # defined inside this scope to force the use of the
  # singleton class.
  class PrivateClass
    constructor: (@message) ->
    echo: -> @message
  # This is a static method used to either retrieve the
  # instance or create a new one.
  @get: (message) ->
    instance ?= new PrivateClass(message)

a = Singleton.get "Hello A"
a.echo() # => "Hello A"

b = Singleton.get "Hello B"
b.echo() # => "Hello A"

Singleton.instance # => undefined
a.instance # => undefined
Singleton.PrivateClass # => undefined
</code></pre>
  
##讨论
  
通过上面的实例我们可以看到，所有的实例是如何从同一个 Singleton 类的实例中输出的。你也可以看到，私有类和实例变量都无法在 Singleton class 外被访问到。 Singleton class 的本质是提供一个静态方法得到只返回一个私有类的实例。它也对外界也隐藏私有类，因此你无法创建一个自己的私有类。
  
隐藏或使私有类在内部运作的想法是更受偏爱的。尤其是由于缺省的 CoffeeScript 将编译的代码封装在自己的 IIFE（闭包）中，你可以定义类而无须担心会被文件外部访问到。在这个实例中，注意，我用惯用的模块导出特点来强调模块中可被公共访问的部分。（请看“导出到全局命名空间”中对此理解更深入的讨论）。
  
<pre><code>
root = exports ? this

# Create a private class that we can initialize however
# defined inside the wrapper scope.
class ProtectedClass
  constructor: (@message) ->
  echo: -> @message

class Singleton
  # You can add statements inside the class definition
  # which helps establish private scope (due to closures)
  # instance is defined as null to force correct scope
  instance = null
  # This is a static method used to either retrieve the
  # instance or create a new one.
  @get: (message) ->
    instance ?= new ProtectedClass(message)

# Export Singleton as a module
root.Singleton = Singleton
</code></pre>
  
我们可以注意到 coffeescript 是如此简单地实现这个设计模式。为了更好地参考和讨论 JavaScript 的实现，请看初学者必备 JavaScript 设计模式。
