## 筛选数组-Filtering Arrays
### 问题
你想要根据布尔条件来筛选数组。
### 方案
使用Array.filter (ECMAScript 5)： array = [1..10]
```
array.filter (x) -> x > 5
# => [6,7,8,9,10]
```
在EC5之前的实现中，可以扩展Array的原型添加一个筛选函数，该函数接受一个回调并对自身进行过滤，将回调函数返回true的元素收集起来。
```
# 扩展Array的原型
Array::filter = (callback) ->
  element for element in this when callback(element)

array = [1..10]

# 筛选偶数
filtered_array = array.filter (x) -> x % 2 == 0
# => [2,4,6,8,10]

# 过滤掉小于或等于5的元素
gt_five = (x) -> x > 5
filtered_array = array.filter gt_five
# => [6,7,8,9,10]
```
### 讨论
这个方法与Ruby的Array#select方法类似。

