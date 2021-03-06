title:  OTA使用教程
---

# OTA功能概述

OTA 英文全称是Over-the-Air Technology，即空间下载技术。当设备连上云端时会收到OTA升级通知，再通过HTTP完成固件升级。机智云的OTA服务主要提供以下功能：

- OTA通知服务，即离线升级。当设备的固件程序有新版本发布，OTA 通知服务会推送升级通知到设备。
- OTA透传服务，即在线升级。设备固件程序通过M2M 消息服务透传到设备端。
- 支持一个产品同时有多个推送
- 支持wifi/mcu升级
- 支持定向升级。可指定设备mac地址、区域、旧固件版本进行推送。
- 支持定时推送。可自定义推送周期及推送时段。
- OTA进度统计分析

# OTA升级流程

Wifi产品OTA服务是在开发者中心网站上实现的，由5部分组成：分别是添加固件、验证固件、添加规则、开始推送、推送完成（查询结果）。
## 添加固件

如将设备的模组烧写的固件为：GAgent_00ESP826_04020019_16101715.bin，其中硬件版本号为：00ESP826，软件版本号为：04020019
步骤一、进入【服务】固件升级（OTA）模块，点击【创建新固件】

![Alt text](/assets/zh-cn/UserManual/OTA/1479262419091.png)

![Alt text](/assets/zh-cn/UserManual/OTA/1479262439396.png)

步骤二、[下载GAgent OTA固件](http://dev.gizwits.com/zh-cn/developer/resource/hardware?type=GAgent)（MCU固件是开发者开发，若是MCU升级，可跳过该步骤。）

![Alt text](/assets/zh-cn/UserManual/OTA/1479451464257.png)

> **备注：**所有**汉枫WiFi模组** OTA固件必须选择web版本，**ESP 8266 WiFi模组**OTA固件为“非combine文件”。如下图：
> 
![Alt text](/assets/zh-cn/UserManual/OTA/1479452492027.png)

![Alt text](/assets/zh-cn/UserManual/OTA/1479452504123.png)

步骤三、固件信息填写
硬件版本号+软件版本号前 4 个字节 +固件类型完全匹配为一系列固件，软件版本号后4个字节区分固件版本，OTA升级需在同系列中进行。

> - 版本名称：自定义，由英文、数字及下划线组成 
> - 固件类型：支持WiFi/MCU两种方式，选择wifi
> - 推送方式：支持V4V4.1两种方式。推送方式的选择可以参考页面“温馨提示”。
>  **注意：由于目前大部分设备所使用的固件支持v4.1推送方式，本文只讲解v4.1推送方式流程。** 
> - 选择固件：上传需要升级的固件，如：GAgent_00MX3162_04020004.bin（wifi为bin文件，mcu为bin/hex文件）
> - 硬件版本号：目标升级WIFI硬件版本（即上传的），必须为8个字节
> - 软件版本号：目标升级WIFI软件版本（即上传的），必须为8个字节

![Alt text](/assets/zh-cn/UserManual/OTA/1479460809772.png)

![Alt text](/assets/zh-cn/UserManual/OTA/1479460824716.png)

步骤四、 点击完成，此时固件为未验证状态

## 验证固件
出于安全性考虑，未验证通过固件不可进行OTA推送。验证固件不区分OTA版本，流程一致。在大批量升级设备之前，需要选择单台设备进行升级，并自行验证升级后的设备稳定性。若无异常，固件变为已验证状态，表示可以进行批量OTA升级。
验证固件流程如下：

步骤一、再次确认已上传的bin文件及信息填写无误（未验证固件还可编辑）
步骤二、准备测试设备并让其连上云端,保证验证的设备在线。
步骤三、进入未验证固件的固件详情页面，点击【验证固件】，出现如下界面：

![Alt text](/assets/zh-cn/UserManual/OTA/1479263377242.png)

![Alt text](/assets/zh-cn/UserManual/OTA/1479263561546.png)

步骤四、在输入框填写在线测试设备的MAC地址，找到目标设备后进入固件升级倒计时

![Alt text](/assets/zh-cn/UserManual/OTA/1479263999098.png)

步骤五、测试设备成功升级后，出现再次确认界面。**此时，为了谨慎起见，请你对升级成功后的设备做一个稳定性验证，确保升级后的设备能正常工作。如无异常，请手动勾选确认框。**

![Alt text](/assets/zh-cn/UserManual/OTA/1479264019691.png)

## 添加规则

通过添加不同的规则可以实现一个产品同时有多个推送请求，并可设置推送周期及时段，个性化定制推送服务。

步骤一、在固件列表，点击已验证固件名称，进入【固件推送】页面，点击【添加规则】

![Alt text](/assets/zh-cn/UserManual/OTA/1479264270858.png)

步骤二、设置推送条件：支持“指定地区”和“指定MAC”两种推送方式

> - **设置推送条件详解：**
> 1. 指定地址：填入目标推送设备区域，如“广东省-广州市” 或者 指定MAC地址：填入目标推送设备地址，**如有多个换行隔开**
> 2. 指定旧固件版本：选择目标推送设备的旧固件版本 
> 3. 目标设备：取条件1&2的交集，刷新后显示欲推送的目标设备数
> 4. 推送周期（UTC）：设置推送规则有效日期
> 5. 推送时段（UTC）：设置每日推送时段。
> - **备注（重要）:**
> 1. 设备在推送周期内&推送时段内，且在线状态下，机智云将发送OTA推送通知。每日发送OTA推送通知次数为一次。设备收到通知后，主动下载OTA推送固件。
> 2. 设备重新上电，设备都将主动询问机智云是否有推送任务。若符合推送周期&推送时段&目标设备，等推送条件，设备主动下载OTA推送固件。
> 3. UTC ：协调世界时（英：Coordinated Universal Time ，法：Temps Universel Coordonné），又称世界统一时间，世界标准时间，国际协调时间。推送页面中，机智云自动将UTC时间映射为本地（北京）推送时间。

![Alt text](/assets/zh-cn/UserManual/OTA/1479264580175.png)

步骤三、点击【保存】，生成新的规则及唯一的request id

![Alt text](/assets/zh-cn/UserManual/OTA/1479264743271.png)

## 开始推送
步骤一、已成功添加规则，点击对应规则的【开始推送】按钮

![Alt text](/assets/zh-cn/UserManual/OTA/1479264944331.png)

步骤二、勾选相关协议，再次确认。此时升级请求已推送，对应规则状态会改变。
> 备注：目标设备栏，当前升级成功设备数/目标推送设备数

![Alt text](/assets/zh-cn/UserManual/OTA/1479264986004.png)

步骤三、在线设备或离线设备上线后会自动执行OTA升级，升级到最新固件，并將状态上报给云端。

## 推送完成
步骤一、推送完成后，刷新界面，对应规则会变为“已完成”状态

![Alt text](/assets/zh-cn/UserManual/OTA/1479265094443.png)

步骤二、查看明细

点击【查看明细】链接，可查询单个设备升级详情，并可以导出当前所有设备升级情况

![Alt text](/assets/zh-cn/UserManual/OTA/1479265132474.png)
