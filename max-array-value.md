## 数组最大值-Max Array Value
### 问题
你需要找出数组中包含的最大的值。
### 方案
你可以使用JavaScript实现，在列表推导基础上使用Math.max()：
```
Math.max [12, 32, 11, 67, 1, 3]... 
# => 67
```
另一种方法，在ECMAScript 5中，可以使用Array的reduce方法，它与旧的JavaScript实现向后兼容。
```
# ECMAScript 5 
[12,32,11,67,1,3].reduce (a,b) -> Math.max a, b 
# => 67
```
### 讨论
Math.max在这里比较两个数值，返回其中较大的一个。省略号 (...) 将每个数组价值转化为给函数的参数。您还可以使用它与其他带可变数量的参数进行讨论，如执行 console.log 。


