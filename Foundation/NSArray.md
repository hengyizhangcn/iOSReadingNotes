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

\+ (instancetype)array;

\+ (instancetype)arrayWithObject:(ObjectType)anObject;

\+ (instancetype)arrayWithObjects:(const ObjectType _Nonnull [_Nonnull])objects count:(NSUInteger)cnt;

\+ (instancetype)arrayWithObjects:(ObjectType)firstObj, ... NS_REQUIRES_NIL_TERMINATION;

\+ (instancetype)arrayWithArray:(NSArray<ObjectType> *)array;

\- (instancetype)initWithObjects:(ObjectType)firstObj, ... NS_REQUIRES_NIL_TERMINATION;

\- (instancetype)initWithArray:(NSArray<ObjectType> *)array;

\- (instancetype)initWithArray:(NSArray<ObjectType> *)array copyItems:(BOOL)flag;

/* Reads array stored in NSPropertyList format from the specified url. */

\- (nullable NSArray<ObjectType> *)initWithContentsOfURL:(NSURL *)url error:(NSError **)error  API_AVAILABLE(macos(10.13), ios(11.0), watchos(4.0), tvos(11.0));

/* Reads array stored in NSPropertyList format from the specified url. */

\+ (nullable NSArray<ObjectType> *)arrayWithContentsOfURL:(NSURL *)url error:(NSError **)error API_AVAILABLE(macos(10.13), ios(11.0), watchos(4.0), tvos(11.0)) NS_SWIFT_UNAVAILABLE("Use initializer instead");

@end

@interface NSArray<ObjectType> (NSDeprecated)

/* This method is unsafe because it could potentially cause buffer overruns. You should use -getObjects:range: instead.

*/

\- (void)getObjects:(ObjectType _Nonnull __unsafe_unretained [_Nonnull])objects NS_SWIFT_UNAVAILABLE("Use 'as [AnyObject]' instead") API_DEPRECATED("Use -getObjects:range: instead", macos(10.0, 10.13), ios(2.0, 11.0), watchos(2.0, 4.0), tvos(9.0, 11.0));

/* These methods are deprecated, and will be marked with API_DEPRECATED in a subsequent release. Use the variants that use errors instead. */

\+ (nullable NSArray<ObjectType> *)arrayWithContentsOfFile:(NSString *)path;

\+ (nullable NSArray<ObjectType> *)arrayWithContentsOfURL:(NSURL *)url;

\- (nullable NSArray<ObjectType> *)initWithContentsOfFile:(NSString *)path;

\- (nullable NSArray<ObjectType> *)initWithContentsOfURL:(NSURL *)url;

\- (BOOL)writeToFile:(NSString *)path atomically:(BOOL)useAuxiliaryFile;

\- (BOOL)writeToURL:(NSURL *)url atomically:(BOOL)atomically;

@end

/****************	Mutable Array		****************/

@interface NSMutableArray<ObjectType> : NSArray<ObjectType>

\- (void)addObject:(ObjectType)anObject;

\- (void)insertObject:(ObjectType)anObject atIndex:(NSUInteger)index;

\- (void)removeLastObject;

\- (void)removeObjectAtIndex:(NSUInteger)index;

\- (void)replaceObjectAtIndex:(NSUInteger)index withObject:(ObjectType)anObject;

\- (instancetype)init NS_DESIGNATED_INITIALIZER;

\- (instancetype)initWithCapacity:(NSUInteger)numItems NS_DESIGNATED_INITIALIZER;

\- (nullable instancetype)initWithCoder:(NSCoder *)aDecoder NS_DESIGNATED_INITIALIZER;

@end

@interface NSMutableArray<ObjectType> (NSExtendedMutableArray)

​    

\- (void)addObjectsFromArray:(NSArray<ObjectType> *)otherArray;

\- (void)exchangeObjectAtIndex:(NSUInteger)idx1 withObjectAtIndex:(NSUInteger)idx2;

\- (void)removeAllObjects;

\- (void)removeObject:(ObjectType)anObject inRange:(NSRange)range;

\- (void)removeObject:(ObjectType)anObject;

\- (void)removeObjectIdenticalTo:(ObjectType)anObject inRange:(NSRange)range;

\- (void)removeObjectIdenticalTo:(ObjectType)anObject;

\- (void)removeObjectsFromIndices:(NSUInteger *)indices numIndices:(NSUInteger)cnt API_DEPRECATED("Not supported", macos(10.0,10.6), ios(2.0,4.0), watchos(2.0,2.0), tvos(9.0,9.0));

\- (void)removeObjectsInArray:(NSArray<ObjectType> *)otherArray;

\- (void)removeObjectsInRange:(NSRange)range;

\- (void)replaceObjectsInRange:(NSRange)range withObjectsFromArray:(NSArray<ObjectType> *)otherArray range:(NSRange)otherRange;

\- (void)replaceObjectsInRange:(NSRange)range withObjectsFromArray:(NSArray<ObjectType> *)otherArray;

\- (void)setArray:(NSArray<ObjectType> *)otherArray;

\- (void)sortUsingFunction:(NSInteger (NS_NOESCAPE *)(ObjectType,  ObjectType, void * _Nullable))compare context:(nullable void *)context;

\- (void)sortUsingSelector:(SEL)comparator;

\- (void)insertObjects:(NSArray<ObjectType> *)objects atIndexes:(NSIndexSet *)indexes;

\- (void)removeObjectsAtIndexes:(NSIndexSet *)indexes;

\- (void)replaceObjectsAtIndexes:(NSIndexSet *)indexes withObjects:(NSArray<ObjectType> *)objects;

\- (void)setObject:(ObjectType)obj atIndexedSubscript:(NSUInteger)idx API_AVAILABLE(macos(10.8), ios(6.0), watchos(2.0), tvos(9.0));

\- (void)sortUsingComparator:(NSComparator NS_NOESCAPE)cmptr API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (void)sortWithOptions:(NSSortOptions)opts usingComparator:(NSComparator NS_NOESCAPE)cmptr API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

@end

@interface NSMutableArray<ObjectType> (NSMutableArrayCreation)

\+ (instancetype)arrayWithCapacity:(NSUInteger)numItems;

\+ (nullable NSMutableArray<ObjectType> *)arrayWithContentsOfFile:(NSString *)path;

\+ (nullable NSMutableArray<ObjectType> *)arrayWithContentsOfURL:(NSURL *)url;

\- (nullable NSMutableArray<ObjectType> *)initWithContentsOfFile:(NSString *)path;

\- (nullable NSMutableArray<ObjectType> *)initWithContentsOfURL:(NSURL *)url;

@end