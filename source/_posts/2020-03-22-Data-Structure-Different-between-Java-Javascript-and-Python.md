---
title: 'Java, JavaScript, Python 常用数据结构总结'
categories:
  - Language
  - Algorithm
  - Data Structure
tags:
  - Java
  - JavaScript
  - Python
date: 2020-03-22 15:23:28
---

# 常用数据类型

| Java| JavaScript| Python
| --- | --------- | ------
| 整数类型：byte,short,int,long<br/>浮点数类型：float,double<br/>字符类型:char<br/>布尔类型:boolean | 数字(Number): 整数,小数<br/>任意精度整数(BigInt)：可处理大整数<br/>字符串(String)：一串字符序列<br/> 布尔值(Boolean):true / false<br/>undefined:未定义或不存在<br/>null:空值<br/>代表(Symbol):实例唯一且不可改变<br/>Object:狭义object,array,function | 数值：int,float,complex<br/>字符串：str（由Unicode构成的不可变序列）<br/>布尔值：True / False (它是int的子类型)<br/>空值：None<br/>序列类型：list,tuple<br/>集合类型：set,forzenset<br/>映射类型：dict

注意事项：

JavaScript的数值都是64位存储，在某些特定的运算，如位运算时，才会将整数自动转成32位来计算。

Python 中的除法，`//`才是其他大多数语言中的`/`


# 常用数据结构

| Java | JavaScript | Python |
| ---- | ---------- | ------ |
| 数组: DataType[]<br/>List: ArrayList, LinkedList<br/>Map: HashMap, LinkedHashMap, TreeMap<br/>Set: HashSet, LinkedHashSet, TreeSet<br/>Stack:Stack<br/>Queue: LinkedList(Deque), PriorityQueue | 数组: Array<br/>Map: Map<br/>Set: Set<br/>Stack: 无<br/>Queue: 无 | 序列类型：list,tuple<br/>集合类型：set,forzenset<br/>映射类型：dict<br/>Stack: queue.LifoQueue<br/>Queue: queue.Queue, queue.PriorityQueue, collections.deque

## 数组 Array

### Java

```
// 声明方式
// 方式1，按部就班
dataType[] arrayRefVar; //申明
arrayRefVar = new dataType[arraySize];  //创建
arrayRefVar[index] = value;//赋值

// 方式2，直接赋值
dataType[] arrayRefVar = new dataType[]{value0, value1, ..., valueN};  //创建

// 方式3，直接赋值的简单写法
dataType[] arrayRefVar = {value0, value1, ..., valueN};

// 遍历
for (dataType value : arrayRefVar) {} // foreach方法
for (int index = 0; index < arrayRefVar.length; index++) {} // 传统fori遍历

// 切片
dataType[] sliceFromStart = Arrays.copyOf(arrayRefVar, 2);
dataType[] sliceOfRange = Arrays.copyOfRange(arrayRefVar, 2, 4);

// 排序
Arrays.sort(arrayRefVar);

// 增删改查
// 由于Java中的数组长度不可变，所以没有提供相关的增删方法，只能通过其他方式实现
// 改
arrayRefVar[index] = newValue;  
// 查
Arrays.binarySearch() // 适用于有序数组
// 非有序数组只能遍历或者转List等数据结构用它们的contains()方法
```

### JavaScript

数组本质上是一种特殊的对象，可以参考[网道-JavaScript教程-数组](https://wangdoc.com/JavaScript/types/array.html)

```
// 声明方式
// 方式1，初始化给值
var arr = new Array(element0, element1, ..., elementN);
var arr = Array(element0, element1, ..., elementN);
var arr = [element0, element1, ..., elementN];

// 初始化不给值，后续填充
var emp = [];  //不像java在声明的时候必须指定大小
emp[0] = "Casey Jones";
emp[1] = "Phil Lesh";

// 与Java的区别，任何类型都可以放入数组
var arr = [
  {a: 1},
  [1, 2, 3],
  function() {return true;}
];

// 遍历操作
for (const value of arr) {} // 符合大家预期的for循环。遍历的是数组内的值，可以正常响应return,break,continue
arr.forEach((value) => { console.log(value) })  // foreach遍历，不能return,break,continue
for (let i = 0; i < arr.length; i++) {}  //fori循环，用let或var关键字，可以正常return,break,continue
for (const index in arr) () // 不要用这种方法。遍历的是索引；看起来可以，但实际上不能return、break、continue

// 切片
arr.slice(start);
arr.slice(start, end);

//排序
arr.sort();

// 增删改查
arr.push(newValue) // 增加到末尾
arr.pop()   // 弹出最后一个元素，改变长度

arr.unshift(newValue) // 增加到开头,index=0的位置
arr.shift() // 删除第一个元素，改变长度

delete arr[index] // 删除后当前位置变undefined，不改变长度

arr[index] = newValue // 改变index的值
arr.concat(newArr) // 拼接2个数组

arr.indexOf(target) //target所在index，不包含返回-1
arr.includes(target)   // 是否包含target，返回boolean

// splice万能方法
var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];
// 从索引2开始删除3个元素,然后再添加两个元素:
arr.splice(2, 3, 'Google', 'Facebook'); // 返回删除的元素 ['Yahoo', 'AOL', 'Excite']
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
// 只删除,不添加:
arr.splice(2, 2); // ['Google', 'Facebook']
arr; // ['Microsoft', 'Apple', 'Oracle']
// 只添加,不删除:
arr.splice(2, 0, 'Google', 'Facebook'); // 返回[],因为没有删除任何元素
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
```

### Python

Python通常用`元组`(tuple，不可变的集合)或`列表`(List，可变的集合)来达到其他语言数组的效果。他们的[通用序列操作](https://docs.python.org/zh-cn/3.7/library/stdtypes.html#common-sequence-operations)。

```
# 元组
theTuple = ("apple", "banana", "cherry")
specialTuple = (1,) # 后面要有逗号,避免定义的是数字1，也就是避免括号运算符与数学意义上的括号混淆
# 列表
fruits = ["apple", "banana", "cherry", "orange", "kiwi", "melon", "mango"]

# 遍历
for fruit in fruits:
    # for in 遍历
    print(fruit)

for index in range(0, len(fruits), 2):
    # 借助range(start, end[, step])遍历
    print(fruits[index])

# 切片操作
newTuple = theTuple[0:4:1] //起始，终点（不包含），步长

# 排序
fruits.sort()

# 增删改查
fruits.append('pear') # 只能添加1个元素，如果参数是['pear']，那么就会在末尾添加一个列表['pear']而不是元素'pear'
fruits.extend('pear')   # 末尾添加元素'pear'
fruits.extend(['pear']) # 末尾添加元素'pear'
fruits.extend(['pear','peach']) # 末尾添加元素'pear','peach'
fruits += ['pear', 'peach'] # 等同于fruits.extend()方法
fruits.insert(2,'peach') # 在index为2的位置插入'peach'，与append一样只能插入一个元素
del fruits[index]
del fruits[start:end:step]
fruits.remove('pear')   # 删除'pear'元素，如果没有会报错
fruits.pop([index]) # index默认为-1，即最后一个元素
'pear' in fruits # 检测是否包含元素
```

## 列表 List

### Java
  
```
ArrayList<Integer> list = new ArrayList<>();    // 内部以数组实现
list.add(100);
list.add(200);
list.get(0);
list.set(0, 150);   // index可能越界
list.remove(1);
list.remove(Integer.valueOf(150));  // 直接写remove(100)会当成index处理，这里需要一个Integer对象

LinkedList<Integer> linkedList = new LinkedList<>();    // 内部以链表实现
```

### JavaScript

同`Array`，见上一节`数组`。

### Python

同`list`。见上一节`数组`。

## 键值对 Map

### Java

```
// 内部以Node<K,V>[]数组存储数
// 声明
Map<String, String> map = new HashMap<>();

// 基本操作
map.put("a", "this is A");
map.put("a", "this is new A");
map.containsKey("a");           // true
map.containsValue("this is A"); // false
map.remove("b");                // return null
map.get("a");                   // return "this is new A"
map.size();                     // return 1

// 遍历
for (Map.Entry<String, String> item : map.entrySet()) { // 遍历键值对
    System.out.println(item.getKey());
    System.out.println(item.getValue());
}

for (String key : map.keySet()) {                       // 遍历键
    System.out.println(key);
}

for (String value : map.values()) {                     // 遍历值
    System.out.println(value);
}
```

### JavaScript

```
// 声明方法
let myMap = new Map();

let keyObj = {};
let keyFunc = function() {};
let keyString = 'a string';
 
// 添加、修改
myMap.set(keyString, "和键'a string'关联的值");
myMap.set(keyObj, "和键keyObj关联的值");
myMap.set(keyFunc, "和键keyFunc关联的值");
myMap.set(NaN, "not a number");   // NaN作为键的时候视为相同
 
myMap.size; // 4
 
// 读取值
myMap.get(keyString);    // "和键'a string'关联的值"
myMap.get(keyObj);       // "和键keyObj关联的值"
myMap.get(keyFunc);      // "和键keyFunc关联的值"
myMap.get(Number("foo"));// "not a number"
 
myMap.get('a string');   // "和键'a string'关联的值"，因为keyString === 'a string'
myMap.get({});           // undefined, 因为keyObj !== {}
myMap.get(function() {});// undefined, 因为keyFunc !== function () {}

// 遍历
for(let [key, value] of myMap) {
    console.log(`${key} => ${value}`)
}

for (let item of myMap) {
    console.log(Array.isArray(item)) // true，可以看出与上例本质一样
    console.log(`${item[0]} => ${item[1]}`)
}
```

更多详情请看[Mozilla关于Map的文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map)。

### Python

```
# Python中的Map就是dict
# 声明方法
a = dict(one=1, two=2, three=3)
b = {'one': 1, 'two': 2, 'three': 3}
c = dict(zip(['one', 'two', 'three'], [1, 2, 3]))
d = dict([('two', 2), ('one', 1), ('three', 3)])
e = dict({'three': 3, 'one': 1, 'two': 2})
# 上述方法等价，即 a == b == c == d == e 为 True

# 添加元素
a['a'] = 'A'
a['c'] = 'C'
# 修改元素
a['a'] = 'AA'
d.update({'a': 'AAA', 'b':'BBB'})  # 可以批量更新，key已经存在会被覆盖
# 读取元素
a['d']      # 读取错误，触发KeyError。这种方式需要通过 key in a 来判断是否存在对应key
a.get('d')  # 返回None，不会触发KeyError
# 删除元素
del a['a']  # 删除对应元素，如果不存在，会触发KeyError
a.clear()   # 删除所有

len(a)  // 获取长度

# 遍历
for key in list(d):         // 遍历所有key
    print(key)

for key in d.keys():        // 与上面的等价
    print(key)

for value in d.values():    // 遍历所有值
    print(value)
    
for key,value in d.items(): // 遍历所有键值对
    print(key + " => " + value)
```

更多详情请看[Python官方文档: 映射类型 dict](https://docs.python.org/zh-cn/3.7/library/stdtypes.html#mapping-types-dict)

## 集合 Set


集合间运算涉及到的一些名词，交集、并集应该好理解，相对补集、对称差可能不太好理解，这里附上一些资料：

* [集合 - 补集](https://zh.wikipedia.org/wiki/%E8%A1%A5%E9%9B%86)
* [集合 - 对称差](https://zh.wikipedia.org/wiki/%E5%AF%B9%E7%A7%B0%E5%B7%AE)

### Java

```
// HashSet内部实现是用的HashMap，其实就是把值当key放到HashMap里
Set<String> set = new HashSet<>();
set.add("A");
set.add("B");
set.add("C");
set.contains("C");  // false
set.remove("B"); // true
set.size();         // 2

// 遍历
// 方式1
for (String value : set) {
    System.out.println(value);
}

// 方式2
Iterator<String> it = set.iterator();
while (it.hasNext()) {
    System.out.println(it.next());
}

// 集合间运算
Set<String> set1 = new HashSet<>();
set1.add("A");
set1.add("B");
set1.add("C");
Set<String> set2 = new HashSet<>();
set2.add("D");
set2.add("B");
set2.add("C");
// 交集
set1.retainAll(set2); // set1 = "B","C"
// 并集
set1.addAll(set2);    // set1 = "A","B","C","D"
// 相对补集（差集）
set1.removeAll(set2); // set1 = "A"
```

### JavaScript

```
// 声明
let mySet = new Set();

// 添加元素
mySet.add(1); // Set [ 1 ]
mySet.add(5); // Set [ 1, 5 ]
mySet.add(5); // Set [ 1, 5 ]
mySet.add("some text"); // Set [ 1, 5, "some text" ]
let o = {a: 1, b: 2};
mySet.add(o);
mySet.add({a: 1, b: 2}); // o 指向的是不同的对象，所以没问题

// 检查元素
mySet.has(1); // true
mySet.has(3); // false
mySet.has(5);              // true
mySet.has(Math.sqrt(25));  // true
mySet.has("Some Text".toLowerCase()); // true
mySet.has(o); // true

// 删除元素
mySet.delete(5);  // true,  从set中移除5
mySet.has(5);     // false, 5已经被移除

// Set长度
mySet.size; // 4, 刚刚移除一个值

console.log(mySet); // Set {1, "some text", Object {a: 1, b: 2}, Object {a: 1, b: 2}}
// 遍历

// 按顺序输出：1, "some text", {"a": 1, "b": 2}, {"a": 1, "b": 2}
for (let item of mySet) console.log(item);

// 按顺序输出：1, "some text", {"a": 1, "b": 2}, {"a": 1, "b": 2} 
for (let item of mySet.keys()) console.log(item);
 
// 按顺序输出：1, "some text", {"a": 1, "b": 2}, {"a": 1, "b": 2} 
for (let item of mySet.values()) console.log(item);

// 按顺序输出：1, "some text", {"a": 1, "b": 2}, {"a": 1, "b": 2} 
//(键与值相等)
for (let [key, value] of mySet.entries()) console.log(key);

// 用forEach迭代
mySet.forEach(function(value) {
  console.log(value);
});
```
[Mozilla - Set相关文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set)

### Python

```
# 声明
set0 = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'} // {'apple', 'pear', 'banana', 'orange'}
set1 = set()
set2 = set('apple') // {'a', 'l', 'p', 'e'}
set3 = {x for x in 'abcd' if x not in 'cdef'} # 集合推导式,结果是{'a', 'b'}

# 添加元素
set3.add('c')  # 只能添加一个元素
# update 可以添加多个元素，也可以添加所有Iterable，例如list,tuple,set,dict,str
set3.update('d', 'e')
set3.update(['f', 'g'])
set3.update({'f': 'd', 'g': 'e'})   # 添加的是keys
set3.update({'f': 'd', 'g': 'e'}.values()) # 添加values

# 检查元素
'a' in set3 # True or False

# 删除元素
set3.remove('a')  # 移除对应元素，不存在会报错
set3.discard('a') # 与remove一样，但不会报KeyError错误
set3.pop()        # 随机删除一个元素
set3.clear()      # 清空

# 获取大小
len(set1)

# 两集合间的运算
set4 = set('abcd')
set5 = set('cde')
set4 - set5 # 相对补集（差集） {'a', 'b'}
set4 | set5 # 并集 {'d', 'c', 'e', 'a', 'b'}
set4 & set5 # 交集 {'d', 'c'}
set4 ^ set5 # 两个集合的对称差 {'e', 'b', 'a'}
```

## 栈 Stack

### Java

```
// 声明。继承自Vector。实际以数组存储数据。
Stack<Integer> stack = new Stack<>();

// 增加
stack.push(1);
stack.push(2);
stack.push(3);

// 检查顶部
stack.peek();

// 取出
stack.pop();

// 查找
stack.search(2);

// 判空
stack.empty();

// 长度
stack.size();
```

### JavaScript

```
// 使用第三方或者自行基于数组等实现
```

### Python

```
import queue

# 后进先出队列，可以当栈用
lifoQueue = queue.LifoQueue()
# 添加元素，相当于Java中的push()方法
lifoQueue.put(1)
lifoQueue.put(2)
lifoQueue.put(3)
# 获取元素，取元素前先判空，否则会block等待有元素加入，直到超时报错。或者也可以设置block为false，队列空就会直接报Empty错误
# 这里连续get可以获得3,2,1。相当于Java的pop()方法
lifoQueue.get()
# 获取队列大小（多线程时不可靠）
lifoQueue.qsize()
```

## 队列 Queue

### Java

```
// 普通的队列
var queue = new LinkedList<AbstractMap.SimpleEntry<Integer, Integer>>();
// 优先级队列。声明时需要设置对比器，比如这里用value排序
var priorityQueue = new PriorityQueue<AbstractMap.SimpleEntry<Integer, Integer>>(
    Comparator.comparingInt(AbstractMap.SimpleEntry::getValue));

// 添加元素
var item1 = new AbstractMap.SimpleEntry<>(1, 10);
var item2 = new AbstractMap.SimpleEntry<>(2, 8);
var item3 = new AbstractMap.SimpleEntry<>(3, 9);
priorityQueue.offer(item1);
priorityQueue.offer(item2);
priorityQueue.offer(item3);
queue.offer(item1);
queue.offer(item2);
queue.offer(item3);

// 获取元素,poll()和peek()
priorityQueue.poll();   // 此时poll()执行3次得到的item2,item3,item1
queue.poll();           // 此时poll()执行3次得到的item1,item2,item3
/*
普通队列默认是添加到队列后面，从前面取元素，即
peek() = peekFirst()
poll() = pollFirst()
offer() = offerLast()

由于LinkedList实现了Deque，支持双向列表，所以还支持这些操作：
peekLast(), pollLast(), offerFirst()等
 */
queue.peekLast();
```

### JavaScript

```
// 没有现成的，只能自定义或使用第三方
```

### Python

```
import queue

# 声明普通队列(先进先出)
fifoQueue = queue.Queue()
# 添加元素，相当于Java中的offer
fifoQueue.put(1)
fifoQueue.put(2)
fifoQueue.put(3)
# 获取元素，取元素前先判空，否则会block等待有元素加入，直到超时报错。或者也可以设置block为false，队列空就会直接报Empty错误
# 这里连续get可以获得1,2,3。相当于Java中的poll()
fifoQueue.get()
# 获取队列大小（多线程时不可靠）
fifoQueue.qsize()


# 双向队列。Deque 支持线程安全，内存高效添加(append)和弹出(pop)，
# 从两端都可以，两个方向的大概开销都是 O(1) 复杂度
from collections import deque
d = deque()
# 在右端增加元素
d.append(1)  # 1,
d.append(2)  # 1,2
# 在左端增加元素
d.appendleft(0)  # 0,1,2
# 在末尾弹出元素
d.pop()  # 0,1
# 在队首弹出元素
d.popleft()  # 1,


# 声明优先队列
priorityQueue = queue.PriorityQueue()
# 添加元素，添加的item必须是可以比较的，会以从小到大的顺序排列
priorityQueue.put(('a', 2))
priorityQueue.put(('a', 1))
priorityQueue.put(('b', 3))
priorityQueue.put(('c', 2))
# 获取元素
while priorityQueue.qsize() != 0:
    print(priorityQueue.get())  # 输出 ('a', 1) ('a', 2) ('b', 3) ('c', 2)
# 获取队列大小（多线程时不可靠）
priorityQueue.qsize()
```
