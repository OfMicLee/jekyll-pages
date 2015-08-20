---
layout: post
title: "ios app 发布流程"
description: "简单描述怎样在appstore上发布你的app"
category: "mobile"
tags: [ios,app,publish]
---

>鉴于网络上的一些教程太过凌乱，又因为版本原因与实际情况不符，所以记录下自身真实发布一个app的过程

## 1、首先，你要有个开发者账号。
>开发者账号种类：个人账号、公司账号和企业账号这三种，还有一种是教育账号，这个就不多说了。申请地址：[https://developer.apple.com](https://developer.apple.com)

- 个人账号（Individual）

  费用99美金一年, 该账号在App Store销售者只能显示个人的ID，比如zhitian zhang，单人使用。个人账号只能有一个开发者。100个苹果的iOS设备UDID测试。

- 公司团队账号（Company/Organization）

  费用99美金一年, 该账号在App Store销售者可以显示类似Studios，或者自定义的团队名称，比如Mamshare INC，
  公司账号可以允许多个开发者协作开发，比个人多一些帐号管理的设置，可以设置多个AppleID，分4种管理级别权限，详细见备注。
  100个苹果的iOS设备UDID测试。但是申请时需要填写公司的邓白氏编码（D-U-N-S）。
  [申请地址](https://developer.apple.com/programs/enroll)

  **备注：**
  - Admin Legal权限：超级管理员。可以管理开发者和管理app store中的应用。
  - Admin权限：管理员，可以管理开发者。添加测试机子和管理团队证书。
  - Member权限：是普通开发者。只能下载证书和使用证书
  - No Access权限：没有相应的权限。

  邓白氏编码（D-U-N-S）
  [申请地址](https://developer.apple.com/ios/enroll/dunsLookupForm.action)

- 企业账号（Enterprise）

  费用299美金一年, 该账号开发应用不能发布到App Store，只能企业内部应用，苹果的iOS设备UDID数量不限制。企业账号适合不希望上线App Store，但是需要企业内部比如1000人的iOS设备都部署。公司测试部门需要全公司测试设备，突破100个UDID的限制。

- 教育账号(University)

  费用0美元 ，只能教育机构或学院内部使用。必须是苹果iOS开发者计划授权机构。不能对外正式发布iOS应用程序。
  [申请地址](https://developer.apple.com/programs/start/university)

## 2、进入苹果开发者网站，生成证书Certificate
如果你拥有一个开发者账户的话，在iOS Dev Center打开Certificates, Indentifiers & Profiles，你就可以看到如下的列表：
![ios Certificates](https://github.com/OfMicLee/img-hosting/blob/master/app/ios-certificate.png)

### 步骤：
- #### Certificates -> Production -> +

- #### What type of certificate do you need?

  Production -> App Store adn Ad Hoc -> Continue

- #### About Creating a Certificate Signing Request (CSR)
  >利用MAC的钥匙串生成CSR文件

  - In the Applications folder on your Mac, open the Utilities folder and launch Keychain Access.
  Within the Keychain Access drop down menu, select Keychain Access > Certificate Assistant > Request a Certificate from a Certificate Authority.

  - In the Certificate Information window, enter the following information:

    - In the User Email Address field, enter your email address.

    - In the Common Name field, create a name for your private key (e.g., John Doe Dev Key).

    - The CA Email Address field should be left empty.

    - In the "Request is" group, select the "Saved to disk" option.

  - Continue

- #### Generate your certificate
  - 上传生成的CSR文件
  - Generate

## 3、创建 IOS APP IDs
- #### Identifiers -> App IDs -> +
- #### 填写APP ID信息
  - Description（名称）：eg. Hosjoy Comfortable Home APP
  - Prefix（前缀）：eg. SB4MBL4J65 (Team ID)
  - Suffix (后缀)：
    - Explicit App ID（固定）：eg. com.hosjoy.middleman
    - Wildcard App ID（通配）：eg. com.hosjoy.*
  - App Services（需要开通的服务）： 通配模式下服务会比较有限

## 4、创建描述文件Profile
- #### Provisioning Profiles -> Distribution -> +
- #### What type of provisioning profile do you need?
  Distribution -> App Store

- #### Select App ID
  选择上一步生成的APP ID

- #### Select certificates
  选择上面生成的证书

- #### 命名并生成描述文件
  eg. Hosjoy Home APP Profile

## 5、打包ipa
- #### 双击生成的描述文件，导入xcode
- #### 配置xcode
  - TARGENTS -> INFO 里填写项目相关信息，要与前面生成描述文件的信息一致
  - TARGENTS -> Build Settings -> PROVISIONING_PROFILE -> Release 里输入证书的UUID
  （从‘~/Library/MobileDevice/Provisioning Profiles’里查看）
  - Product -> Scheme 里选择Release
- #### 打包
  - Product -> Archive (如果为灰色，插入真机设备再试)

## 6、上传版本至APP STORE
- 使用xcode或者Application Loader


## 7、在iTunes Connect里添加你的APP
- #### iTunes Connect -> 我的APP -> 新建IOS APP
  - 名称：App 在 App Store 中显示的名称
  - 版本：与 Xcode 中所使用的版本号相符
  - 语言：Simplified Chinese
  - SKU：一个独特的、不会在 App Store 中显示的 App ID
  - 套装ID：选择一个前面创建的 APP ID ，必须与 Xcode 中使用的 BundleID 相符
  - 其他：图标（1024jpg）、联系人信息、版权信息、评级等等
  - 版本信息：选择上一步上传到app store的版本

## 8、大功告成，等待审核结果
