# 宏定义

1.NS_DESIGNATED_INITIALIZER 用来指定构造器。告诉调用者要用这个方法去初始化（构造）类对象。

2.NS_UNAVAILABLE 不想让调用者调用父类的初始化函数，只希望通过该类指定的初始化函数进行初始化，如

```
- (instancetype)init NS_UNAVAILABLE;
```

如果调用者使用init 初始化，编译器就会给出一个编译错误。使用NS_UNAVAILABLE后，就不需要在.m中重写父类初始化函数了。如果要允许调用者使用init就需要在.m中重写父类的初始化函数，如上提到的，否则就会报警告。

http://blog.csdn.net/zcube/article/details/51657417

3.NS_ASSUME_NONNULL_BEGIN

4.NS_ASSUME_NONNULL_END

在NS_ASSUME_NONNULL_BEGIN和NS_ASSUME_NONNULL_END两个宏之间的代码，所有简单指针对象都被假定为NONNULL，我们只需指定那个NULLABLE的指针。

5.NS_ENUM_AVAILABLE_IOS

代表枚举从某一个版本开始，如NS_ENUM_AVAILABLE_IOS(2_0)代表>=2.0

6.NS_ENUM_DEPRECATED_IOS

代表已经过时的API，如NS_ENUM_DEPRECATED_IOS(2,7)代表始于2弃于7

7.__TVOS_PROHIBITED

在TVOS系统上不能使用

8.UIKIT_EXTERN

extern定义字符串变量，比#define更高效；而UIKIT_EXTERN是根据是否是C语言宏定义，根据语言区分，比extern更加的高效。如：

```
UIKIT_EXTERN NSString *const UIApplicationInvalidInterfaceOrientationException NS_AVAILABLE_IOS(6_0) __TVOS_PROHIBITED;
#代表iOS6以后可用，tvOS不可用
```

上面代码一般在.h中定义，在.m中实现.

9.NS_CLASS_AVAILABLE_IOS

与NS_ENUM_AVAILABLE_IOS类似

10.NS_EXTENSION_UNAVAILABLE_IOS

标记iOS插件不能使用该API，后台添加提示，如：

```
+ (UIApplication *)sharedApplication NS_EXTENSION_UNAVAILABLE_IOS("Use view controller based solutions where appropriate instead.");
```

11.NS_REQUIRES_SUPER

要求子类重写父类方法必须先调用父类的这个方法。你烦方法中加NS_REQUIRES_SUPER，子类重写才有警告提示。如：

```
- (void)prepare NS_REQUIRES_SUPER;
```



# 关键词

###### 属性关键字

普通类型属性默认关键字 atomic, readwrite, assign

OC对象属性默认关键字 atomic, readwrite, strong



Xcode6.3 之后出现

__nullable 指对象可以为NULL或NIL

__nonnull 指对象不能为NULL

以上两个关键字仅限于指针类型上，如

```
- (id)labelWithTitle:(NSString *_nonnull)title
```

方法中的声明也可以使用不带下划线的nullable和nonnull，如

```
- (nullable id)labelWithTitle:(NSString *nonnull)title
```

属性声明中也可以使用nullable和nonnull关键字，如

```
@property (nonatomic, copy, nonnull) NSArray * items;(推荐使用)
或
@property (nonatomic, copy) NSArray * __nonnull items;
```

https://www.jianshu.com/p/b3a31eed945f