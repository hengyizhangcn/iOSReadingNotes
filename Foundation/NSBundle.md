@class NSString, NSURL, NSError, NSUUID, NSLock, NSNumber;

NS_ASSUME_NONNULL_BEGIN

/* Because NSBundle caches allocated instances, subclasses should be prepared

   to receive an already initialized object back from [super initWithPath:] */

/* Methods for creating or retrieving bundle instances. */

@property (class, readonly, strong) NSBundle *mainBundle;

\+ (nullable instancetype)bundleWithPath:(NSString *)path;

\- (nullable instancetype)initWithPath:(NSString *)path NS_DESIGNATED_INITIALIZER;

\+ (nullable instancetype)bundleWithURL:(NSURL *)url API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (nullable instancetype)initWithURL:(NSURL *)url API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

```
NSBundle会缓存实例，如果路径上的NSBundle对象已存在，则返回存在的对象，不存在则创建新的
```



\+ (NSBundle *)bundleForClass:(Class)aClass;

\+ (nullable NSBundle *)bundleWithIdentifier:(NSString *)identifier;

```
使用bundleWithIdentifier有时比bundleForClass更高效，但如果失败的话，该方法将会使用潜在的耗时探索法去定位bundle，有可能会遍历整个文件系统
所以为了避免该时间消耗，使用bundleWithPath或者bundleWithUrl更好！
```



@property (class, readonly, copy) NSArray<NSBundle *> *allBundles; //返回当前应用的所有非框架包

@property (class, readonly, copy) NSArray<NSBundle *> *allFrameworks; //包括bundles和frameworks

/* Methods for loading and unloading bundles. */

\- (BOOL)load;

@property (readonly, getter=isLoaded) BOOL loaded;

\- (BOOL)unload;

\- (BOOL)preflightAndReturnError:(NSError **)error API_AVAILABLE(macos(10.5), ios(2.0), watchos(2.0), tvos(9.0));

\- (BOOL)loadAndReturnError:(NSError **)error API_AVAILABLE(macos(10.5), ios(2.0), watchos(2.0), tvos(9.0));

/* Methods for locating various components of a bundle. */

@property (readonly, copy) NSURL *bundleURL API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

@property (nullable, readonly, copy) NSURL *resourceURL API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

@property (nullable, readonly, copy) NSURL *executableURL API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (nullable NSURL *)URLForAuxiliaryExecutable:(NSString *)executableName API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));//返回附加的可执行程序的url

@property (nullable, readonly, copy) NSURL *privateFrameworksURL API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

@property (nullable, readonly, copy) NSURL *sharedFrameworksURL API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

@property (nullable, readonly, copy) NSURL *sharedSupportURL API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

@property (nullable, readonly, copy) NSURL *builtInPlugInsURL API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

@property (nullable, readonly, copy) NSURL *appStoreReceiptURL API_AVAILABLE(macos(10.7), ios(7.0), watchos(2.0), tvos(9.0)); //获取应用在appstore上的付费收据

```
(lldb) po Bundle.main.appStoreReceiptURL
▿ Optional<URL>
  ▿ some : file:///Users/zhy/Library/Developer/CoreSimulator/Devices/866D2568-05F4-4AAD-9FF2-21248CBB3081/data/Containers/Data/Application/77CCEBF7-A9F4-4B1F-BC4F-E5B1B5CD0E47/StoreKit/receipt
```



@property (readonly, copy) NSString *bundlePath;

@property (nullable, readonly, copy) NSString *resourcePath;

@property (nullable, readonly, copy) NSString *executablePath;

\- (nullable NSString *)pathForAuxiliaryExecutable:(NSString *)executableName;

@property (nullable, readonly, copy) NSString *privateFrameworksPath;

@property (nullable, readonly, copy) NSString *sharedFrameworksPath;

@property (nullable, readonly, copy) NSString *sharedSupportPath;

@property (nullable, readonly, copy) NSString *builtInPlugInsPath;

/* Methods for locating bundle resources.  Instance methods locate resources in the bundle indicated by the receiver; class methods take an argument pointing to a bundle on disk.  In the class methods, bundleURL is a URL pointing to the location of a bundle on disk, and may not be nil; bundlePath is the path equivalent of bundleURL, an absolute path pointing to the location of a bundle on disk.  By contrast, subpath is a relative path to a subdirectory inside the relevant global or localized resource directory, and should be nil if the resource file in question is not in a subdirectory.  Where appropriate, localizationName is the name of a .lproj directory in the bundle, minus the .lproj extension; passing nil for localizationName retrieves only global resources, whereas using a method without this argument retrieves both global and localized resources (using the standard localization search algorithm).  */

\+ (nullable NSURL *)URLForResource:(nullable NSString *)name withExtension:(nullable NSString *)ext subdirectory:(nullable NSString *)subpath inBundleWithURL:(NSURL *)bundleURL API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0)); //使用 方法如下

```
[NSBundle URLForResource:@"FoundationDemo" withExtension:nil subdirectory:@"Debug" inBundleWithURL:[NSURL fileURLWithPath:@"/Users/zhy/Library/Developer/Xcode/DerivedData/FoundationDemo-gonbipzwthszmnclzsbercqtvxyn/Build/Products"]]
```



\+ (nullable NSArray<NSURL *> *)URLsForResourcesWithExtension:(nullable NSString *)ext subdirectory:(nullable NSString *)subpath inBundleWithURL:(NSURL *)bundleURL API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0)); //目录下的所有资源，用法如下

```
po [NSBundle URLsForResourcesWithExtension:@"bpmn" subdirectory:@"" inBundleWithURL:[NSURL URLWithString:@"/Users/zhy/Desktop"]]
<__NSArrayM 0x100647390>(
diagram.bpmn -- file:///Users/zhy/Desktop/,
diagram(1).bpmn -- file:///Users/zhy/Desktop/
)
```



\- (nullable NSURL *)URLForResource:(nullable NSString *)name withExtension:(nullable NSString *)ext API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (nullable NSURL *)URLForResource:(nullable NSString *)name withExtension:(nullable NSString *)ext subdirectory:(nullable NSString *)subpath API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (nullable NSURL *)URLForResource:(nullable NSString *)name withExtension:(nullable NSString *)ext subdirectory:(nullable NSString *)subpath localization:(nullable NSString *)localizationName API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (nullable NSArray<NSURL *> *)URLsForResourcesWithExtension:(nullable NSString *)ext subdirectory:(nullable NSString *)subpath API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\- (nullable NSArray<NSURL *> *)URLsForResourcesWithExtension:(nullable NSString *)ext subdirectory:(nullable NSString *)subpath localization:(nullable NSString *)localizationName API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0));

\+ (nullable NSString *)pathForResource:(nullable NSString *)name ofType:(nullable NSString *)ext inDirectory:(NSString *)bundlePath;

\+ (NSArray<NSString *> *)pathsForResourcesOfType:(nullable NSString *)ext inDirectory:(NSString *)bundlePath;

\- (nullable NSString *)pathForResource:(nullable NSString *)name ofType:(nullable NSString *)ext;

\- (nullable NSString *)pathForResource:(nullable NSString *)name ofType:(nullable NSString *)ext inDirectory:(nullable NSString *)subpath;

\- (nullable NSString *)pathForResource:(nullable NSString *)name ofType:(nullable NSString *)ext inDirectory:(nullable NSString *)subpath forLocalization:(nullable NSString *)localizationName;

\- (NSArray<NSString *> *)pathsForResourcesOfType:(nullable NSString *)ext inDirectory:(nullable NSString *)subpath;

\- (NSArray<NSString *> *)pathsForResourcesOfType:(nullable NSString *)ext inDirectory:(nullable NSString *)subpath forLocalization:(nullable NSString *)localizationName;

/* Method for retrieving localized strings. */

\- (NSString *)localizedStringForKey:(NSString *)key value:(nullable NSString *)value table:(nullable NSString *)tableName NS_FORMAT_ARGUMENT(1);

/* Methods for obtaining various information about a bundle. */

@property (nullable, readonly, copy) NSString *bundleIdentifier;

@property (nullable, readonly, copy) NSDictionary<NSString *, id> *infoDictionary;

@property (nullable, readonly, copy) NSDictionary<NSString *, id> *localizedInfoDictionary;

\- (nullable id)objectForInfoDictionaryKey:(NSString *)key;

\- (nullable Class)classNamed:(NSString *)className;

@property (nullable, readonly) Class principalClass;

/* Methods for dealing with localizations. */

@property (readonly, copy) NSArray<NSString *> *preferredLocalizations;	// a subset of this bundle's localizations, re-ordered into the preferred order for this process's current execution environment; the main bundle's preferred localizations indicate the language (of text) the user is most likely seeing in the UI

@property (readonly, copy) NSArray<NSString *> *localizations;		// list of language names this bundle appears to be localized to

@property (nullable, readonly, copy) NSString *developmentLocalization;

\+ (NSArray<NSString *> *)preferredLocalizationsFromArray:(NSArray<NSString *> *)localizationsArray;

\+ (NSArray<NSString *> *)preferredLocalizationsFromArray:(NSArray<NSString *> *)localizationsArray forPreferences:(nullable NSArray<NSString *> *)preferencesArray;

/* Method for determining executable architectures. */

enum {

​    NSBundleExecutableArchitectureI386      = 0x00000007,

​    NSBundleExecutableArchitecturePPC       = 0x00000012,

​    NSBundleExecutableArchitectureX86_64    = 0x01000007,

​    NSBundleExecutableArchitecturePPC64     = 0x01000012

};

@property (nullable, readonly, copy) NSArray<NSNumber *> *executableArchitectures API_AVAILABLE(macos(10.5), ios(2.0), watchos(2.0), tvos(9.0));

@end

\#define NSLocalizedString(key, comment) \

​	    [NSBundle.mainBundle localizedStringForKey:(key) value:@"" table:nil]

\#define NSLocalizedStringFromTable(key, tbl, comment) \

​	    [NSBundle.mainBundle localizedStringForKey:(key) value:@"" table:(tbl)]

\#define NSLocalizedStringFromTableInBundle(key, tbl, bundle, comment) \

​	    [bundle localizedStringForKey:(key) value:@"" table:(tbl)]

\#define NSLocalizedStringWithDefaultValue(key, tbl, bundle, val, comment) \

​	    [bundle localizedStringForKey:(key) value:(val) table:(tbl)]

@interface NSString (NSBundleExtensionMethods)

/* For strings with length variations, such as from a stringsdict file, this method returns the variant at the given width. If there is no variant at the given width, the one for the next smaller width is returned. And if there are none smaller, the smallest available is returned. For strings without variations, this method returns self. The unit that width is expressed in is decided by the application or framework. But it is intended to be some measurement indicative of the context a string would fit best to avoid truncation and wasted space.

*/ 

\- (NSString *)variantFittingPresentationWidth:(NSInteger)width API_AVAILABLE(macos(10.11), ios(9.0), watchos(2.0), tvos(9.0));

@end

FOUNDATION_EXPORT NSNotificationName const NSBundleDidLoadNotification;

FOUNDATION_EXPORT NSString * const NSLoadedClasses;	// notification key

/*

 The NSBundleResourceRequest class is used to interact with the on demand resource loading system.

 

 The purpose of the system is to allow an application to download certain resources on demand, when they are required. This also means that the system can purge a resource from disk when it is no longer required, which will save disk space. This class describes which resources are required, makes the request and reports progress, allows the app to specify how long during its execution that they are required.

 

 Resources are downloaded into the application container, and are made available via the standard NSBundle resource lookup API.

 

 The request object itself is lightweight. You may create as many as you need, for example to request the same set of tags in different components of your application.

*/

NS_CLASS_AVAILABLE(NA, 9_0)

@interface NSBundleResourceRequest : NSObject <NSProgressReporting>

\- (instancetype)init NS_UNAVAILABLE;

/*

 A tag and bundle are required arguments to the init methods. The tag argument is required and it must exist in the manifest of the specified bundle. The bundle argument describes an existing bundle which was built with on demand resources support. Any resources downloaded can be found using the standard NSBundle resource lookup API once the request is completed. If no bundle is specified then the main bundle is used.

 */

\- (instancetype)initWithTags:(NSSet<NSString *> *)tags;

\- (instancetype)initWithTags:(NSSet<NSString *> *)tags bundle:(NSBundle *)bundle NS_DESIGNATED_INITIALIZER;

/*

 Provides a hint to the resource loading system as to the loading priority of this request. Values are limited to between 0 and 1, with 1 being the highest priority. The default priority is 0.5.

 

 The exact meaning of the value is up to your application. The system will prefer to act on requests that have a higher priority (from the same application). You may change the priority at any time, even after a request has started. The system will make a best attempt to take the new priority into account.

 */

@property double loadingPriority;

/*

 The tags this request will load.

 */

@property (readonly, copy) NSSet<NSString *> *tags;

/*

 The bundle object that will hold the requested resources. After the -beginAccessingResourcesWithCompletionHandler: callback is invoked, you may use the standard bundle lookup APIs on this bundle object to find your resources.

 */

@property (readonly, strong) NSBundle *bundle;

/*

 Ask the system to fetch the resources that were part of the tag set in this request. Resources will not be purged while in use by the application (as indicated by the application using this begin API paired with a call to -endAccessingResources). If an application has too many fetched resources and the system is unable to reserve enough space for newly requested tags, the request may return an error.

 

 When you are finished with the resources and they may be purged off the disk, invoke -endAccessingResources. If the request object is deallocated, it will also inform the system that the resources are no longer in use.

 

 The completion block will be invoked on a non-main serial queue when the resources are available or an error has occurred. An example of a possible error that may be reported is the lack of a network connection or a problem connecting to the on-demand servers.

 

 Fetch requests are reference counted across the application. So if you have two requests outstanding with the same set of tags, each may be used independently without having to know about any global state. However, each NSBundleResourceRequest object may only be used once.

 

 If you cancel an outstanding request (via the cancel method on the NSProgress object, or cancelling a parent progress object you have created) the completion handler argument to this method will be called back with an NSUserCancelledError in the NSCocoaErrorDomain.

 

 Be sure to always invoke the -endAccessingResources method to balance a call to the begin method, even in the case of an error in the completion handler.

 

 If you want to access the resources again, create a new NSBundleResourceRequest object.

 */

\- (void)beginAccessingResourcesWithCompletionHandler:(void (^)(NSError * _Nullable error))completionHandler;

/*

 Inform the system that you wish to begin accessing the resources that are part of this request, but do not attempt to download any content over the network. The completion handler will be invoked with a YES argument if the resources are available.

 

 If the resources were available, then you must invoke the -endAccessingResources method once you are done accessing them. If the resources were not available, then you may invoke the -beginAccessingResourcesWithCompletionHandler: method to initiate a download of the resources.

*/

\- (void)conditionallyBeginAccessingResourcesWithCompletionHandler:(void (^)(BOOL resourcesAvailable))completionHandler;

/*

 Informs the system that you are finished with the resources that were part of the tag set in this request. Call this after you no longer need the resources to be available on disk. It is important to invoke this method to make room for newly requested resources. This method may only be invoked if you have received a callback from -beginAccessingResourcesWithCompletionHandler:. To cancel an in-progress request, invoke cancel on the -progress property.

 */

\- (void)endAccessingResources;

/*

 Progress for the request. The progress object will be valid at initialization and begin updating after the -beginAccessingResourcesWithCompletionHandler: method is called.

*/

@property (readonly, strong) NSProgress *progress;

@end

@interface NSBundle (NSBundleResourceRequestAdditions)

/* Set a preservation priority for tags that are included in this bundle for the On Demand Resources system. Preservation priorities may be between 0.0 and 1.0, with higher values being the last choice for purging by the system. The exact meaning of this value is up to your application as it only has meaning within the set of tags your application uses.

   

   The default value is 0.0.

 

   This method will throw an exception if the receiver bundle has no on demand resource tag information.

*/

\- (void)setPreservationPriority:(double)priority forTags:(NSSet<NSString *> *)tags API_AVAILABLE(ios(9.0), watchos(2.0), tvos(9.0)) API_UNAVAILABLE(macos);

\- (double)preservationPriorityForTag:(NSString *)tag API_AVAILABLE(ios(9.0), watchos(2.0), tvos(9.0)) API_UNAVAILABLE(macos);

@end

/*

 This notification is posted to the default notification center when the resource request system detects a low disk space condition.

 

 If the application is in the background, the system needs more space, and the application does not free up enough in response to the notification then the application may be killed. The application can free up space by calling -endAccessingResources on any outstanding requests. This will inform the system that you are done with those resources and it may purge the content to make room for a new request.

 

 Note that this notification may not be the same as low disk space on the system, as applications can have a smaller quota.

 */

FOUNDATION_EXPORT NSNotificationName const NSBundleResourceRequestLowDiskSpaceNotification API_AVAILABLE(ios(9.0), watchos(2.0), tvos(9.0)) API_UNAVAILABLE(macos);

/* Use this value for the loadingPriority property if the user is doing nothing but waiting on the result of this request. The system will dedicate the maximum amount of resources available to finishing this request as soon as possible.

 */

FOUNDATION_EXPORT double const NSBundleResourceRequestLoadingPriorityUrgent API_AVAILABLE(ios(9.0), watchos(2.0), tvos(9.0)) API_UNAVAILABLE(macos);

NS_ASSUME_NONNULL_END



参考：

1.Bundle编程规范

https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFBundles/Introduction/Introduction.html#//apple_ref/doc/uid/10000123i

bundle Id用来验证应用的签名

2.Frame编程规范

https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPFrameworks/Frameworks.html#//apple_ref/doc/uid/10000183i

3.SDK兼容规范

https://developer.apple.com/library/content/documentation/DeveloperTools/Conceptual/cross_development/Introduction/Introduction.html#//apple_ref/doc/uid/10000163i

4.Cocoa Core Competencies ★★★★★

https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/ObjectCreation.html#//apple_ref/doc/uid/TP40008195-CH39-SW1

5.App Programming Guide for iOS

https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/TheAppLifeCycle/TheAppLifeCycle.html#//apple_ref/doc/uid/TP40007072-CH2-SW9

1）在info.plist文件中，UIBackgroundModes有哪几种值？包括remote-notification、external-accessory等

2）描述了怎样的app允许在后台执行

3）怎样减少内存占用

4）如何提高应用性能

6.Resource Programming Guide

https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/LoadingResources/Strings/Strings.html#//apple_ref/doc/uid/10000051i-CH6-96936

7.Concepts in Objective-C Programming

https://developer.apple.com/library/content/documentation/General/Conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html#//apple_ref/doc/uid/TP40010810-CH14

8.Document-Based App Programming Guide for iOS

https://developer.apple.com/library/content/documentation/DataManagement/Conceptual/DocumentBasedAppPGiOS/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011149

9.View Controller Programming Guide for iOS

https://developer.apple.com/library/content/featuredarticles/ViewControllerPGforiPhoneOS/index.html#//apple_ref/doc/uid/TP40007457

10.Location and Maps Programming Guide

https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/LocationAwarenessPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009497

11.Drawing and Printing Guide for iOS

https://developer.apple.com/library/content/documentation/2DDrawing/Conceptual/DrawingPrintingiOS/Introduction/Introduction.html#//apple_ref/doc/uid/TP40010156

12.Concurrency Programming Guide

https://developer.apple.com/library/content/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091

13.Preferences and Settings Programming Guide

https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i

14.Local and Remote Notification Programming Guide

https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/index.html#//apple_ref/doc/uid/TP40008194

15.Internationalization and Localization Guide

https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/Introduction/Introduction.html#//apple_ref/doc/uid/10000171i
16.File System Programming Guide

https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40010672

17.SDK Compatibility Guide

https://developer.apple.com/library/content/documentation/DeveloperTools/Conceptual/cross_development/Introduction/Introduction.html#//apple_ref/doc/uid/10000163i

18.Coding Guidelines for Cocoa

https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html#//apple_ref/doc/uid/20001282-1001751-BCIJHEDH

19.Archives and Serializations Programming Guide

https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Archiving/Archiving.html#//apple_ref/doc/uid/10000047i

20.Collections Programming Topics

https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Collections/Collections.html#//apple_ref/doc/uid/10000034i

21.Programming with Objective-C

https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/FoundationTypesandCollections/FoundationTypesandCollections.html#//apple_ref/doc/uid/TP40011210-CH7-SW28

22.Error Handling Programming Guide

https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/ErrorHandlingCocoa/CreateCustomizeNSError/CreateCustomizeNSError.html#//apple_ref/doc/uid/TP40001806-CH204-BAJIIGCC

23.Information Property List Key Reference

https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW18

24.Key-Value Observing Programming Guide

https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i

6.xinetd和inetd比较？