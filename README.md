<div align="center">
<h1> 📺NVIDIA-SHILED_Frp </h1>
</div>

## ⚠️ 注意
 - 项目仅用于编写教程和相关提醒，不提供任何“二级”域名或服务器，仅限但不包括配置文件
 - 仅用于个人学习，禁止进行获利、筹钱等非法活动
 - 仅适应大带宽的服务器，以及大宽带的家庭网络
 - 会消耗大量的，无法预知的流量，请谨慎使用

---

## 🔗 项目引用
[![](./img/Moonlight.png)](https://github.com/moonlight-stream) [![](./img/FRP.png)](https://github.com/fatedier/frp)
##### * `Moonlight` 仅连接端（NVIDIA SHILED）

---

## 🔧 配置Frp相关
###  一、服务端（Frps）
1. 配置Frp服务端前，去Frp仓库下载对应你服务器系统或架构的包（以下都以 Liunx X86_64 为例）
> 例如服务器是 `Liunx` 系统，`X86_64` 架构，则下载 [`frp_0.38.0_linux_amd64.tar.gz`](https://github.com/fatedier/frp/releases/download/v0.38.0/frp_0.38.0_linux_amd64.tar.gz)
##### * 注： 父级目录建议重命名为好记的名称
```ini
# 目录结构
frp_0.38.0_linux_amd64
  ├─systemd
  │    ├─frpc.service
  │    ├─frpc@.service
  │    ├─frps.service
  │    └─frps@.service
  ├─frpc            # Frp客户端
  ├─frpc.ini        # Frp客户端配置文件
  ├─frpc_full.ini
  ├─frps            # Frp服务端
  ├─frps.ini        # Frp服务端配置文件
  ├─frps_full.ini
  └─LICENSE
```

2. 服务端仅需要 `frps` ，即 `frps.ini` 配置文件。
    - `frps.ini` 可以更改为任意名称但运行时，必须也要以定义的名称运行
    - 请不要删除任何关于 `frps` 的任何文件，否则将无法正常运行
    - 示例如下：
```ini
[common]
# [common] 部分为 Frps 运行的主要配置
bind_addr = 0.0.0.0
# 此为 Frps 服务端监听的 IP (此部分不建议使用 127.0.0.1 并使用宝塔进行反代，不然会出现各种无法预料的 BUG)
bind_port = 7650
# Frps 主端口，用于服务端与客户端进行通信的端口
token = 'xxxxx'
# Frps 建立连接时的验证密钥（如果客户端请求的密钥不正确，那么连接将会被阻断）

enable_prometheus = true
dashboard_addr = 127.0.0.1     # 管理员面板的监听地址（建议使用 127.0.0.1 并使用宝塔反代）
dashboard_port = 23556         # 管理员面板的端口
dashboard_user = xxxxx         # 管理员用户名
dashboard_pwd = 'xxxxx'        # 管理员密码

# === [common] Frps 穿透各协议配置 ===
vhost_http_port = 8086
# Frp HTTP 穿透监听端口
# 如果你在用 BT 控制面板并安装相关Web环境，请不要设置为 80 端口，因为 Nginx 或其他环境占用 80 端口
vhost_https_port = 8087
# Frp HTTPS 穿透监听端口
# 如果你在用 BT 控制面板并安装相关Web环境，请不要设置为 443 端口，因为 Nginx 或其他环境占用 443 端口
custom_404_page = '/home/frp_0.38.0_linux_amd64/pages/404.html'
# HTTP/HTTPS 隧道错误 (404) 页面文件，pages 为新创建文件夹
```

3. 请检查Frp父级目录的权限，使用 `chmod` 更改权限为 `755` | 命令：
```shell
chmod 755 -R /home/frp_0.38.0_linux_amd64
```

4. 服务端运行需要 `screen` 创建后台窗口，创建命名为 `Frps` 的窗口 | 命令：
```shell
screen -S frps
```
  - 第一步：`cd` 到Frp的目录（ `ls` 目录有 `frps` 二进制执行文件）
  - 第二步：编辑好 `frps.ini` 后，执行运行命令：
  - ```shell
    ./frps -c ./frps.ini
    ```

5. 至此，服务端已配置完成。

### 二、客户端（Frpc）
1. 配置Frp客户端前，去Frp仓库下载对应你服务器系统或架构的包
> 因为远程被控制端只有 `Windows` 才有支持 **`NVIDIA SHILED`**，所以只需要下在Windows端（以下示例环境 `Windows X86_64`）<br /> 
客户端 -> [frp_0.38.0_windows_amd64.zip](https://github.com/fatedier/frp/releases/download/v0.38.0/frp_0.38.0_windows_amd64.zip)

##### * 注： 父级目录建议重命名为好记的名称
```ini
# 目录结构
frp_0.38.0_windows_amd64
  ├─systemd
  │    ├─frpc.service
  │    ├─frpc@.service
  │    ├─frps.service
  │    └─frps@.service
  ├─frpc.exe        # Frp客户端
  ├─frpc.ini        # Frp客户端配置文件
  ├─frpc_full.ini
  ├─frps.exe        # Frp服务端
  ├─frps.ini        # Frp服务端配置文件
  ├─frps_full.ini
  └─LICENSE
```

2. 运行前请添加Windows Defender的文件夹排除名单

#### **`病毒和威胁防护` > `病毒和威胁防护设置[管理设置]` > `排除项[添加或删除排除项]` > `添加排除项`**  

3. 客户端仅需要 `frpc` ，即 `frpc.ini` 配置文件。
    - `frpc.ini` 可以更改为任意名称但运行时，必须也要以定义的名称运行
    - 请不要删除任何关于 `frpc` 的任何文件，否则将无法正常运行
    - 示例如下：
```ini
[common]
# [common] 部分为 Frpc 运行的主要配置
server_addr = 0.0.0.0  # 此为 Frp 客户端的域名或者 IP
server_port = 7650     # 此为 Frp 客户端设置的端口
token = 'xxxxx'        # Frp 客户端所设置的鉴权密钥

admin_addr = 127.0.0.1 # Frp 客户端 WebUI 监听地址
admin_port = 23554     # Frp 客户端 WebUI 监听端口

# ===================
# 以下为隧道列表
# ===================
[test]
type = tcp
local_ip = 0.0.0.0
local_port = 12345
remote_port = 12345
```

4. **`NVIDIA SHILED`** 客户端配置如下：
```ini
[common]
server_addr = 0.0.0.0   # 服务端IP或域名
server_port = 7650      # 服务端目标端口
token = 'xxxxx'         # 服务端建立连接时的验证密钥

# ================
# 以下内容不要更改
# ================
[nvidia-stream-tcp-1]
type = tcp
local_ip = 127.0.0.1
local_port = 47984
remote_port = 47984

[nvidia-stream-tcp-2]
type = tcp
local_ip = 127.0.0.1
local_port = 47989
remote_port = 47989

[nvidia-stream-tcp-3]
type = tcp
local_ip = 127.0.0.1
local_port = 48010
remote_port = 48010

[nvidia-stream-udp-1]
type = udp
local_ip = 127.0.0.1
local_port = 5353
remote_port = 5353

[nvidia-stream-udp-2]
type = udp
local_ip = 127.0.0.1
local_port = 47998
remote_port = 47998

[nvidia-stream-udp-3]
type = udp
local_ip = 127.0.0.1
local_port = 47999
remote_port = 47999

[nvidia-stream-udp-4]
type = udp
local_ip = 127.0.0.1
local_port = 48000
remote_port = 48000

[nvidia-stream-udp-5]
type = udp
local_ip = 127.0.0.1
local_port = 48002
remote_port = 48002

[nvidia-stream-udp-6]
type = udp
local_ip = 127.0.0.1
local_port = 48010
remote_port = 48010
```

5. 运行 `frpc.exe` | 命令(PowerShell)：
```ps
.\frpc.exe -c ./frpc.ini
```

6. 至此，客户端已配置完成。

---

## ⚙️ **NVIDIA GeForce Experience** 配置
- 检查是否是 `Game Ready` 驱动程序，不是请切换至此驱动
- 依次点击设置 > SHILED ，打开 `GAMESTREAM` 开关（如果没有请不要继续）
- 建议创建一个单向软件，以防 `moonlight` 检查不到你要打开的软件会自动关闭，比如 *explorer* .

---

## 🕹️ **Moonlight** 连接
 - 安卓端/PC端/Mac端（Intel / M1）
   - 支持外网ip或域名
   - 带宽不建议高于家庭上行速率，40 Mbps 即可
 - iOS端/iPadOS端
   - 很遗憾，Apple官方不允许moonlight添加外网设备<br />
   > [**Moonlight issues #417** @690064177](https://github.com/moonlight-stream/moonlight-ios/issues/417#issuecomment-690064177)

   [cgutman](https://github.com/cgutman) :<br />
   Apple's [App Store Review Guideline 4.2.7a](https://developer.apple.com/app-store/review/guidelines/#design) states: 
   > The app must only connect to a user-owned host device that is a personal computer or dedicated game console owned by the user, and both the host device and client must be connected on a local and LAN-based network.

   Therefore, Moonlight does not allow you to add PCs that aren't on your local network anymore. This was a hard requirement by Apple's App Store Review team.
   - 但是issues有提示iPv4映射成iPv6的方法<br />
   > [**Moonlight issues #417** @1001257949](https://github.com/moonlight-stream/moonlight-ios/issues/417#issuecomment-1001257949)

   [U-siro](https://github.com/U-siro) :<br />
   I found a alternate method to bypass this limitation.
   Use a IPv6 address: if your ip is 1.1.1.1 input ::ffff:1.1.1.1
   Then it will added without any vpn.

   https://www.ibm.com/docs/en/zos/2.4.0?topic=addresses-ipv4-mapped-ipv6

---

## 🛠️ 更新日志
 - 2022/1/22
    - 1.更新md文档

---

## 🌎 MIT LICENSE
```
MIT License

Copyright (c) 2021 ReaJason

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
