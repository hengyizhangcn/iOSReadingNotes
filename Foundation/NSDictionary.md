NS_ASSUME_NONNULL_BEGIN

@interface NSDictionary<__covariant KeyType, __covariant ObjectType> : NSObject <NSCopying, NSMutableCopying, NSSecureCoding, NSFastEnumeration>

\- (NSEnumerator<KeyType> *)keyEnumerator;

\- (instancetype)init NS_DESIGNATED_INITIALIZER;

\#if TARGET_OS_WIN32

\- (instancetype)initWithObjects:(const ObjectType _Nonnull [_Nullable])objects forKeys:(const KeyType _Nonnull [_Nullable])keys count:(NSUInteger)cnt;

\#else

\- (instancetype)initWithObjects:(const ObjectType _Nonnull [_Nullable])objects forKeys:(const KeyType <NSCopying> _Nonnull [_Nullable])keys count:(NSUInteger)cnt NS_DESIGNATED_INITIALIZER; //objects和keys都为c数组，若数组中存在nil元素会崩溃，若count越界会崩溃，使用方法如下：（其中objects和keys的元素个数不需要一致）

```
NSString *date[2];
date[0] = @"monday";
date[1] = @"tuesday";        
NSString *weatherStatus[2];
weatherStatus[0] = @"cloudy";
weatherStatus[1] = @"windy";
weatherStatus[2] = @"rainy";
NSDictionary *newDict = [[NSDictionary alloc] initWithObjects:weatherStatus forKeys:date count:2];
```



\#endif

\- (nullable instancetype)initWithCoder:(NSCoder *)aDecoder NS_DESIGNATED_INITIALIZER;

@end

@interface NSDictionary<KeyType, ObjectType> (NSExtendedDictionary)

@property (readonly, copy) NSString *description;

@property (readonly, copy) NSString *descriptionInStringsFileFormat;

\- (NSString *)descriptionWithLocale:(nullable id)locale;

\- (NSString *)descriptionWithLocale:(nullable id)locale indent:(NSUInteger)level;

\- (NSEnumerator<ObjectType> *)objectEnumerator;

\- (NSArray<ObjectType> *)objectsForKeys:(NSArray<KeyType> *)keys notFoundMarker:(ObjectType)marker; //marker，如果数组keys中的键没有对应的值，则用marker代替

/* Serializes this instance to the specified URL in the NSPropertyList format (using NSPropertyListXMLFormat_v1_0). For other formats use NSPropertyListSerialization directly. */

\- (BOOL)writeToURL:(NSURL *)url error:(NSError **)error API_AVAILABLE(macos(10.13), ios(11.0), watchos(4.0), tvos(11.0));

\- (NSArray<KeyType> *)keysSortedByValueUsingSelector:(SEL)comparator;

// count refers to the number of elements in the dictionary

\- (void)getObjects:(ObjectType _Nonnull __unsafe_unretained [_Nullable])objects andKeys:(KeyType _Nonnull __unsafe_unretained [_Nullable])keys count:(NSUInteger)count API_AVAILABLE(macos(10.7), ios(5.0), watchos(2.0), tvos(9.0)) NS_SWIFT_UNAVAILABLE("Use 'allKeys' and/or 'allValues' instead"); //使用方法如下：

```

        NSInteger count = 2;
        NSDictionary *newDict = [[NSDictionary alloc] initWithObjects:weatherStatus forKeys:date count:3];
        id __unsafe_unretained exchangeArr1[count];
        id __unsafe_unretained exchangeArr2[count];
        [newDict getObjects:exchangeArr1 andKeys:exchangeArr2 count:count];
```



\- (nullable ObjectType)objectForKeyedSubscript:(KeyType)key API_AVAILABLE(macos(10.8), ios(6.0), watchos(2.0), tvos(9.0));

\- (void)enumerateKeysAndObjectsUsingBlock:(void (NS_NOESCAPE ^)(KeyType key, ObjectType obj, BOOL *stop))block API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (void)enumerateKeysAndObjectsWithOptions:(NSEnumerationOptions)opts usingBlock:(void (NS_NOESCAPE ^)(KeyType key, ObjectType obj, BOOL *stop))block API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

```
如果opts为NSEnumerationConcurrent，遍历出的结果每次可能都不一样，但需要注意的是block内的代码必须是并发调用安全的...
```

-(NSArray<KeyType> *)keysSortedByValueUsingComparator:(NSComparator NS_NOESCAPE)cmptr API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (NSArray<KeyType> *)keysSortedByValueWithOptions:(NSSortOptions)opts usingComparator:(NSComparator NS_NOESCAPE)cmptr API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (NSSet<KeyType> *)keysOfEntriesPassingTest:(BOOL (NS_NOESCAPE ^)(KeyType key, ObjectType obj, BOOL *stop))predicate API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

```
//用于获取经predicate过滤的键
NSSet *setTemp = [newDict keysOfEntriesPassingTest:^BOOL(id  _Nonnull key, id  _Nonnull obj, BOOL * _Nonnull stop) {
            NSLog(@"key=%@, obj=%@", key, obj);
            if ([key hasPrefix:@"t"]) {
                return NO;
            }
            return YES;
        }];
```

\- (NSSet<KeyType> *)keysOfEntriesWithOptions:(NSEnumerationOptions)opts passingTest:(BOOL (NS_NOESCAPE ^)(KeyType key, ObjectType obj, BOOL *stop))predicate API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

@end

@interface NSDictionary<KeyType, ObjectType> (NSDeprecated)

/// This method is unsafe because it could potentially cause buffer overruns. You should use -getObjects:andKeys:count:

\+ (nullable NSDictionary<KeyType, ObjectType> *)dictionaryWithContentsOfFile:(NSString *)path;//file必须是xml类型

\+ (nullable NSDictionary<KeyType, ObjectType> *)dictionaryWithContentsOfURL:(NSURL *)url;//url对应内容必须是xml类型

\- (nullable NSDictionary<KeyType, ObjectType> *)initWithContentsOfFile:(NSString *)path;

\- (nullable NSDictionary<KeyType, ObjectType> *)initWithContentsOfURL:(NSURL *)url;

\- (BOOL)writeToFile:(NSString *)path atomically:(BOOL)useAuxiliaryFile;

\- (BOOL)writeToURL:(NSURL *)url atomically:(BOOL)atomically; // the atomically flag is ignored if url of a type that cannot be written atomically.

@end

@interface NSDictionary<KeyType, ObjectType> (NSDictionaryCreation)

\+ (instancetype)dictionary;

\+ (instancetype)dictionaryWithObject:(ObjectType)object forKey:(KeyType <NSCopying>)key;

\#if TARGET_OS_WIN32

\+ (instancetype)dictionaryWithObjects:(const ObjectType _Nonnull [_Nullable])objects forKeys:(const KeyType _Nonnull [_Nullable])keys count:(NSUInteger)cnt;

\#else

\+ (instancetype)dictionaryWithObjects:(const ObjectType _Nonnull [_Nullable])objects forKeys:(const KeyType <NSCopying> _Nonnull [_Nullable])keys count:(NSUInteger)cnt;

```
cnt不能超过objects或keys的个数，否则会崩溃
```



\#endif

\+ (instancetype)dictionaryWithObjectsAndKeys:(id)firstObject, ... NS_REQUIRES_NIL_TERMINATION NS_SWIFT_UNAVAILABLE("Use dictionary literals instead");

\+ (instancetype)dictionaryWithDictionary:(NSDictionary<KeyType, ObjectType> *)dict;

\+ (instancetype)dictionaryWithObjects:(NSArray<ObjectType> *)objects forKeys:(NSArray<KeyType <NSCopying>> *)keys;

\- (instancetype)initWithObjectsAndKeys:(id)firstObject, ... NS_REQUIRES_NIL_TERMINATION;

\- (instancetype)initWithDictionary:(NSDictionary<KeyType, ObjectType> *)otherDictionary;

\- (instancetype)initWithDictionary:(NSDictionary<KeyType, ObjectType> *)otherDictionary copyItems:(BOOL)flag;

\- (instancetype)initWithObjects:(NSArray<ObjectType> *)objects forKeys:(NSArray<KeyType <NSCopying>> *)keys;

/* Reads dictionary stored in NSPropertyList format from the specified url. */

\- (nullable NSDictionary<NSString *, ObjectType> *)initWithContentsOfURL:(NSURL *)url error:(NSError **)error API_AVAILABLE(macos(10.13), ios(11.0), watchos(4.0), tvos(11.0));

/* Reads dictionary stored in NSPropertyList format from the specified url. */

\+ (nullable NSDictionary<NSString *, ObjectType> *)dictionaryWithContentsOfURL:(NSURL *)url error:(NSError **)error API_AVAILABLE(macos(10.13), ios(11.0), watchos(4.0), tvos(11.0)) NS_SWIFT_UNAVAILABLE("Use initializer instead");

@end

/****************	Mutable Dictionary	****************/

@interface NSMutableDictionary<KeyType, ObjectType> : NSDictionary<KeyType, ObjectType>

\- (void)removeObjectForKey:(KeyType)aKey;

\- (void)setObject:(ObjectType)anObject forKey:(KeyType <NSCopying>)aKey;

\- (instancetype)init NS_DESIGNATED_INITIALIZER;

\- (instancetype)initWithCapacity:(NSUInteger)numItems NS_DESIGNATED_INITIALIZER;

\- (nullable instancetype)initWithCoder:(NSCoder *)aDecoder NS_DESIGNATED_INITIALIZER;

@end

@interface NSMutableDictionary<KeyType, ObjectType> (NSExtendedMutableDictionary)

\- (void)addEntriesFromDictionary:(NSDictionary<KeyType, ObjectType> *)otherDictionary;

\- (void)removeAllObjects;

\- (void)removeObjectsForKeys:(NSArray<KeyType> *)keyArray;

\- (void)setDictionary:(NSDictionary<KeyType, ObjectType> *)otherDictionary;

\- (void)setObject:(nullable ObjectType)obj forKeyedSubscript:(KeyType <NSCopying>)key API_AVAILABLE(macos(10.8), ios(6.0), watchos(2.0), tvos(9.0));

@end

@interface NSMutableDictionary<KeyType, ObjectType> (NSMutableDictionaryCreation)

\+ (instancetype)dictionaryWithCapacity:(NSUInteger)numItems;

\+ (nullable NSMutableDictionary<KeyType, ObjectType> *)dictionaryWithContentsOfFile:(NSString *)path;

\+ (nullable NSMutableDictionary<KeyType, ObjectType> *)dictionaryWithContentsOfURL:(NSURL *)url;

\- (nullable NSMutableDictionary<KeyType, ObjectType> *)initWithContentsOfFile:(NSString *)path;

\- (nullable NSMutableDictionary<KeyType, ObjectType> *)initWithContentsOfURL:(NSURL *)url;

@end

@interface NSDictionary<KeyType, ObjectType> (NSSharedKeySetDictionary)

/*  Use this method to create a key set to pass to +dictionaryWithSharedKeySet:.

 The keys are copied from the array and must be copyable.

 If the array parameter is nil or not an NSArray, an exception is thrown.

 If the array of keys is empty, an empty key set is returned.

 The array of keys may contain duplicates, which are ignored (it is undefined which object of each duplicate pair is used).

 As for any usage of hashing, is recommended that the keys have a well-distributed implementation of -hash, and the hash codes must satisfy the hash/isEqual: invariant.

 Keys with duplicate hash codes are allowed, but will cause lower performance and increase memory usage.

 */

\+ (id)sharedKeySetForKeys:(NSArray<KeyType <NSCopying>> *)keys API_AVAILABLE(macos(10.8), ios(6.0), watchos(2.0), tvos(9.0));

@end

@interface NSMutableDictionary<KeyType, ObjectType> (NSSharedKeySetDictionary)

/*  Create a mutable dictionary which is optimized for dealing with a known set of keys.

 Keys that are not in the key set can still be set into the dictionary, but that usage is not optimal.

 As with any dictionary, the keys must be copyable.

 If keyset is nil, an exception is thrown.

 If keyset is not an object returned by +sharedKeySetForKeys:, an exception is thrown.

 */

\+ (NSMutableDictionary<KeyType, ObjectType> *)dictionaryWithSharedKeySet:(id)keyset API_AVAILABLE(macos(10.8), ios(6.0), watchos(2.0), tvos(9.0));

@end

@interface NSDictionary<K, V> (NSGenericFastEnumeraiton) <NSFastEnumeration>

\- (NSUInteger)countByEnumeratingWithState:(NSFastEnumerationState *)state objects:(K __unsafe_unretained _Nullable [_Nonnull])buffer count:(NSUInteger)len;

@end

NS_ASSUME_NONNULL_END