# 配置鸿蒙的运行和发行功能@configs

## 准备工作

HX 里面有两个与鸿蒙相关的功能入口：

1. 【运行】菜单里的【运行到鸿蒙】
2. 【发行】菜单里的【App-Harmony-本地打包】

点击这些功能时，HX 首先会检查 `Settings.json` 里面配置的 `harmony.devTools.path`（应该是指向 DevEco Studio 或 Command Line Tools 的安装目录），
如果尚未配置，HX 会尝试自动检测电脑上安装的 DevEco Studio 并写入这个配置。

只有正确配置了这个选项，后续的功能才能正常执行。

## 操作过程

在一个 uni-app 项目中点击鸿蒙的运行或发行菜单后，HX 会检查 `harmony-configs` 目录内容的完整性，如果目录不存在或者里面缺少文件，会自动添加进去，已有的文件不会被覆盖。

在构建过程中，会在 `unpackage` 目录下生成鸿蒙工程目录，并把 `harmony-configs` 目录下的内容合并到这里，uni-app 项目编译的结果也会输出到这里。

然后 HX 会对这个鸿蒙工程目录进行构建、打包、签名，如果是运行的话，会把生成的 .hap 安装到选定的鸿蒙设备上并启动运行，如果是发行则最终生成 .app 安装包。

## 配置

所有属于鸿蒙工程的配置调整，都可以写在 `harmony-configs` 目录下，具体内容可以参照鸿蒙的相关文档。其中常用内容有：

- 应用包名

    在 `harmony-configs/AppScrope/app.json5` 中修改 `app.bundleName`

- 数字签名证书相关信息

    在 `harmony-configs/build-profile.json5` 中修改 `signingConfigs`

    数字证书可以通过 DevEco Studio 来申请，申请的结果就写在 DevEco Studio 所打开的鸿蒙工程根目录下的 `build-profile.json5` 文件里，
    可以通过这种方式将申请到的证书信息抄写到 `harmony-configs/build-profile.json5` 里面。

    理论上，数字证书也可以在华为的 AppGallery Connect 服务中手工申请并下载，但是目前在 AGC 手工申请调试证书的时候无法开启受限的权限，
    导致签名后的应用包可能无法安装到设备。

## 调试运行时可能遇到的问题及处理方法

### 报错 `依赖包与运行设备不兼容`@install-parse-native-so-failed

在一个 uni-app 项目上首次使用【运行到鸿蒙】的时候，可能会遇到这个报错。
这是由于默认配置里面依赖了支付宝SDK，而这个包不支持运行在 Windows 平台的模拟器上，所以在打包后安装到模拟器设备时会报错。
出现这种情况时，如果并不需要这个依赖，可以修改 `harmony-configs/oh-package.json5` 文件，在 `dependencies` 里面删除 `@cashier_alipay/cashiersdk` 就行了；
如果确实需要，那就只能改用真机设备来运行了。

### 报错 `运行所需的权限没有签名授权`@permissions-failed

这是由于默认配置里面声明申请了一些权限，其中包含受限权限，这就要求安装包必须用具备足够权限授权的数字证书进行签名，否则无法安装到设备上。

如果业务代码里面并没有实际使用到这些权限，一个简单的办法就是修改 `harmony-configs/entry/src/main/module.json5` 文件，
删除 `module.requestPermissions` 数组里面的这三项内容（因为它们是需要白名单授权的 ACL 权限），重新运行即可：

- `ohos.permission.WRITE_IMAGEVIDEO`
- `ohos.permission.WRITE_CONTACTS`
- `ohos.permission.READ_PASTEBOARD`

如果确实需要这里的某些权限，那就需要申请一个调试证书，并配置到 `harmony-configs/build-profile.json5` 文件的 `app.signingConfigs` 中。
这里要注意，一定要通过 DevEco Studio 来申请这个证书，且需要开启 `Automatically generate signature` 选项，只有这样拿到的调试证书才会支持 ACL 权限。
手动申请的调试证书不会直接支持 ACL 权限，必须要获得白名单授权才可以，这个申请过程要求比较高。

## 关于数字签名证书的配置@singing

如前所述，如果业务代码用到了 ACL 权限，那么就需要申请并配置一个**调试证书**用于数字签名。
另外，在发行安装包的时候，一定需要配置一个**发布证书**，否则只能拿到一个未签名的安装包，是无法实际使用的。

数字签名证书需要配置到 `harmony-configs/build-profile.json5` 中，这个文件等同于一个普通的鸿蒙工程中对应的文件。

为了便于操作，可以用 DevEco Studio 创建一个简单的鸿蒙工程（注意设置好正确的应用包名 `bundleName`），在里面完成申请证书的操作，
具体方法可参考 [自动签名](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/ide-signing-V5#section18815157237)，
然后把 `build-profile.json5` 文件中 `app.signingConfigs` 的内容复制到 `harmony-configs/build-profile.json5` 中。

通过 DevEco Studio 申请得到的证书，缺省会保存到电脑的用户目录下，在 Windows 系统中一般是 `%USERPROFILE%\.ohos\config`，在 Mac 系统中一般是 `~/.ohos/config`。
配置信息中包含的三个文件缺省都是采用绝对路径来表示，也可以把这些文件移到 `harmony-configs` 目录下，这样就可以使用相对路径来表示，相对于 `harmony-configs` 目录。
如果要移动证书文件的位置，需注意跟这三个文件一起的还有一个名为 `material` 的目录，也要一起移动。