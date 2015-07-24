# 查找子字符串

## 问题

你需要搜索一个字符串，并返回匹配的起始位置或匹配值本身。

## 方案

有几种使用正则表达式的方法来实现这个功能。其中一些方法被称为 RegExp 模式或对象还有一些方法被称为 String 对象。

### RegExp 对象

第一种方式是在 RegExp 模式或对象中调用 test 方法。test 方法返回一个布尔值：
```
match = /sample/.test("Sample text")
# => false

match = /sample/i.test("Sample text")
# => true
```

下一种方式是在 RegExp 模式或对象中调用 exec 方法。exec 方法返回一个匹配信息的数组或空值:
```
match = /s(amp)le/i.exec "Sample text"
# => [ 'Sample', 'amp', index: 0, input: 'Sample text' ]

match = /s(amp)le/.exec "Sample text"
# => null
```

### String 对象

match 方法使给定的字符串与表达式对象匹配。有“g”标识的返回一个包含匹配项的数组，没有“g”标识的仅返回第一个匹配项或如果没有找到匹配项则返回 null。
```
"Watch out for the rock!".match(/r?or?/g)
# => [ 'o', 'or', 'ro' ]

"Watch out for the rock!".match(/r?or?/)
# => [ 'o', index: 6, input: 'Watch out for the rock!' ]

"Watch out for the rock!".match(/ror/)
# => null
```

search 方法以字符串匹配正则表达式，且如果找到的话返回匹配的起始位置，未找到的话则返回-1。
```
"Watch out for the rock!".search /for/
# => 10

"Watch out for the rock!".search /rof/
# => -1
```

## 讨论

正则表达式是一种可用来测试和匹配子字符串的强大的方法。













