# Expected App Behaviors(预期的应用行为)
> 我的理解是： 应用程序实现应该定制好的行为和准备

## 1、Providing the Required Resources(提供所需的资源)
* An information property-list file: The info.plist File Contains metadata about your app,which the system users to interact with your app. Xcodecreate this file for you automatically in the info tab of you project.for more information,see [The information PropretyList File]()  
> info.list 是用于系统和应用程序交互的配置文件，Xcode 自动生成，更多信息[The information PropretyList File]()  
* A declaration of the app’s required capabilities. 
> 声明程序所需的设备功能。在Info.list中配置。
* One or more icons
> 配置程序icons，对应的image files配置文件中,用于不同设备应用图标、AppStroe显示图标、通知图标。
* One or more launch images，When the system launches an app for the first time on a device, it temporarily displays a static launch image on the screen. This image is your app’s launch image and it is a resource that you specify in your Xcode project. Launch images provide the user with immediate feedback that your app has launched while giving your app time to prepare its initial user interface. When your app’s window is configured and ready to be displayed, the system swaps out the launch image for that window.When a recent snapshot of your app’s user interface is available, the system prefers the use of that image over the use of your app’s launch images. The system takes a snapshot of your app’s user interface when your app transitions from the foreground to the background. When your app returns to the foreground, it uses that image instead of a launch image whenever possible. In cases where the user has killed your app or your app has not run for a long time, the system discards the snapshot and relies once again on your launch images
> 配置程序启动图。当程序启动时，同时初始用户界面准备过程完成之前，会使用默认程序启动图 和 最近可用的屏幕快照（屏幕快照优先）。 屏幕快照为，程序切换到后台时，界面所显示界面的快照。
当然还有其他方法可用修改屏幕快照 [忘记是什么了，待会再写](www.baidu.com)。
* 其他需要配置的属性,如何麦克风NSMicrophoneUsageDescription密钥配置，根据你程序需要的特定功能而定。
> 总结：info.list 包含程序所需资源的相关配置，可以通过General,Capabilities和Info tabs 进行修改配置。

#### The App Bundle (应用程序包了解包结构)
1. App executable（可执行应用）
2. info.list (信息属性配置列表）
3. 应用图标和启动图
4. storyBoard或者xib
5. Ad hoc distribution icon (临时分发图标 用于AppStroe 和 Itunes 显示的)
6. Settings bundle
> this bunble is optional. For more information about preferences and specifying a settings bundle, see [Preferences and Settings Programming Guide](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i) or [Preferences And Setting Programming Guide Study Note](www.baidu.com)
7. Nonlocalized resource files(非本地化资源) 
> Nonlocalized resources include things like images, sound files, movies, and custom data files that your app uses. All of these files should be placed at the top level of your app bundle
8. Subdirectories for localized resources (本地化资源的子目录)： 用于国际化 

> 总结：初步了解应用程序包结构， For more information about the structure of an iOS app bundle, see [Bundle Programming Guide](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFBundles/Introduction/Introduction.html#//apple_ref/doc/uid/10000123i). For information about how to load resource files from your bundle, see [Resource Programming Guide](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/LoadingResources/Introduction/Introduction.html#//apple_ref/doc/uid/10000051i).

## 2、Supporting User Privacy (支持用户隐私)
只有根据适用法律获得用户的知情同意才能访问用户或设备数据。此外，采取适当的步骤来保护用户和设备数据，并对您如何使用它们进行透明化。以下是您可以采取的一些最佳做法
* Review guidelines from government or industry sources(审查政府或行业来源的指导方针)
* Request access to sensitive user or device data, which is protected by the iOS system authorization settings, at the time your app needs the data. You must supply a purpose string (sometimes called a usage description string) in your app’s Info.plist file explaining why your app needs the data or resource you are attempting to access. Data protected by iOS system authorization settings includes location, contacts, calendar events, reminders, photos, media, and many other types as well; see Table 1-2. Provide reasonable fallback behavior in situations where the user does not grant access to the requested data
> 说白了要解释为何要访问这些敏感的数据
* Be transparent with users about how their data is going to be used. For example, when you submit your app to the App Store, specify a URL for your privacy policy or statement as part of your iTunes Connect metadata. You might also want to summarize that policy or statement in your app description(先忽略吧)
* Give the user control over their user or device data. Provide settings so that the user can disable access to certain types of sensitive information as needed.
> 用户可以通过设置模块去控制 是否可以访问他们的敏感数据
* Request and use the minimum amount of user or device data needed to accomplish a given task. Do not seek access to or collect data for non obvious reasons, for unnecessary reasons, or because you think it might be useful later(请求并使用完成给定任务所需的最少量的用户或设备数据。不要因为不明确的原因寻求访问或收集数据，或者因为您以后认为可能有用)
* Take reasonable steps to protect the user and device data that you collect in your apps. When storing such information locally, try to use the iOS data protection feature (described in Protecting Data Using On-Disk Encryption) to store it in an encrypted format. Use App Transport Security (as described in NSAppTransportSecurity) when sending user or device data over the network. (采用合理措施去保护数据，本地保存时，尝试使用ios数据保护功能，加密咯。通过网络访问时，使用应用数据安全性）
> more information see [Protecting Data Using On-Disk Encryption](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/StrategiesforImplementingYourApp/StrategiesforImplementingYourApp.html#//apple_ref/doc/uid/TP40007072-CH5-SW21) and [NSAppTransportSecurity](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW33)
* 广告方面的 more information see 我也不知道
* apps should use the identifierForVendor property of the UIDevice class or the advertisingIdentifier property of the ASIdentifierManager class, as appropriate.  uniqueIdentifier was deprecated in iOS 5.0. (不要使用uniqueIdentifier,设备使用identifierForVendor，广告使用advertisingIdentifier)
* 音频录入相关权限，最好在开始录入前进行权限授权提醒。

>  总结，1、 [数据授权提醒描述以及info.list中对应的key](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/ExpectedAppBehaviors/NaN);2、 数据加密；3、获取数据和数据用途透明合法化；

## 3、 Internationalizing Your App（国际化你的应用程序）
A typical iOS app requires localized versions of the following types of resource files（典型的iOS应用程序需要以下类型资源文件的本地化版本）：
1. Storyboard files (or nib files)
2. Strings files
3. Image files
4. Video and audio files

> 感觉上面都是废话,重点就是 Strings files ,更多信息 [Internationalization and Localization Guide](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/Introduction/Introduction.html#//apple_ref/doc/uid/10000171i)
















