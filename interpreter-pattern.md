
#解释器模式
  
##问题
  
其他人需要以控制方式运行你的一部分代码。相对地，你选择的语言不能以一种简洁的方式表达问题域。
  
##解决方法
  
使用解释器模式来创建一个你翻译为特定代码的领域特异性语言（ domain-specific language ）。
  
我们来做个假设，例如用户希望在你的应用程序中执行数学运算。你可以让他们正向运行代码来演算指令（eval）但这会让他们运行任意代码。相反，你可以提供一个小型的“堆栈计算器（stack calculator）”语言，用来做单独分析，以便只运行数学运算，同时报告更有用的错误信息。
  
<pre><code>
class StackCalculator
    parseString: (string) ->
        @stack = [ ]
        for token in string.split /\s+/
            @parseToken token

        if @stack.length > 1
            throw "Not enough operators: numbers left over"
        else
            @stack[0]

    parseToken: (token, lastNumber) ->
        if isNaN parseFloat(token) # Assume that anything other than a number is an operator
            @parseOperator token
        else
            @stack.push parseFloat(token)

    parseOperator: (operator) ->
        if @stack.length < 2
            throw "Can't operate on a stack without at least 2 items"

        right = @stack.pop()
        left = @stack.pop()

        result = switch operator
            when "+" then left + right
            when "-" then left - right
            when "*" then left * right
            when "/"
                if right is 0
                    throw "Can't divide by 0"
                else
                    left / right
            else
                throw "Unrecognized operator: #{operator}"

        @stack.push result

calc = new StackCalculator

calc.parseString "5 5 +" # => { result: 10 }

calc.parseString "4.0 5.5 +" # => { result: 9.5 }

calc.parseString "5 5 + 5 5 + *" # => { result: 100 }

try
    calc.parseString "5 0 /"
catch error
    error # => "Can't divide by 0"

try
    calc.parseString "5 -"
catch error
    error # => "Can't operate on a stack without at least 2 items"

try
    calc.parseString "5 5 5 -"
catch error
    error # => "Not enough operators: numbers left over"

try
    calc.parseString "5 5 5 foo"
catch error
    error # => "Unrecognized operator: foo"
</code></pre>
  
##讨论
  
作为一种替代编写我们自己的解释器的选择，你可以将现有的 CoffeeScript 解释器与更自然的（更容易理解的）表达自己的算法的正常方式相结合。
  
<pre><code>
class Sandwich
    constructor: (@customer, @bread='white', @toppings=[], @toasted=false)->

white = (sw) ->
    sw.bread = 'white'
    sw

wheat = (sw) ->
    sw.bread = 'wheat'
    sw

turkey = (sw) ->
    sw.toppings.push 'turkey'
    sw

ham = (sw) ->
    sw.toppings.push 'ham'
    sw

swiss = (sw) ->
    sw.toppings.push 'swiss'
    sw

mayo = (sw) ->
    sw.toppings.push 'mayo'
    sw

toasted = (sw) ->
    sw.toasted = true
    sw

sandwich = (customer) ->
    new Sandwich customer

to = (customer) ->
    customer

send = (sw) ->
    toastedState = sw.toasted and 'a toasted' or 'an untoasted'

    toppingState = ''
    if sw.toppings.length > 0
        if sw.toppings.length > 1
            toppingState = " with #{sw.toppings[0..sw.toppings.length-2].join ', '} and #{sw.toppings[sw.toppings.length-1]}"
        else
            toppingState = " with #{sw.toppings[0]}"
    "#{sw.customer} requested #{toastedState}, #{sw.bread} bread sandwich#{toppingState}"

send sandwich to 'Charlie' # => "Charlie requested an untoasted, white bread sandwich"
send turkey sandwich to 'Judy' # => "Judy requested an untoasted, white bread sandwich with turkey"
send toasted ham turkey sandwich to 'Rachel' # => "Rachel requested a toasted, white bread sandwich with turkey and ham"
send toasted turkey ham swiss sandwich to 'Matt' # => "Matt requested a toasted, white bread sandwich with swiss, ham and turkey"
</code></pre>
  
这个实例可以允许功能层实现返回修改后的对象，从而外函数可以依次修改它。示例通过借用动词和介词的用法，把自然语法提供给结构，当被正确使用时，会像自然语句一样结束。这样，利用 CoffeeScript 语言技能和你现有的语言技能可以帮助你关于捕捉代码的问题。
