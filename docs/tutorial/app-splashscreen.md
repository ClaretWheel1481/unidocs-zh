App启动时，系统加载应用渲染首页需要一定的时间，为了避免用户等待，手机操作系统提供了特殊的启动界面设计，让用户先看到一个简单的界面，等应用加载完成后正式进入应用首页。

这个界面，即被称为启动界面，也成称为 splash 或 lauch screen。

### 启动界面选项  

#### 等待首页渲染完毕后再关闭Splash图  

进入应用后启动界面在合适的时机会自动关闭并显示应用首页，可在manifest.json文件中进行配置。

打开项目的manifest.json文件，在“App启动界面配置”中的“启动界面选项”配置是否“等待首页渲染完毕后再关闭Splash图”：
![](https://native-res.dcloud.net.cn/images/uniapp/splashscreen/setting-closesplash-onrender.png)

勾选“等待首页渲染完毕后再关闭Splash图”，表示需要等待首页渲染完成后再关闭启动界面
不勾选“等待首页渲染完毕后再关闭Splash图”，则表示首页加载完成后就会关闭启动界面，此时首页可能没有完成渲染，在部分设备可能会闪一下白屏，不推荐使用。



### 启动界面设置

`启动界面`原本是一个静态png图片方式。随着移动设备屏幕的多样化，为了让每种屏幕启动时界面都不变形，开发者需要为越来越多的屏幕尺寸制作不同的图片。这带来很多问题，包括制作复杂、app包体积增大等。

HBuilderX中提供了以下`启动界面`方式：

| 启动界面 | 平台支持 | 特点 |
|-- |-- |-- |
| 通用启动界面 | Android、iOS均支持。其中在iOS上通过storyboard实现 |简单，自定义性弱、可适配不同屏幕 |
| 自定义启动图 | Android支持，同时支持使用[.9.png图片](https://ask.dcloud.net.cn/article/35527)；iOS可以打包，但从2020年6月30日起，无法提交Appstore|为了适配不同屏幕尺寸，需要做大量图片	|
|[自定义storyboard启动界面](#storyboard)|仅iOS支持，HBuilderX2.8+版本 | 可适配不同屏幕 |

> 提示：启动界面设置需提交云端打包后才能生效


<a id="common"/>

#### 通用启动界面

`通用启动界面`是一种简单、可适配不同屏幕的`启动界面`。它以app的logo、name为元素，自动生成适配不同屏幕尺寸、适配不同OS要求的`启动界面`。

`通用启动界面`有着最低的门槛，仅需要开发者为app在manifest里配好logo和name即可。并且符合任何应用商店的上线规范。它在iOS上就是通过storyboard实现的。

> 提示`通用启动界面`是为了方便开发者而设计的，它不具有很强的灵活性，如果开发者有较强的自定义需求，Android平台请使用[.9.png格式自定义启动图](https://ask.dcloud.net.cn/article/35527)、iOS平台请使用[自定义storyboard启动界面](#storyboard)

##### Android平台通用启动界面

打开项目的manifest.json文件，在“App启动界面配置”中的“Android启动界面设置”项下勾选“通用启动界面”：  
![](https://native-res.dcloud.net.cn/images/uniapp/splashscreen/setting-android.png)

通用启动界面上部显示应用图标（圆形裁剪，外围显示进度），图标下面为应用名称，效果如下：  
![](https://native-res.dcloud.net.cn/images/uniapp/splashscreen/common-android.png)


##### iOS平台通用启动界面

打开项目的manifest.json文件，在“App启动界面配置”中的“iOS启动界面设置”项下勾选“通用启动界面”：  
![](https://native-res.dcloud.net.cn/images/uniapp/splashscreen/setting-ios.png)

通用启动界面使用storyboard实现，在界面上部显示应用图标（无裁剪），图标下面为应用名称，效果如下：  
![](https://native-res.dcloud.net.cn/images/uniapp/splashscreen/common-ios.png)

如果应用开启适配暗黑模式/夜间模式/深色模式，则启动界面背景色会自动使用深色，文字颜色自动使用白色。


<a id="default"/>

#### 自定义启动图

##### Android平台自定义启动图

打开项目的manifest.json文件，在“App启动界面配置”中的“Android启动界面设置”项下勾选“自定义启动图”：  
![](https://native-res.dcloud.net.cn/images/uniapp/splashscreen/setting-android-default.png)

##### iOS平台自定义启动图

> 提示：2020年6月30日起，苹果AppStore审核要求应用在启动时，不能使用启动图片，必须改为使用Storyboard来制作启动界面，如果需要提交AppStore请使用[通用启动界面](#common)或[自定义storyboard启动界面](#storyboard)。

打开项目的manifest.json文件，在“App启动界面配置”中的“iOS启动界面设置”项下勾选“自定义启动图”：  
![](https://native-res.dcloud.net.cn/images/uniapp/splashscreen/setting-ios-default.png)



<a id="storyboard"/>

#### 自定义storyboard启动界面

Storyboard是Apple提供的一种简化的布局界面，通过xml描述界面，不能编程。
虽然无法制作非常灵活的界面，但满足启动界面是没问题的，比如设定背景色背景图、设定前景文字、图片的位置。
storyboard的优势是启动速度快。在App的真实首页被渲染完成前，可以快速给用户提供一个基于Storyboard的启动屏。

##### 制作storyboard文件

storyboard有两种制作方式：  
**1.** **直接使用附件提供的相对常用的 storyboard 模板，可在这个文件的基础上进行自定义（不需要 Mac 及 XCode，详情请查看附件中的 readme 教程）**
此 storyboard 文件适用于各种 iPhone 及 iPad 设备的横竖屏，支持自定义界面元素包括

- 页面背景图片或背景颜色
- 中间显示图片
- 底部显示文字及颜色
注：每一项都是可选的（比如只显示背景图片，只提供背景图片即可）

**2.** 使用xcode自行制作。xcode提供了可视化的制作storyboard的方式，但依赖于mac电脑。在xcode中制作storyboard的教程请自行网络搜索，请注意下面的注意事项。

HBuilderX需要的自定义storyboard文件格式为zip压缩包，里面要求包含XCode使用的.storyboard文件，以及.stroybard文件中使用的png图，如下图所示：  
![](https://img.cdn.aliyun.dcloud.net.cn/client/ask/pkg/splash/storyboard.png)

**注意事项**
- zip压缩包中不要包含目录，直接包含.storyboard和.png文件
- 有且只有一个.storyboard文件
- .storyboard文件可以通过xcode生成，也可以使用任何文本编辑器修改其源码，比如对.storyboard文件点右键，使用HBuilderX打开。它本质是一个xml文件。
- png文件名称中的@2x和@3x是适配不同分辨率的图片，系统会自动根据设备dpi选择，可参考[这里](https://www.jianshu.com/p/5b5f47ff87d4)
- 为了避免png文件名称与应用中内置的文件名冲突，建议以dc_launchscreen开头
- 制作 storyboard 时，**请将图片资源直接拖到放工程中，不要放到 imageset 里面，并且图片命名要保证一定的唯一性可参考附件中的示例**
- XCode中创建 storyboard 文件时，**页面元素添加约束时一定要相对于** `Superview`，不然启动图到 loading页面过渡时页面会跳动或者变形  
![](https://img.cdn.aliyun.dcloud.net.cn/client/ask/pkg/splash/xcode.png)

##### 使用storyboard文件

打开项目的manifest.json文件，在“App启动界面配置”中的“iOS启动界面设置”项下勾选“自定义storyboard启动界面”，并选择自己制作的storyboard文件：  
![](https://native-res.dcloud.net.cn/images/uniapp/splashscreen/setting-storyboard.png)

