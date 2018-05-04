# 安全测试Android应用程序

1.反编译：

  在这里下载apktool
  https://ibotpeaches.github.io/Apktool/

  macOS使用方法：

  cd 进入到apktool存放的位置
  java -jar apktool.jar 的 APK的路径


2.查看apk签名信息：
  
  在这里下载keytool：https://github.com/mojohaus/keytool

  macOS使用方法:

  cd 进入到keytool存放的位置

  keytool -printcert -file 文件路径

  注意：该文件为解压apk包或者反编译apk包后，“META-INF”目录下后缀名为.RSA的文件
  
3.MacOS使用模拟器进行Android应用测试，抓取证书验证的数据包
 
  下载夜神模拟器：https://www.yeshen.com

  首先安装burp证书：

  打开burp-Proxy-Options-Proxy Listeners-Import/export CA certificate -Export 

  选择certificate in der format-next

  命名为：PortSwiggerCA.cer，选择一个存放的位置。

  开启夜神模拟器，使用adb（后续将如何配置adb）：

  adb push 路径  /scared/  （路径为PortSwiggerCA.cer的路径）

  让夜神模拟器进入开发者模式：设置-关于平板电脑-连续点击版本号直至开启（它会弹出Toast告知你开启）

  回到设置-安全-从sd卡安装

  找到你的PortSwiggerCA.cer，点击安装，会提示你设置PIN密码，设置完密码即安装成功。

  接着下载xposed，windows的系统可能会对夜神模拟器的版本安装xposed存在不兼容的情况，建议先更新到最新版本的模拟器。

  macOS系统安装xposed，（不确定是否存在xposed不兼容的情况）安装完打开-xposed-框架-安装/更新 完成后重启模拟器

  然后安装JustTrueMe.apk，重启。

  即可使用burp捕获到一些有证书验证的数据包。

  如果使用eclipse中ddms看不到模拟器设备，则使用adb：
  adb connect 127.0.0.1:62001

4.使用AndBugs检查Android apk代码

  下载AndBugs：https://github.com/AndroBugs/AndroBugs_Framework

  macOS使用方法：
  python androbugs.py -f apk的路径
 （注意：如果apk进行了代码混淆及类抽取加固这些安全措施，AndBugs是无法检测到这些内容的，只能检测到能反编译的java代码。）








