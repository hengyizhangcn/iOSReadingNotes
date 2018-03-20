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

12.NS_REQUIRES_NIL_TERMINATION

必须以nil结尾，如：

```
+ (instancetype)arrayWithObjects:(ObjectType)firstObj, ... NS_REQUIRES_NIL_TERMINATION;
```

13.NS_RETURNS_INNER_POINTER

返回纯C语言的指针变量

14.NS_RETURNS_RETAINED

NS_RETURNS_NOT_RETAINED

15.NS_NOESCAPE

```
允许编译器在block内优化代码。
```



# 关键词

###### 属性关键字(或lifetime qualifiers，即终身限定词)

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

其它关键字有：

1.class

2.retain和strong是同义词

3.assign和weak很相似，如下：

>```
>@property(assign) MyClass *myObject;
>@property(weak) MyClass *myObject; 
>//区别是当myObject被释放时，属性值会被设置为nil; 而assign所指的属性会变成悬浮指针(dangling pointer)或野指针
>//assign可以修饰oc或非oc对象，而weak只能修饰oc对象
>```

###### 变量限定符

__strong 默认

__weak 声明一个引用但不保持对象生命，当对象没有强引用的时候这个弱引用就被设置为nil了

__unsafe_unretained，声明一个引用但不保持对象生命，当对象没有强引用的时候它不会被设置为nil，如果引用的对象被释放的话，指针就变成悬浮的了

__autoreleasing

在对象声明中使用限定符的正确格式如下：

```
ClassName * qualifier variableName;
如：
MyClass * __weak myWeakReference;
MyClass * __unsafe_unretained myUnsafeReference;
```



###### 消除循环引用 

1.打破block的循环引用的两种方法

a.使用__weak

b.把__block的值置为nil，如下：

```
MyViewController * __block myController = [[MyViewController alloc] init…];
// ...
myController.completionHandler =  ^(NSInteger result) {
    [myController dismissViewControllerAnimated:YES completion:nil];
    myController = nil;
};
```

###### 无损桥接（Toll-free bridging）

如果需要在OC和CoreFoundation类型对象间进行强转，你需要告诉编译器对象的所有权

__bridge 在OC和CoreFoundation间传递指针，但不发生所有的权的转移

__bridge_retained 或者 CFBridgingRetain 会把OC指针强转成CF指针，而且向你转移所有权；你有责任去调用CFRelease或者相关的函数去释放所有权

__bridge_transfer或者CFBridgingRelease 会把非OC指针转成OC并且向ARC传递所有权（你不用再关心对象的释放，由ARC去负责）

注：不能直接在id和void *之间进行转换

>For declared properties, you should use `assign` instead of `weak`; for variables you should use `__unsafe_unretained` instead of `__weak`.



# 数据类型

容器类对应有可变类型的，有NSString, NSData, NSData, NSDictionary等10个，数据类型的另种表现形式如下：

1.__NSCFConstantString，代表不可变/常量字符串(包括空字符串也用此表示)

__NSCFString，代表可变字符串

2.__NSCFNumber

3._NSZeroData，代表不可变数据，并且为空

NSConcreteMutableData，代表不可变或可变数据

4.__NSSingleEntryDictionaryI，代表不可变字典，且字典只有一个键值对

__NSDictionaryI，代表不可变字典，有两个及以上键值对

__NSDictionaryM，代表可变字典

__NSDictionary0，代表空字典

5.__NSSingleObjectArrayI，代表不可变数组，且数组只有一个元素

__NSArrayI，代表不可变数组，有两个及以上元素

__NSArrayM，代表可变数组

__NSArray0，代表空数组

6.__NSSingleObjectSetI，代表不可变集合只有一个元素

__NSSetI，代表不可变集合，为空或有两个及以上元素

__NSSetM，代表可变集合

7.__NSOrderedSetI，代表不可变有序集合

__NSOrderedSetM，代表可变有序集合

8._NSCachedIndexSet，代表不可变索引集合

NSMutableIndexSet，代表可变索引集合

9.NSURLRequest，代表不可变url请求

NSMutableURLRequest，代表可变url请求

10.__NSCFCharacterSet，不可变/可变字符集合

11.NSConcreteAttributedString，不可变属性字符串

NSConcreteMutableAttributedString，可变属性字符串

iOS可变对象有哪些：

```
NSMutableArray,
NSMutableDictionary,
NSMutableSet,
NSMutableString,
NSMutableOrderedSet, //有序集合，元素不可重复
NSMutableIndexSet,
NSMutableData,
NSMutableURLRequest,
NSMutableCharacterSet,
NSMutableAttributedString,
#NSMutableFontCollection为AppKit的类
#NSMutableCoping为协议
```



# 异常有哪些

1.NSInvalidArgumentException

无效参数异常

2.NSParseErrorException

解析错误，（如NSString的propertyList可能会出现）

3.NSRangeException

范围越界错误



# 类型知多少

1.unichar 是两个字节长的char，代表unicode的一个字符。

# 类型检查建议

1.对于类，isKindOfClass:

2.对于消息，respondsToSelector:

3.对于协议一致性，conformsToProtocol:

# 数据转换

1.NSString -> NSData

```
-(NSData *)dataUsingEncoding:(NSStringEncoding)encoding allowLossyConversion:(BOOL)lossy;
-(NSData *)dataUsingEncoding:(NSStringEncoding)encoding;
```

NSString -> NSArray

```
- (NSArray<NSString *> *)componentsSeparatedByString:(NSString *)separator;
- (NSArray<NSString *> *)componentsSeparatedByCharactersInSet:(NSCharacterSet *)separator API_AVAILABLE(macos(10.5), ios(2.0), watchos(2.0), tvos(9.0));//在集合里面的字符都可作为分隔符号
其中第二种形式，需要先把NSString转化为NSCharacterSet，如
NSCharacterSet *characterSet = [NSCharacterSet characterSetWithCharactersInString:@"&@*"];
```

NSString->NSDictionary

这里的NSString必须是jsonString，做法是先把jsonString转成NSData，再转成NSDictionary，用NSJsonSerialization类方法，如下：

```
+ (nullable NSData *)dataWithJSONObject:(id)obj options:(NSJSONWritingOptions)opt error:(NSError **)error;
+ (nullable id)JSONObjectWithData:(NSData *)data options:(NSJSONReadingOptions)opt error:(NSError **)error;
```

2.NSArray -> NSString

```
- (NSString *)componentsJoinedByString:(NSString *)separator;
```

NSArray->NSData，可通过NSKeyedArchiver类方法，如下：

```
NSArray *tables = @[@"12", @"34", @"45"];
NSData *tablesData = [NSKeyedArchiver archivedDataWithRootObject:tables];
```

3.NSData->NSString

需使用NSString的对象方法，即

```
- (nullable instancetype)initWithData:(NSData *)data encoding:(NSStringEncoding)encoding;
```

NSData->NSDictionary，通过NSJsonSerialization类，如下:

```
+ (nullable id)JSONObjectWithData:(NSData *)data options:(NSJSONReadingOptions)opt error:(NSError **)error;
```

NSData->NSArray，可通过NSKeyedUnarchiver类，如下：

```
NSArray *unArchiveredTables = [NSKeyedUnarchiver unarchiveObjectWithData:tablesData];
```

4.NSDictionary->NSData

通过NSJsonSerialization类，如下:

```
+ (nullable NSData *)dataWithJSONObject:(id)obj options:(NSJSONWritingOptions)opt error:(NSError **)error;
```

NSDictionary->NSString

可以先把NSDictionary转成NSData，再由NSData转换成NSString，如:

```
NSDictionary *dict = @{@"name":@"zhy"};
NSData *nameData = [NSJSONSerialization dataWithJSONObject:dict options:NSJSONReadingMutableLeaves error:nil];
NSString *nameStr = [[NSString alloc] initWithData:nameData encoding:NSUTF8StringEncoding];
```

