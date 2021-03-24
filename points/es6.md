### 1. symbol
> 表示独一无二的值, 防止属性名的冲突。

### 2. Set Map
- Set: 类数组，成员不重复，
- WeakSet: WeakSet 的成员只能是对象, WeakSet 中的对象都是弱引用(弱引用：垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中。)
- Map: 类对象，key值不限于字符串，“值-值”对应。
- WeakMap: 没有遍历操作，不能取到键名，二是无法清空。

### es6 module 与 common module 区别
- CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。
- CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。
- CommonJS 模块的require()是同步加载模块，ES6 模块的import命令是异步加载。