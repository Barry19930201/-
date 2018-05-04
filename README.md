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

  开启夜神模拟器，使用adb（后续将说明如何配置adb）：

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
 
 5.下载Android Stuido，并配置adb
   
   我这里用的是macOS，下载macOS的Android Studio：http://tools.android-studio.org
   
   在安装完成之后，将android的adb工具所在目录加入环境变量里面去：
   
   在终端中输入 vim ~/.bash_profile ,打开 .bash_profile文件
   按 i 进入输入模式，在文件内容的末尾加入以下内容：
   #Setting PATH for Android ADB Tools
   export PATH=${PATH}:/Users/xxx/Library/Android/sdk/platform-tools
   export PATH=${PATH}:/Users/xxx/Library/Android/sdk/tools
   这里面的xxx根据自己实际的用户名称进行修改，然后点击 esc ，输入 :wq  回车（保存并退出文本）。添加完成后输入： source ~/.bash_profile 。
   

6.安装drozer
   
   MacOS终端下载drozer：git clone https://github.com/mwrlabs/drozer/
   模拟器也需要安装drozer，安装完之后打开“Embedded Server”
   
   运行：adb forward tcp:31415 tcp:31415
        drozer console connect
   即可。



7.安装Inspeckage
   
   打开xposed-下载-搜索“Inspeckage”，选择下载，勾选上，安装。
   打开运行“Inspeckage”，将“only user app”打开， “choose target”选择你已安装在模拟器（or手机）的应用程序。
   在macOS开启终端，adb forward tcp:8008 tcp:8008
   然后使用macOS的浏览器打开127.0.0.1:8008即可访问。
   
   
  
8.MobSF检测平台安装

   去docker官网下载docker，可以再将Kitematic安装了，拉取镜像方便一些。
   用docker安装：docker run -it -p 8000:8000 opensecurity/mobile-security-framework-mobsf:latest
   或者直接在Kitematic里搜索“mobsf”
   拉取完即可访问。可以使用kitematic进行镜像的管理，需要登录docker的账户。然后可以从这里直接启动MobSF，不需要输入命令行。
    
   (注意：docker镜像中的mobsf为v1.0版本，在github中的mobsf版本于目前更新到v0.9.5版本。此外这个docker方法安装无法对ipa进行静态分析，扫描apk
   文件也比搭建在本地的时间长) 
   
   
   
9.apk漏洞扫描平台

 
   有很多扫描的平台，不一一贴出来。个人感觉比较好用是盘古的：https://www.appscan.io/home.html
   还有360的，但不太稳定： http://appscan.360.cn  360的可以检查克隆漏洞。
    https://service.security.tencent.com/kingkong （唯一可以扫描ipa的，但作用比较小，不要太在意）
   
   
   




