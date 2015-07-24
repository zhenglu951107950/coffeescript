#策略模式
  
##问题
  
解决问题的方式有多种，但是你需要在程序运行时选择（或是转换）这些方法。
  
##解决方法
  
在策略对象（Strategy objects）中封装你的算法。
  
例如，给定一个未排序的列表，我们可以在不同情况下改变排序算法。
  
**基类：**
<pre><code>
StringSorter = (algorithm) ->
    sort: (list) -> algorithm list
</code></pre>
  
**策略：**
<pre><code>
bubbleSort = (list) ->
    anySwaps = false
    swapPass = ->
        for r in [0..list.length-2]
            if list[r] > list[r+1]
                anySwaps = true
                [list[r], list[r+1]] = [list[r+1], list[r]]

    swapPass()
    while anySwaps
        anySwaps = false
        swapPass()
    list

reverseBubbleSort = (list) ->
    anySwaps = false
    swapPass = ->
        for r in [list.length-1..1]
            if list[r] < list[r-1]
                anySwaps = true
                [list[r], list[r-1]] = [list[r-1], list[r]]

    swapPass()
    while anySwaps
        anySwaps = false
        swapPass()
    list
</code></pre>
  
**使用策略：**
<pre><code>
sorter = new StringSorter bubbleSort

unsortedList = ['e', 'b', 'd', 'c', 'x', 'a']

sorter.sort unsortedList

# => ['a', 'b', 'c', 'd', 'e', 'x']

unsortedList.push 'w'

# => ['a', 'b', 'c', 'd', 'e', 'x', 'w']

sorter.algorithm = reverseBubbleSort

sorter.sort unsortedList

# => ['a', 'b', 'c', 'd', 'e', 'w', 'x']
</code></pre>
  
##讨论

“没有作战计划在第一次接触敌人时便能存活下来。”用户如是，但是我们可以运用从变化的情况中获得的知识来做出适应改变。在示例末尾，例如，数组中的最新项是乱序排列的，知道了这个细节，我们便可以通过切换算法来加速排序，只要简单地重赋值就可以了。
  
###练习
  

- 将 StringSorter 扩展为 AlwaysSortedArray 类来实现规则序列的所有功能，但是要基于插入方法自动分类新的项（例如 push 对比 shift）。

