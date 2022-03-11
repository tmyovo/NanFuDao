# 奕辅导 Quantumult X 脚本
>理论上支持Quantumult X, Surge, Loon. 仅在Quantumult X上测试。

  - [**！免责声明！**](#免责声明)
  - [说明](#说明)
    - [2022-02-09更新](#2022-02-09更新)
  - [使用方法（一）软件自动抓包（推荐）](#使用方法一软件自动抓包推荐)
    - [0. 手动完成当天的打卡](#0-手动完成当天的打卡)
    - [1. Quantumult X](#1-quantumult-x)
    - [2. 配置QuantumultX](#2-配置quantumultx)
    - [3. 自动抓取数据](#3-自动抓取数据)
    - [4. 运行一次脚本以获取打卡数据](#4-运行一次脚本以获取打卡数据)
    - [5. 自动运行（可选）](#5-自动运行可选)
    - [6. 正常运行](#6-正常运行)
    - [7. 更新数据](#7-更新数据)
  - [使用方法（二）手动抓包](#使用方法二手动抓包)
  - [多账户打卡](#多账户打卡)
  - [Todo](#todo)

## **！免责声明！**

- **不要瞒报疫情！** 请严格遵守疫情防控规定、法律法规、校规。
- 产生的一切问题和后果，使用者自行负责，与作者无关。
- 仅供编程学习交流，不得用于非法、盈利、商业等用途！请在下载后24小时内删除。

## 说明

### 2022-02-09更新

- 理论上支持Surge, Loon, 但未测试

## 使用方法（一）软件自动抓包（推荐）

### 0. 手动完成当天的打卡
正常在小程序完成打卡

### 1. Quantumult X

需要外区apple store账号购买并安装Quantumult X，（共享账号无法使用rewrite和mitm）

注意要启动mitm证书。

### 2. 配置QuantumultX

#### 添加rewrite重写规则：

（用途：点击健康打卡时，获取`accessToken`）
```
 脚本: yfd_checkin.js

 类型: script-request-header

 url: https://yfd.ly-sky.com/ly-pd-mb/form/api/healthCheckIn/client/stu/index
```


#### 添加mitim主机名：
```
 yfd.ly-sky.com
```

#### Quantumult X配置文件参考示例
```
[rewrite_local]
#重写规则，点击健康打卡时，自动获取accessToken和User-Agent
https://yfd.ly-sky.com/ly-pd-mb/form/api/healthCheckIn/client/stu/index url script-request-header https://raw.githubusercontent.com/uiolee/NanFuDao/main/yfd_checkin.js

[mitm]
hostname = yfd.ly-sky.com

#task_local规则，每天定时自动执行脚本
[task_local]
5 0 * * * https://raw.githubusercontent.com/uiolee/NanFuDao/main/yfd_checkin.js, tag=奕辅导, enabled=true
```

### 3. 自动抓取数据

配置好rewrite和mitm，启动QuantumultX

小程序中点击健康打卡图标，脚本会获取`accessToken`和`UA`并保存。

### 4. 运行一次脚本以获取打卡数据
（无需rewrite和mitm，无需开启Quantumult X）
在`accessToken`配置正确且已完成完成手动打卡的情况下，手动执行脚本即可获取打卡所需要提交的数据。

### 5. 自动运行（可选）

- 配置QuantumultX的定时任务，即可定时运行脚本（需要保持QuantumultX后台运行）

- 通过ios的快捷指令，并设置自动化快捷指令，实现定时运行（推荐）（需将脚本下载到本地目录/Qauntumult X/scripts/）

### 6. 正常运行

- 当脚本可以正常运行后，建议关闭rewirte和mitm，避免影响正常使用。

### 7. 更新数据
- 当`accessToken`失效时，重新执行步骤[3. 自动抓取数据](#3-自动抓取数据)。
- 当问卷问题失效时，配置脚本中第43行，[yfd_checkin.js#L43](./yfd_checkin.js#L43)为true，重新执行步骤[4. 运行一次脚本以获取打卡数据](#4-运行一次脚本以获取打卡数据)。
```JavaScript
//###########	Config		####################

var clear_data = false;	//当为true时，清除已保存的打卡数据，重新获取。默认值应为false
```

## 使用方法（二）手动抓包

需要自己学习抓包。如无特殊需要不建议使用。

适合调试使用或者无rewrite和mitm，或者自定义打卡数据。

该方法相当自由，可以手动配置`accessToken`和`UA`，让脚本自动获取打卡数据。

也可以全部数据都手动配置。

有需要的人自行折腾，这里不赘述。

## 多账户打卡

本脚本的初衷是自用，不会考虑加入多账户打卡的功能。

不过笔者可以提供一个简单的思路，有需要的人自行折腾

1. 将脚本复制多份。
2. 修改每份脚本的三个全局常量，以此类推，每份脚本不重复即可。
```JavaScript
const token = "yfd_accessToken_1";
const UA = "yfd_User-Agent_1";
const data = "yfd_checkin_data_1";
```
3. 通过自动抓包或手动抓包的方式，配置每个账号的数据。

## 致谢

参考了[@NobyDa](https://github.com/NobyDa)和[@chavyleung](https://github.com/chavyleung)的环境封装函数。

## Todo

 - [x] 实现自动抓取“已打卡”的数据，而不是手动打卡的数据
 - [x] 适配Surge, Loon
 - [ ] 生成一定范围内的随机位置数据，避免被识别

 [回到顶部](#readme)

----------------

# 模拟假条

1. 务必遵守疫情防控、法律法规、校规！
2. 至少正常请假一次，本脚本才能运行。
3. 脚本没有对服务器上的数据进行操作。
4. 仅供交流学习，请在下载后24小时内删除。一切责任由使用者自负，与作者无关。

- `applyList.js`    假条列表
- `detail.js`    假条详情

Quantumult X 配置参考，其他软件类似

[rewrite]
```
脚本: applyList.js
类型: script-response-body
url: https://yfd.ly-sky.com/ly-ms/application/api/oa/applyList
```
```
脚本: detail.js
类型: script-response-body
url: https://yfd.ly-sky.com/ly-ms/application/api/oa/detail/*
```
[Mitm]
```
主机名: yfd.ly-sky.com
```

[回到顶部](#)
