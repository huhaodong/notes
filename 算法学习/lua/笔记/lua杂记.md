# lua语法杂记

## 数据

1.nil表示空值无效值

2.table类型实际是一个关联数组，可以通过a={}来创建一个空的table。table中可以做数组使用a[1]=1,其中的下标是从1开始的；table也可以当作python中的字典来使用，如：a={key=val}。需要销毁table时直接将其指定为nil即可（a=nil），lua的垃圾回收会将其内存释放掉。遍历talbe可以使用函数ipairs()取游标，进行遍历。
```lua

for k,v in ipairs(a):
    print(k,v)

```

3.table.insert(table_x,[position,]value),函数用于向table_x中的position位置插入值value，如果不指定position则会在table的末尾插入值value。

## 模块/包

1.包中的内容通过table返回：

```lua

-- 文件名为 module.lua
-- 定义一个名为 module 的模块
module = {}
 
-- 定义一个常量
module.constant = "这是一个常量"
 
-- 定义一个函数
function module.func1()
    io.write("这是一个公有函数！\n")
end
 
local function func2()
    print("这是一个私有函数！")
end
 
function module.func3()
    func2()
end
 
return module

```

## 类

1.类的定义也是基于table的，将类成员变量与成员函数都放入table中：

```lua

-- Meta class
Rectangle = {area = 0, length = 0, breadth = 0}

-- 派生类的方法 new
function Rectangle:new (o,length,breadth)
  o = o or {}
  setmetatable(o, self)
  self.__index = self
  self.length = length or 0
  self.breadth = breadth or 0
  self.area = length*breadth;
  return o
end

-- 派生类的方法 printArea
function Rectangle:printArea ()
  print("矩形面积为 ",self.area)
end

```