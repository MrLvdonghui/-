## Topics
### Initializing a Class (初始化一个类)
* class func initialize()
> Initializes the class before it receives its first message
> 类收到第一次收到消息时，调用
* class func load()
> Invoked whenever a class or category is added to the Objective-C runtime; implement this method to perform class-specific behavior upon loading
> 在类或者类扩展文件被程序装载时调用。

#### initialize
1. `open class func initialize()`：class关键字意外着initialize属于类型方法，并且可以被子类覆盖
2. 子类第一次收到消息时，父类也会收到相应的消息，若父类也是第一次收到消息，父类同样也会调用initialize()方法。这个其实不重要，知道就行。
3. 当子类没有实现initialize(),则会调用父类initialize()，可以使用在父类中判断是否是父类指针，然后调用初始化代码:
##### objective-c: 
4. initialize 方法是block（堵塞）方法，如果在此方法调用其他类方法，其他方法也在调用此方法，会造成死锁。所以initialize方法只适合类本地初始化
```c
+ (void)initialize {
  if (self == [ClassName self]) {
    // ... do the initialization ...
  }
}
`

> 使用场景:  1. 初始化类静态常量，比如 .M文件中 `static NSArray someArr; `+ (void)initialize {someArr = [NSArray array] }`；2、 Swift当中Load方法不可用，可代替Load方法做相关初始化操作，如方法Swizzling

##### swift: 
```swift
override open class func initialize() {
        if self.classForCoder() == ClassName.classForCoder() {
            // ... do the initialization ...
        }
    }
```

#### load
> 几乎和Load一样，只是调用顺序不一致，Load()在类或者类扩展文件被程序装载时调用，initialize()在类收到第一次收到消息时调用
> Load中一般用来做在类扩展文件中做：方法Swizzling，关联属性等面向切面编程有关操作。__注意:Swift中Load方法不可用，可用initialize()代替，不过initialize（）在未来的Swift版本中，也将启用，现在版本为SWIFT3__

#### Discardable Content Proxy Support
* autoContentAccessingProxy
> 暂时理解，获取receiver继承NSDiscardableContent协议的代理对象。 相关知识点： NSDiscardableContent,NSProxy,NSCache

#### Sending Messages
* 





