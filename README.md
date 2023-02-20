<div align="center">
<h1 align="center">
Auto-GongXueYun
</h1>
<p align="center">
🥰工学云自动打卡解决方案🥰
</p>
<p align="center">
支持多用户、自定义位置信息、保持登录状态、每日打卡检查
</br>
打卡位置浮动、自定义UA、微信消息推送、自动填写周报（TODO）
</br>
</p>
</br>
<a target="_blank" href="https://www.bilibili.com/video/BV1VP4y117w8/?spm_id_from=333.999.0.0&vd_source=23b414916f0ead82eaa42a85d58614c8">视频教程</a>
</div>
</br>



## 前言

**1、请务必认真阅读此文档后继续！**

**2、本项目开源&免费，所有开发均仅限于学习交流，禁止用于任何商业用途。**

**3、在开始之前请务必帮我点一下右上角的star。**

**4、如基于或参考此项目进行二次开发，请注明原作者并使用GPL2.0许可证开源**


</br>


## 目录

- [前言](#前言)
- [使用门槛](#使用门槛)
- [使用方法](#使用方法)
  - [Github Actions](#github-actions)
  - [百度云函数部署](#百度云函数部署)
  - [使用自己的服务器部署](#使用自己的服务器部署)
- [启用保持登录](#启用保持登录)
- [启用每日打卡检查](#启用每日打卡检查)
- [启用打卡位置浮动](#启用打卡位置浮动)
- [修改自动打卡时间🎯](#修改自动打卡时间)
- [赞助支持](#赞助支持)
- [常见问题](#常见问题)
  - [打卡失败](#打卡失败)
  - [Actions运行错误](#Actions运行错误)
  - [增加打卡检查运行次数](#增加打卡检查运行次数)
  - [上班及下班卡判断逻辑](#上班及下班卡判断逻辑)
  - [暂停自动打卡](#暂停自动打卡)
  - [修改配置文件](#修改配置文件)
  - [周报日报功能](#周报日报功能)
  - [保持最新代码](#保持最新代码)

</br>
</br>

## 使用门槛

帮我点一下右上角的star（星星）



</br></br>

## 使用方法

### Github Actions

推荐指数：⭐⭐⭐⭐⭐


优点：适合没有自己服务器的人使用。


缺点：

1、每日打卡时间无法保证十分准时，拥有10-30分钟的误差。

2、Action在某些情况下可能会连接工学云服务器超时造成打卡失败(已加入每日打卡检查，当未打卡时会补签)。

</br>

1.点击Star后Fork本仓库🤪

![1.png](https://tc.xuanran.cc/2022/11/13/1932d085c97c2.png)
![2.png](https://tc.xuanran.cc/2022/11/13/c6e9abebb113c.png)
</br>
2.准备配置文件🤔

如果想同时打卡多个用户,请再添加一个数据体就好了([如果还不理解点我](https://github.com/XuanRanDev/Auto-GongXueYun/wiki))

**注意：配置文件模板下方有配置含义，请务必参照配置含义填写**
```json
[
  {
    "enable": true,
    "phone": "17666666666",
    "password": "1111111111",
    "keepLogin": false,
    "token": "如果keepLogin为false就不填",
    "userId": "如果keepLogin为false就不填",
    "planId": "如果keepLogin为false就不填",
    "randomLocation": true,
    "user-agent": "null",
    "signCheck": false,
    "country": "中国",
    "province": "河南省",
    "city": "洛阳市",
    "area": "新安县",
    "desc": "打卡备注",
    "type": "android",
    "address": "你的详细地址",
    "longitude": "101.987848",
    "latitude": "33.100038",
    "pushKey": "dhsajifysfsafsdfdsxxxxxx"
  }
]

```

其配置含义如下：

| 参数名称       | 含义                                                         |
| -------------- | ------------------------------------------------------------ |
| enable         | 是否启用该用户的打卡（true或false)                           |
| phone          | 手机号                                                       |
| password       | 密码                                                         |
| keepLogin      | 是否启用保持登录，启用后程序每次打卡将不再重新登录，避免挤掉手机工学云的登录，启用后请通过抓包工学云获取你的token、userId、planId然后填写在下方 |
| token          | 如果keepLogin启用，则在此填写你的token                       |
| userId         | 如果keepLogin启用，则在此填写你的userId                      |
| planId         | 如果keepLogin启用，则在此填写你的planId                      |
| randomLocation | 是否启用打卡位置浮动，启用后每次打卡会在原有位置基础上进行位置浮动 |
| user-agent     | 是否自定义UA，如果不需要自定义填null（字符串形式），否则填写你的UA（[可以点我随便选一个](https://wp.xuanran.cc/s/JBTA)） |
| signCheck      | 每日签到检查,某些情况下action可能没网络造成打卡失败,启用此选项后,将在每日11点以及23点检查今日的打卡状态,如未打卡则补卡 |
| country        | 国家                                                         |
| province       | 省份                                                         |
| city           | 城市                                                         |
| area           | 区/县                                                        |
| desc           | 打卡备注                                                     |
| type           | android 或 ios                                               |
| address        | 详细地址，如果你打卡的时候中间带的有·这个符号你也就手动加上，这里填什么，打卡后工学云就会显示你填的内容（工学云默认·这个符号左右都会有一个空格） |
| longitude      | 打卡位置精度,通过坐标拾取来完成(仅需精确到小数点后6位)，[传送门](https://jingweidu.bmcx.com/) |
| latitude       | 打卡位置纬度,通过坐标拾取来完成(仅需精确到小数点后6位)，[传送门](https://jingweidu.bmcx.com/) |
| pushKey        | 打卡结果微信推送，微信推送使用的是pushPlus，请到官网绑定微信([传送门](https://www.pushplus.plus/))，然后在发送消息里面把你的token复制出来粘贴到pushKey这项 |



</br>

3.配置Secret

填写完成后请复制如上配置文件，然后打开仓库的Settings->Secrets->Actions->New repository secret

Name填USERS

Secret填改好的配置文件

![3.png](https://tc.xuanran.cc/2022/11/13/2143b390f8199.png)
![4.png](https://tc.xuanran.cc/2022/11/13/8de9cb85e479b.png)

4.运行测试
![5.png](https://tc.xuanran.cc/2022/11/13/500e789b3dfec.png)
![6.png](https://tc.xuanran.cc/2022/11/13/1366e5e0ced97.png)
![7.png](https://tc.xuanran.cc/2022/11/13/2a2b4b7e01884.png)
![8.png](https://tc.xuanran.cc/2022/11/13/bd1cd3218f77a.png)
![9.png](https://tc.xuanran.cc/2022/11/13/33c6cec2e37ec.png)
![10.png](https://tc.xuanran.cc/2022/11/13/a9e80f17d304b.png)
</br></br></br></br>

至此，自动打卡将会在每天7点和18点左右自动运行打卡。😉

</br></br></br>


### 百度云函数部署

以下教程来自：[@96368a](https://github.com/96368a)

目前仅测试了百度云函数，其他云厂商请自行测试

> tips: 腾讯云、阿里云函数不用试了，IP全黑

首先下载项目

**创建一个user.json配置文件在项目目录，并填写配置文件**

![](https://file.uhsea.com/2301/1087d34db474a3b99d01005e93333ba7D9.png)

登录百度云后台创建云函数，模块选空白

![](https://file.uhsea.com/2301/c31d59dd65e759edd802e3a300bb2f4fDX.png)

运行时选择python3.6

![](https://file.uhsea.com/2301/a43eca4387f564982a9e9a278c94aaf5CI.png)

配置定时触发器，表达式[参考这里](#修改自动打卡时间)

函数基本信息里可以添加多个触发器，上班下班需要分别配置

进入代码编辑，选择上传函数zip包，然后把代码打包上传即可




### 使用自己的服务器部署

推荐指数：⭐⭐

优点：运行稳定、准时。

缺点：有一定的上手成本。

> 目前已知腾讯云、阿里云以及部分百度云的服务器、云函数IP被工学云拉黑，如果你使用以上云服务商产品不用再费时间了，华为云目前可用。



具体教程：

1、下载本仓库源码到你服务器。

2、在服务器中安装好Python环境。

3、在百度搜索：你的操作系统+ 定时任务，查看如何创建定时任务。

4、创建一个user.json配置文件在项目目录，并将配置文件放入其中

5、运行python main.py测试

</br></br></br>




## 启用保持登录
启用保持登录指的是打卡程序在启用保持登录开启后不再使用账号密码登录打卡，而是使用Token方式。

启用保持登录需要会抓包，而且要抓https的包，如果你手机没Root就别想了

1、下载小黄鸟，抓包软件选择工学云

2、打开工学云

3、在抓包软件里找到https://api.moguding.net/attendence/clock/v1/listSynchro
这个请求

4、找到Token以及userId、planId
![11](https://tc.xuanran.cc/2022/11/13/520b118fe371a.jpg)
![12](https://tc.xuanran.cc/2022/11/13/ec400140df3d8.jpg)



</br></br>


## 启用每日打卡检查
因为Github Action某些情况下可能会无法连接工学云服务器造成打卡+推送失败问题，为此推出每日打卡检查，启用后，每日11点（有1次检查）以及23点（也有1次检查）将会对今日打卡情况进行分析，如存在漏签则自动补签。

如需开启，请在配置文件中的signCheck设置为true
</br></br>


## 启用打卡位置浮动
启用打卡位置浮动后，每次打卡系统会在原有经纬度中删掉最后一位数字，并随机加入一位数字，使每次打卡经纬度不同。
</br></br>

## 修改自动打卡时间🎯	

修改自动打卡时间需要了解Cron表达式的使用，且需注意不要将打卡时间设置为11点以及23点，此时间段中会运行每日打卡检查，自动打卡在此时间段内不生效。😴


**修改打卡时间不要开浏览器翻译！**


</br>
1.编辑sign.yml文件，找到图中我圈出的部分

![image-20221021093411661](https://tc.xuanran.cc/2022/11/10/5d81dcc0bff46.png)

</br>

2.编辑表达式

GitHub的cron表达式不支持精准到秒，所以从最左边开始，分别为：

分钟 小时 日 月份 星期

而且Github的服务器时间会比我们晚八个小时，所以在你需要打卡的时间-8配置到里面就行了

</br>

例如说在每天上午十点打上班卡就是:

```yml
- cron: "00 02 * * *"
```


周一到周五晚上九点打下班卡：

```yml
- cron: "00 13 * * 1-5"
```



</br>
</br>





## 赞助支持

如果此仓库帮助了你学到了新知识，你可以帮我买杯可乐。

![赞助支持](https://tc.xuanran.cc/2022/11/20/b8f5ddc944634.png)



## 常见问题


### 打卡失败 

如果遇到action运行失败或者打卡失败，99%都是Github无法连接工学云服务器(错误信息中含有Network字样)，属于玄学问题，猜测是服务器IP被拉黑，这点没很好的处理办法，只能重试，如打卡十分重要可适当增加每日打卡检查次数。


### Actions运行错误
脚本已经对除了解析配置文件失败外所有的异常进行捕获，如果你运行后图标是一个×，99%都是解析配置文件错误，这种情况下Start Sign里面会显示有此关键字：

json.decoder.JSONDecodeError: Expecting value:xxxxxxxxxxxxxxxxxxx

它代表无法解析配置文件，请仔细检查你的配置文件格式！（大部分人都是拿中文的逗号，当英文的逗号,用！这两个是不一样的，配置文件内除了双引号中的字符可以是中文外其他都是英文！）




### 增加打卡检查运行次数
从1.0版本开始，每日打卡检查从每日运行4次调整为每日2次并默认关闭，具体原因请查看更新日志，如需要调整每日打卡检查运行次数，可先学习修改自动打卡这一小节的基础知识后再来。
默认情况下的sign.yml时间配置如下：
```yml
- cron: "00 23 * * *" # 上班卡
- cron: "15 03 * * *" # 每日签到检查，如不需要可删除这行，其含义为每日11点15运行（存在延时）
- cron: "00 10 * * *" # 下班卡
- cron: "15 15 * * *" # 每日签到检查，如不需要可删除这行，其含义为每日23点15运行（存在延时）
```
**打卡检查仅限在每日11点以及23点运行，其他时间运行会造成多打卡情况出现**，所以如果你需要增加自动打卡的次数就复制第二行或第四行，然后修改具体的时间即可，但请注意分钟必须限制在0-25之间（因action运行20分钟左右的延时，防止action启动后已过11/23点），例如说我想要增加两次打卡检查，则可这样写：
```yml
- cron: "00 23 * * *" # 上班卡
- cron: "15 03 * * *" # 每日签到检查，如不需要可删除这行，其含义为每日11点15运行（存在延时）
- cron: "05 03 * * *" # 每日签到检查，如不需要可删除这行，其含义为每日11点5运行（存在延时）
- cron: "00 10 * * *" # 下班卡
- cron: "15 15 * * *" # 每日签到检查，如不需要可删除这行，其含义为每日23点15运行（存在延时）
- cron: "05 15 * * *" # 每日签到检查，如不需要可删除这行，其含义为每日23点5运行（存在延时）
```

### 上班及下班卡判断逻辑
每日12点以前运行全部为上班，12点及以后默认为下班卡。
</br>

### 暂停自动打卡
在Actions-Sign-右边三个点disable workflow
</br>

### 修改配置文件
Settings-Secrets-Actions-下面Repository secrets有个USERS，点击小箭头编辑，里面没内容是正常的，配置文件一旦保存将无法再被看到。
</br>


### 周报日报功能
周报日报打卡图片功能已做，但不会上线，[原因点我](https://mp.weixin.qq.com/s/ZdOb3VcN4lPs4LgnsPj9bw)，**所以不要在加我说多少多少钱收算法。**
</br>

### 保持最新代码
随着工学云的更新，自动打卡可能会在某个版本后失效，开发者会及时更新代码，但你Fork的代码并无法保证与主分支（我的代码）实时同步，此时需要手动同步代码，需要注意的是Github不会有任何通知告诉你代码过时或有新版发布，为此，你可以扫描下方二维码关注我的公众号，当有重要更新或调整时会在公众号发文提醒。

手动同步代码方法：
![微信截图_20221130142844.png](https://tc.xuanran.cc/2022/11/30/1dad103cbcd0a.png)


公众号：
![e87a1043ea8f4fada3bb99ba8e35767.jpg](https://tc.xuanran.cc/2022/12/02/d1b00d4d20886.jpg)


</br></br>

## Project supported by JetBrains

Many thanks to Jetbrains for kindly providing a license for me to work on this and other open-source projects.

[![](https://resources.jetbrains.com/storage/products/company/brand/logos/jb_beam.svg)](https://www.jetbrains.com/?from=https://github.com/XuanRanDev/Auto-GongXueYun)

