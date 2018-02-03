1.NSNotFound，代表查找不到，最大的NSInteger值

如以下四个方法都有可能查找不到，

```
- (NSUInteger)indexOfObject:(ObjectType)anObject;
- (NSUInteger)indexOfObject:(ObjectType)anObject inRange:(NSRange)range;
- (NSUInteger)indexOfObjectIdenticalTo:(ObjectType)anObject;
- (NSUInteger)indexOfObjectIdenticalTo:(ObjectType)anObject inRange:(NSRange)range;
```

2.会崩溃的方法

```
- (NSArray<ObjectType> *)arrayByAddingObject:(ObjectType)anObject; //如果添加的对象anObject为nil，则此方法会抛出NSInvalidArgument异常




+ (instancetype)arrayWithObject:(ObjectType)anObject; //当anObject为nil时会崩溃
+ (instancetype)arrayWithObjects:(const ObjectType Nonnull [Nonnull])objects count:(NSUInteger)cnt;//当cnt超过objects的总数时，会崩溃

```

3.如果数组为空，以下两个方法获取的值为nil

```
@property (nullable, nonatomic, readonly) ObjectType firstObject API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));
@property (nullable, nonatomic, readonly) ObjectType lastObject;
```





@interface NSArray<__covariant ObjectType> : NSObject <NSCopying, NSMutableCopying, NSSecureCoding, NSFastEnumeration>

\- (instancetype)init NS_DESIGNATED_INITIALIZER;

\- (instancetype)initWithObjects:(const ObjectType _Nonnull [_Nullable])objects count:(NSUInteger)cnt NS_DESIGNATED_INITIALIZER;

\- (nullable instancetype)initWithCoder:(NSCoder *)aDecoder NS_DESIGNATED_INITIALIZER;

@end

@interface NSArray<ObjectType> (NSExtendedArray)

\- (NSString *)descriptionWithLocale:(nullable id)locale;

\- (NSString *)descriptionWithLocale:(nullable id)locale indent:(NSUInteger)level;

\- (nullable ObjectType)firstObjectCommonWithArray:(NSArray<ObjectType> *)otherArray;

\- (void)getObjects:(ObjectType _Nonnull __unsafe_unretained [_Nonnull])objects range:(NSRange)range NS_SWIFT_UNAVAILABLE("Use 'subarrayWithRange()' instead");

\- (NSEnumerator<ObjectType> *)objectEnumerator;

\- (NSEnumerator<ObjectType> *)reverseObjectEnumerator;

@property (readonly, copy) NSData *sortedArrayHint;

\- (NSArray<ObjectType> *)sortedArrayUsingFunction:(NSInteger (NS_NOESCAPE *)(ObjectType, ObjectType, void * _Nullable))comparator context:(nullable void *)context;

\- (NSArray<ObjectType> *)sortedArrayUsingFunction:(NSInteger (NS_NOESCAPE *)(ObjectType, ObjectType, void * _Nullable))comparator context:(nullable void *)context hint:(nullable NSData *)hint;

\- (NSArray<ObjectType> *)sortedArrayUsingSelector:(SEL)comparator;

\- (NSArray<ObjectType> *)subarrayWithRange:(NSRange)range;

/* Serializes this instance to the specified URL in the NSPropertyList format (using NSPropertyListXMLFormat_v1_0). For other formats use NSPropertyListSerialization directly. */

\- (BOOL)writeToURL:(NSURL *)url error:(NSError **)error API_AVAILABLE(macos(10.13), ios(11.0), watchos(4.0), tvos(11.0));

\- (void)makeObjectsPerformSelector:(SEL)aSelector NS_SWIFT_UNAVAILABLE("Use enumerateObjectsUsingBlock: or a for loop instead");

\- (void)makeObjectsPerformSelector:(SEL)aSelector withObject:(nullable id)argument NS_SWIFT_UNAVAILABLE("Use enumerateObjectsUsingBlock: or a for loop instead");

\- (NSArray<ObjectType> *)objectsAtIndexes:(NSIndexSet *)indexes;

\- (ObjectType)objectAtIndexedSubscript:(NSUInteger)idx API_AVAILABLE(macos(10.8), ios(6.0), watchos(2.0), tvos(9.0));

\- (void)enumerateObjectsUsingBlock:(void (NS_NOESCAPE ^)(ObjectType obj, NSUInteger idx, BOOL *stop))block API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (void)enumerateObjectsWithOptions:(NSEnumerationOptions)opts usingBlock:(void (NS_NOESCAPE ^)(ObjectType obj, NSUInteger idx, BOOL *stop))block API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (void)enumerateObjectsAtIndexes:(NSIndexSet *)s options:(NSEnumerationOptions)opts usingBlock:(void (NS_NOESCAPE ^)(ObjectType obj, NSUInteger idx, BOOL *stop))block API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (NSUInteger)indexOfObjectPassingTest:(BOOL (NS_NOESCAPE ^)(ObjectType obj, NSUInteger idx, BOOL *stop))predicate API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (NSUInteger)indexOfObjectWithOptions:(NSEnumerationOptions)opts passingTest:(BOOL (NS_NOESCAPE ^)(ObjectType obj, NSUInteger idx, BOOL *stop))predicate API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (NSUInteger)indexOfObjectAtIndexes:(NSIndexSet *)s options:(NSEnumerationOptions)opts passingTest:(BOOL (NS_NOESCAPE^)(ObjectType obj, NSUInteger idx, BOOL *stop))predicate API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (NSIndexSet *)indexesOfObjectsPassingTest:(BOOL (NS_NOESCAPE ^)(ObjectType obj, NSUInteger idx, BOOL *stop))predicate API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (NSIndexSet *)indexesOfObjectsWithOptions:(NSEnumerationOptions)opts passingTest:(BOOL (NS_NOESCAPE ^)(ObjectType obj, NSUInteger idx, BOOL *stop))predicate API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (NSIndexSet *)indexesOfObjectsAtIndexes:(NSIndexSet *)s options:(NSEnumerationOptions)opts passingTest:(BOOL (NS_NOESCAPE ^)(ObjectType obj, NSUInteger idx, BOOL *stop))predicate API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (NSArray<ObjectType> *)sortedArrayUsingComparator:(NSComparator NS_NOESCAPE)cmptr API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (NSArray<ObjectType> *)sortedArrayWithOptions:(NSSortOptions)opts usingComparator:(NSComparator NS_NOESCAPE)cmptr API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

typedef NS_OPTIONS(NSUInteger, NSBinarySearchingOptions) {

​	NSBinarySearchingFirstEqual = (1UL << 8),

​	NSBinarySearchingLastEqual = (1UL << 9),

​	NSBinarySearchingInsertionIndex = (1UL << 10),

};

\- (NSUInteger)indexOfObject:(ObjectType)obj inSortedRange:(NSRange)r options:(NSBinarySearchingOptions)opts usingComparator:(NSComparator NS_NOESCAPE)cmp API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0)); // binary search

@end

@interface NSArray<ObjectType> (NSArrayCreation)

\- (instancetype)initWithArray:(NSArray<ObjectType> *)array copyItems:(BOOL)flag; //如果flag为YES，在array数组里面的对象都必须遵循NSCopying协议

/* Reads array stored in NSPropertyList format from the specified url. */

\- (nullable NSArray<ObjectType> *)initWithContentsOfURL:(NSURL *)url error:(NSError **)error  API_AVAILABLE(macos(10.13), ios(11.0), watchos(4.0), tvos(11.0));

/* Reads array stored in NSPropertyList format from the specified url. */

\+ (nullable NSArray<ObjectType> *)arrayWithContentsOfURL:(NSURL *)url error:(NSError **)error API_AVAILABLE(macos(10.13), ios(11.0), watchos(4.0), tvos(11.0)) NS_SWIFT_UNAVAILABLE("Use initializer instead");

@end

/****************	Mutable Array		****************/

@interface NSMutableArray<ObjectType> : NSArray<ObjectType>

\- (void)addObject:(ObjectType)anObject; //如果anObject为nil，会崩溃

\- (void)insertObject:(ObjectType)anObject atIndex:(NSUInteger)index; //如果anObject会崩溃，如果index越界也会崩溃

\- (void)removeObjectAtIndex:(NSUInteger)index; //如果index越界会崩溃

\- (void)replaceObjectAtIndex:(NSUInteger)index withObject:(ObjectType)anObject; //如果index越界会崩溃，如果anObject为nil会崩溃

\- (nullable instancetype)initWithCoder:(NSCoder *)aDecoder NS_DESIGNATED_INITIALIZER;

@end

@interface NSMutableArray<ObjectType> (NSExtendedMutableArray)

​    

\- (void)exchangeObjectAtIndex:(NSUInteger)idx1 withObjectAtIndex:(NSUInteger)idx2; //如果idx1或idx2越界，则会崩溃

\- (void)removeObject:(ObjectType)anObject inRange:(NSRange)range; //如果range越界，则会崩溃

\- (void)removeObjectIdenticalTo:(ObjectType)anObject inRange:(NSRange)range;//如果range越界，则会崩溃

\- (void)removeObjectIdenticalTo:(ObjectType)anObject;

\- (void)removeObjectsFromIndices:(NSUInteger *)indices numIndices:(NSUInteger)cnt API_DEPRECATED("Not supported", macos(10.0,10.6), ios(2.0,4.0), watchos(2.0,2.0), tvos(9.0,9.0));

\- (void)removeObjectsInArray:(NSArray<ObjectType> *)otherArray;

\- (void)removeObjectsInRange:(NSRange)range;//如果range越界，则会崩溃

\- (void)replaceObjectsInRange:(NSRange)range withObjectsFromArray:(NSArray<ObjectType> *)otherArray range:(NSRange)otherRange;//如果range越界，则会崩溃

\- (void)replaceObjectsInRange:(NSRange)range withObjectsFromArray:(NSArray<ObjectType> *)otherArray;//如果range越界，则会崩溃

\- (void)setArray:(NSArray<ObjectType> *)otherArray;

\- (void)sortUsingFunction:(NSInteger (NS_NOESCAPE *)(ObjectType,  ObjectType, void * _Nullable))compare context:(nullable void *)context;

\- (void)sortUsingSelector:(SEL)comparator;

\- (void)insertObjects:(NSArray<ObjectType> *)objects atIndexes:(NSIndexSet *)indexes; //如果indexes越界，或者indexs为nil会崩溃

\- (void)removeObjectsAtIndexes:(NSIndexSet *)indexes;//如果indexes越界，或者indexs为nil会崩溃

\- (void)replaceObjectsAtIndexes:(NSIndexSet *)indexes withObjects:(NSArray<ObjectType> *)objects;//如果indexes越界，或者indexs为nil会崩溃

\- (void)setObject:(ObjectType)obj atIndexedSubscript:(NSUInteger)idx API_AVAILABLE(macos(10.8), ios(6.0), watchos(2.0), tvos(9.0));

\- (void)sortUsingComparator:(NSComparator NS_NOESCAPE)cmptr API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (void)sortWithOptions:(NSSortOptions)opts usingComparator:(NSComparator NS_NOESCAPE)cmptr API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

@end

@interface NSMutableArray<ObjectType> (NSMutableArrayCreation)

\+ (nullable NSMutableArray<ObjectType> *)arrayWithContentsOfFile:(NSString *)path; //文件必须是xml格式才能读取

\+ (nullable NSMutableArray<ObjectType> *)arrayWithContentsOfURL:(NSURL *)url;

\- (nullable NSMutableArray<ObjectType> *)initWithContentsOfFile:(NSString *)path;

\- (nullable NSMutableArray<ObjectType> *)initWithContentsOfURL:(NSURL *)url;

@end