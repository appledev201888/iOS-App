# iOS-App
使用企业证书重新签名iOS App

在网上查了不少文章最后找到如下方法可以在Xcode8下使用

解压你的ipa包
删除期内的签名文件: rm -rf Payload/Your-XXX.app/_CodeSignature
将你企业证书对应的mobileprovision文件copy到app文件中：cp -rf Your-XXX.mobileprovision Payload/Your-XXX.app/embedded.mobileprovision
准备好entitlements文件，该文件可以包含你使用的一些系统功能。例如：gps定位、healthkit等
重新签名：codesign -v -vvvv -f -s "iPhone Distribution: Your-XXX-Co., Ltd." --entitlements=Your-XXX.entitlements Payload/Your-XXX.app
重新打包： xcrun -sdk iphoneos PackageApplication -v Payload/Your-XXX.app -o ~/Desktop/Your-Resigned.ipa
这时候你就可以上传到Fir上测试了

注意：

上文中出现的Your-XXX就是你的要重新签名的app名称
iPhone Distribution: Your-XXX-Co., Ltd.是你KeyChain中企业证书的名称
一个可以用的entitlements内容如下：

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>application-identifier</key>
    <string>Your-App-Id.Your-BoundId</string>
    <key>com.apple.developer.team-identifier</key>
    <string>Your-App-Id</string>
    <key>com.apple.developer.healthkit</key>
    <true/>
</dict>
</plist>

上面的Your-App-Id就是你在开发者证书网站中的App Ids分类下的BoundId的prefix，例如：P499PN56FF
