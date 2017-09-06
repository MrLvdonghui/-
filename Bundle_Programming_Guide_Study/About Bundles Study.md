## About Bundles
Bundles are a convenient way to deliver software in macOS and iOS. Bundles provide a simplified interface for end users and at the same time provide support for development. This chapter provides an introduction to bundles and discusses the role they play in macOS and iOS(Bundle是在MacOS和iOS中提供软件的便捷方式。Bundle为终端用户提供简化的界面，同时为开发提供支持。本章提供了Bundle的介绍，并讨论了它们在macOS和iOS中的作用)
## Bundles and Packages
Although bundles and packages are sometimes referred to interchangeably, they actually represent very distinct concepts(虽然bundles和packages有时被称为可互换的，但它们实际上代表着非常不同的概念)
* A package is any directory that the Finder presents to the user as if it were a single file.
* A bundle is a directory with a standardized hierarchical structure that holds executable code and the resources used by that code
> note: 即使软件包默认为不透明文件，用户仍然可以查看和修改其内容。在包目录的上下文菜单中是“显示包内容”命令。选择此命令将显示一个新的Finder窗口，设置为包目录的顶层。用户可以使用此窗口来导航包的目录结构，并进行更改，就像它是常规目录层次结构一样。

packages可以改善用户体验(如何应用程序和可加载软件包直接运行或者用于其他用途，不需要用户了解内部实行),bundles定义用于组织与您的软件相关联的代码和资源的基本结构,bundle的确切结构取决于您是创建应用程序，框架还是插件。它还取决于其他因素，如目标平台和插件的类型.

bundles和packages有时被认为是可互换的原因是许多类型的bundles也是packages.例如，应用程序和可加载的软件包是软件包，因为它们通常被系统视为不透明目录。但是并不是所有的bundle都是包，反之亦然.

> 总结: 1、package是单一的抽象文件，使得使用者不可随意更改，一般是透明的。2、bundles是用于定义用于组织与您的软件相关联的代码和资源的基本结构。3、即单一的抽象文件，又满足bundles基本结构，就即是package又是bundle咯。

## About Bundle Display Names
当用户更改包的名称时，更改只是表面的。Finder不是在文件系统中更改包名称，而是将一个单独的字符串（称为显示名称）与bundles相关联，并显示该字符串.

## The Advantages of Bundles
* 因为bundle是文件系统中的目录层次结构，所以bundle只包含文件。因此，您可以使用所有相同的基于文件的界面打开包资源，就像打开其他类型的文件一样。
* bundle目录结构可以轻松支持多个本地化。您可以轻松添加新的本地化资源或删除不需要的资源
* bunble可以驻留在许多不同格式的卷上，包括诸如HFS，HFS +和AFP之类的多种叉格式，以及UFS，SMB和NFS等单叉格式
* 用户可以通过在Finder中拖动它们来安装，重新定位和删除软件包.
* bundle也是package的话，因此被视为不透明文件，不太容易受到意外的用户修改，例如关键资源的删除，修改或重命名.
* bundle可以支持多种芯片架构（PowerPC，Intel）和不同的地址空间要求（32位/ 64位）。它还可以支持包含专门的可执行文件（例如，针对特定向量指令集优化的库
* 大多数（但不是全部）可执行代码可以捆绑。应用程序，框架（共享库）和插件都支持捆绑包模型。静态库，动态库，shell脚本和UNIX命令行工具不使用bundle结构
* bundle的应用程序可以直接从服务器运行。不需要在本地系统上安装特殊的共享库，扩展和资源

## Types of Bundles
* Application
* Frameworks
* Plug-Ins

> 虽然文档格式可以利用捆绑结构来组织其内容，但文档通常不会以纯粹的意义被视为捆绑。实现为目录并被视为不透明类型的文档被认为是文档包，而不管其内部格式如何。有关文档包的详细信息，请参阅[Document Packages](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFBundles/DocumentPackages/DocumentPackages.html#//apple_ref/doc/uid/10000123i-CH106-SW1)。

## Creating a Bundle
在大多数情况下，您不会手动创建bundles或packages。当您创建新的Xcode项目（或向现有项目添加目标）时，Xcode会在需要时自动创建所需的bundle结构。例如，应用程序，框架和可加载bundle目标都具有关联的bundle结构。当您构建任何这些目标时，Xcode会为您自动创建相应的bundles
> 某些Xcode目标（例如shell工具和静态库）不会导致创建捆绑包或包。这是正常的，不需要为这些目标类型专门创建bundle。为这些目标生成的生成的二进制文件旨在按原样使用

如果您使用make文件（而不是Xcode）来构建项目，那么创建bundle并不是那么神奇。bundle只是文件系统中具有明确定义结构的目录，并将特定文件扩展名添加到捆绑包目录名称的末尾。只要您创建顶级捆绑包目录并适当地构建您的捆绑包的内容，您就可以使用编程支持访问这些内容来访问捆绑包。
> 有关如何组织bundle目录的更多信息，请参阅[Bundle Structures](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFBundles/BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW1)。

## Guidelines for Using Bundles
* 始终Info.plist在您的包中包含一个信息属性列表（）文件。确保包含推荐使用的捆绑类型的密钥。有关可以包含在此文件中的所有密钥的列表，请参阅运行时配置指南
* 如果应用程序无法在没有特定资源文件的情况下运行，请将该文件包含在应用程序包中。应用程序应始终包含他们需要操作的所有图像，字符串文件，本地化资源和插件。非关键资源应尽可能类似地存储在应用程序包中，但如果需要，可以将其放在bundle外部。有关应用程序捆绑结构的更多信息，请参阅应用程序包
> note: 这边概念比较模糊,详见官方[About Bundles](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFBundles/AboutBundles/AboutBundles.html#//apple_ref/doc/uid/10000123i-CH100-SW1)













