# Readme for ali_ddns

## 概述

1. 本插件用于启动和结束ali_ddns
2. 该插件仅适用于使用阿里云作为DNS服务提供商的域名
3. 安装本插件后，可直接实现DDNS，不依赖华硕路由器固件中DDNS客户端
4. 当路由器通过PPPoE方式拨号上网时，若获得的IP地址为私有IP，则自动禁用DDNS服务，并向指定邮箱发送提醒邮件

## 安装前提

1. 必须安装并启用entware、monit和aliyun

## 文件结构

`ASUS_ROUTER/script_bootloader/usr/ali_ddns/`

| 权限      | 名称      | 属性     | 说明             |
| --------- | --------- | -------- | ---------------- |
| rwxrwxrwx | README.md | 普通文件 | 说明文件         |
| rwxrwxrwx | bin       | 目录     | 可执行文件目录   |
| rwxrwxrwx | etc       | 目录     | 配置文件目录     |
| rwxrwxrwx | tmp       | 目录     | 临时文件目录     |
| rwxrwxrwx | usr       | 目录     | 外部软件资源目录 |

`ASUS_ROUTER/script_bootloader/usr/ali_ddns/bin/`

| 权限      | 名称                     | 属性     | 说明                           |
| --------- | ------------------------ | -------- | ------------------------------ |
| rwxrwxrwx | ali_ddns_install.service | 普通文件 | 安装文件                       |
| rwxrwxrwx | ali_ddns_enable.service  | 普通文件 | 插件的可执行程序，用于启动程序 |
| rwxrwxrwx | ali_ddns_disable.service | 普通文件 | 插件的可执行程序，用于结束程序 |

`ASUS_ROUTER/script_bootloader/usr/ali_ddns/etc/`

| 权限      | 名称             | 属性     | 说明            |
| --------- | ---------------- | -------- | --------------- |
| rwxrwxrwx | monit.d/ali_ddns | 普通文件 | monit.d配置文件 |

`ASUS_ROUTER/script_bootloader/usr/ali_ddns/tmp/`

| 权限      | 名称     | 属性     | 说明                       |
| --------- | -------- | -------- | -------------------------- |
| rwxrwxrwx | mail.tmp | 普通文件 | 使用curl发送邮件时自动生成 |

`ASUS_ROUTER/script_bootloader/usr/ali_ddns/usr/`

| 权限      | 名称                 | 属性     | 说明                           |
| --------- | -------------------- | -------- | ------------------------------ |
| rwxrwxrwx | NO_PUBLIC_IP_ADDRESS | 普通文件 | 通知邮件文件（用于无公网IP时） |

## 安装方法

执行`/tmp/mnt/ASUS_ROUTER/script_bootloader/usr/ali_ddns/bin/ali_ddns_install`

## 调用方法

| 插件文件                 | 插件调用者       |
| ------------------------ | ---------------- |
| ali_ddns_enable.service  | monit.d/ali_ddns |
| ali_ddns_disable.service | monit.d/ali_ddns |

## 需修改部分

`ali_ddns/bin/ali_ddns_enable.service`

| 行号 | 代码                         | 说明                                                                   |
| ---- | ---------------------------- | ---------------------------------------------------------------------- |
| 40   | `ACCESS_KEY_ID=""`           | 你的ACCESS_KEY_ID                                                      |
| 41   | `ACCESS_KEY_SECRET=""`       | 你的ACCESS_KEY_SECRET                                                  |
| 47   | `DOMAIN_NAME=""`             | 你的主域名（假设你的域名是`www.example.com`，则主域名是`example.com`） |
| 48   | `RR_KEY_WORD=""`             | 你的子域名（假设你的域名是`www.example.com`，则子域名是`www`）         |
| 64   | `MAIL_SMTP_SERVER="smtp://"` | 发件人的SMTP服务器地址                                                 |
| 68   | `MAIL_PASSWORD=""`           | 发件人密码                                                             |
| 72   | `MAIL_FROM=""`               | 发件人邮箱地址                                                         |
| 76   | `MAIL_TO=""`                 | 收件人邮箱地址                                                         |

`ali_ddns/bin/ali_ddns_disable.service`

| 行号 | 代码                   | 说明                                                                   |
| ---- | ---------------------- | ---------------------------------------------------------------------- |
| 15   | `ACCESS_KEY_ID=""`     | 你的ACCESS_KEY_ID                                                      |
| 16   | `ACCESS_KEY_SECRET=""` | 你的ACCESS_KEY_SECRET                                                  |
| 22   | `DOMAIN_NAME=""`       | 你的主域名（假设你的域名是`www.example.com`，则主域名是`example.com`） |
| 23   | `RR_KEY_WORD=""`       | 你的子域名（假设你的域名是`www.example.com`，则子域名是`www`）         |