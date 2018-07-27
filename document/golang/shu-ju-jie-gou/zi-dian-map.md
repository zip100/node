> map 存储的是 键值对\(key-value\)。是一个无序的数据的集合，通过键来进行索引得到对应的值。 这种方式可以加快查找速度。Map 通常称为 字典（dictionary） 或者哈希表\(Hash table\)。Map 现在是很多语言的标配。

* **声明方式**

```go
var mapName map[keyType]valueType
```

1. **不需要给字典指定长度，字典的长度会在初始化或者创建的过程中动态增长**
2. **Key 必须是能支持 比较运算符（==, !=）的数据类型，比如 整数，浮点数，指针，数组，结构体，接口等。 而不能是 函数，字典，切片这些类型。**
3. **Value 类型 可以是Go语言的任何基本数据类型。 **

```go
var map1 map[string]int
map1["key1"] = 2                 //编译不通过，字典没有初始化或者创建


var map1 map[string]int {}       //编译通过，字典的初始化
map1["key1"] = 1

var map2 map[string]int
map2 = make(map[string]int)      //字典的创建
map2["key2"] = 2                 //使用 等号 添加数据项
```



* ##### 字典元素的查找

```
v, err := mapName[Key]
```

* ##### 字典元素的删除

```
delete(mapName, "key")
```



