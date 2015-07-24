## 找到上一个月(或下一个月)-Finding Last (or Next) Month
### 问题
你需要计算相关日期范围例如“上一个月”，“下一个月”。
### 方案
添加或减去当月，JavaScript 日期的构造函数会修复数学知识。
```
# these examples were written in GMT-6
# Note that these examples WILL work in January!
now = new Date
# => "Sun, 08 May 2011 05:50:52 GMT"

lastMonthStart = new Date 1900+now.getYear(), now.getMonth()-1, 1
# => "Fri, 01 Apr 2011 06:00:00 GMT"

lastMonthEnd = new Date 1900+now.getYear(), now.getMonth(), 0
# => "Sat, 30 Apr 2011 06:00:00 GMT"
```
### 讨论
JavaScript 日期对象会处理下溢和溢出的月和日，并将相应调整日期对象。例如，你可以要求寻找三月的第42天，你将获得四月11日。  
JavaScript 对象存储日期为从1900开始的每年的年份数，月份为一个0到11的整数，日期为从1到31的一个整数。在上述解决方案中，上个月的起始日是要求在本年度某一个月的第一天，但月是从-1至10。如果月是- 1的日期对象将实际返回为前一年的十二月：  
```
lastNewYearsEve = new Date 1900+now.getYear(), -1, 31
# => "Fri, 31 Dec 2010 07:00:00 GMT"
The same is true for overflows:
thirtyNinthOfFourteember = new Date 1900+now.getYear(), 13, 39
# => "Sat, 10 Mar 2012 07:00:00 GMT"


