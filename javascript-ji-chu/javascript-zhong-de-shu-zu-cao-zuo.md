---
description: 在日常的业务中很多的时候会涉及到数组的处理，所有这里针对js的数组处理进行一个总结。
---

# javascript 中的数组操作

## 数组中包含哪些操作

```text
Array
ƒ Array() { [native code] }
Array.prototype
[constructor: ƒ, concat: ƒ, copyWithin: ƒ, fill: ƒ, find: ƒ, …]
length: 0
constructor: ƒ Array()
concat: ƒ concat()
copyWithin: ƒ copyWithin()
fill: ƒ fill()
find: ƒ find()
findIndex: ƒ findIndex()
lastIndexOf: ƒ lastIndexOf()
pop: ƒ pop()
*push: ƒ push()
reverse: ƒ reverse()
shift: ƒ shift()
unshift: ƒ unshift()
slice: ƒ slice()
sort: ƒ sort()
splice: ƒ splice()
*includes: ƒ includes()
indexOf: ƒ indexOf()
join: ƒ join()
keys: ƒ keys()
entries: ƒ entries()
values: ƒ values()
*forEach: ƒ forEach()
*filter: ƒ filter()
flat: ƒ flat()
flatMap: ƒ flatMap()
*map: ƒ map()
*every: ƒ every()
some: ƒ some()
reduce: ƒ reduce()
reduceRight: ƒ reduceRight()
toLocaleString: ƒ toLocaleString()
toString: ƒ toString()
Symbol(Symbol.iterator): ƒ values()
Symbol(Symbol.unscopables): {copyWithin: true, entries: true, fill: true, find: true, findIndex: true, …}
__proto__: Object
```

可以看到数组的方法非常的多，但是我不打算在这里逐个介绍，这里我只挑选几个我用的比较多的方法进行介绍，分别是：push、includes、forEach、filter、map、every。

## includes

这个方法非常好用，主要用于判断数组中是否存在某个元素，这里的判断是对元素的内存地址进行判断，所以对于对象数组来说，可操作性不是很高，但是对于字符串和数组的可操作性就比较好了。

```text
let list = ['1', 1, 2, 3]
undefined
list.includes(1)
true
```

## push

这个方法也比较常用，主要是将新增的对象添加到数组当中，这里我们需要注意一下这个方法的返回值，这个方法的返回值是添加到数组数据的位置，但是新添加对象的位置对于我们来说也不是特别的重要，我们只是要将数据添加到数组当中。

## forEach、filter、map

准备好数组数据

```text
class User {
    constructor(name, age, score) {
        this.name = name
        this.age = age
        this.score = score
    }
}

let userList = [new User('小明', 16, 99), new User('天明', 16, 88), new User('明明',18, 60)]
```

### forEach

forEach 方法比较适合在处理数组相关的数据时使用，但是处理的数据不是数组当中的数据，而是数组之外的数据，但是与数组中的数据存在关联，注意forEach方法没有返回值，这也是我为什么不推荐使用forEach处理数组数据的原因：

```text
userList.forEach((user, index, arr) => {
    console.dir(user);
    console.dir(index);
    console.dir(arr);
    user['test'] = 'test'
    // 在此处处理相关的页面比如说获取各个用户的详细信息
})
```

### filter

filter 方法看方法的名称就知道是对数组的数据进行筛选，需要注意的是该方法是有返回值的，返回值为删选后的数据



