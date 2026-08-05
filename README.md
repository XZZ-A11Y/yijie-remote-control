# 易捷远程控制
这是一个开源（目前还没上传，因为还在改）的远程控制软件。拥有多种功能。目前仅支持内网。
---
基于 .NET 8 的 Windows 远程控制系统，集服务端与客户端于一体，支持实时远程桌面、即时通讯、文件传输、系统信息查看等功能。
---
## 📋 专业版 版本说明
本版本为 **专业版**，在基础远程桌面功能之上，新增了专业版集成启动器（RemoteLauncher）、授权管理系统、即时通讯、文件传输、剪贴板同步等多项增强功能。
---
## 官网
``
易捷远程控制官网1(未更新，没余额了）：https://yjyc.netlify.app/

易捷远程控制官网2(已报废）：https://yjyc1.netlify.app/

易捷远程控制官网3(最新版）：https://w99.site/yjyc/index.html

易捷远程控制官网4：https://www.axureshow.com/project/6XKd7K2h/
``
---
### 🆕 专业版新增功能
| 模块 | 功能 | 状态 |
|------|------|:--:|
| **RemoteLauncher** | 集成启动器：一个程序同时包含服务端和客户端 | ✅ |
| **授权系统** | 硬件绑定 + HMAC-SHA256 加密 + 20位授权码 | ✅ |
| **KeyGenerator** | 独立授权码生成工具（支持批量生成） | ✅ |
| **即时通讯** | 控制端与被控端之间实时文字聊天 | ✅ |
| **文件传输** | 控制端向被控端发送文件（最大 50MB） | ✅ |
| **剪贴板同步** | 控制端剪贴板自动同步到被控端 | ✅ |
| **系统信息** | 远程查看 OS/CPU/内存/磁盘/.NET 运行时 | ✅ |
| **延迟检测** | 实时 Ping 往返时间，颜色分级显示 | ✅ |
| **画质调节** | 实时滑块调节 JPEG 画质（10%-90%） | ✅ |
| **帧率调节** | 实时滑块调节帧率（2-20 FPS） | ✅ |
| **启动画面** | 多阶段动画淡入淡出 SplashScreen | ✅ |
| **暗色/亮色主题** | 双主题一键切换 | ✅ |
| **快速连接** | 保存常用连接配置，一键快速连接 | ✅ |
| **连接历史** | 记录每次连接的时间/模式/目标/结果/时长 | ✅ |
| **日志导出** | 连接日志导出 TXT / CSV 格式 | ✅ |
| **截图保存** | 当前远程画面一键保存 PNG/JPEG | ✅ |
| **会话统计** | 状态栏实时 FPS / Ping / 数据量 / 时长 | ✅ |
---
### 通信协议（16 种消息类型）
| 消息类型 | 用途 | 消息类型 | 用途 |
|---------|------|---------|------|
| `Auth` | 账号密码认证 | `ChatMessage` | 即时通讯 |
| `ScreenFrame` | JPEG 屏幕帧 | `FileTransfer` | 文件传输 (≤50MB) |
| `ScreenInfo` | 屏幕尺寸信息 | `ClipboardSync` | 剪贴板同步 |
| `MouseEvent` | 鼠标操作 | `SystemInfoRequest/Response` | 系统信息 |
| `KeyboardEvent` | 键盘操作 | `QualityAdjust` | 画质/帧率调节 |
| `Heartbeat` | 心跳保活 | `ScreenshotCapture` | 截图保存 |
| `Disconnect` | 断开连接 | `PingRequest/Response` | 延迟检测 |
---
## 🚀 技术栈
| 层级 | 技术 |
|------|------|
| 运行时 | **.NET 8.0** |
| UI 框架 | **Windows Forms (WinForms)** |
| 通信 | **TCP Socket + JSON 序列化** |
| 消息格式 | 4 字节长度前缀 + UTF-8 JSON Body |
| 屏幕捕获 | `Graphics.CopyFromScreen()` |
| 键鼠模拟 | `user32.dll` (`mouse_event` / `keybd_event`) |
| 加密 | **HMAC-SHA256**（授权码） + MD5（硬件ID） |
| 压缩 | JPEG（Encoder 可调质量） |
---
## 📁 项目结构（暂未发布）
```
RemoteControl/
├── RemoteControl.sln          # 解决方案
├── RemoteCommon/              # 公共库（协议 + 授权）
│   ├── Protocol.cs            # 16 种消息类型定义
│   └── LicenseManager.cs      # HMAC-SHA256 授权管理
├── RemoteServer/              # 独立服务端（被控端）
│   ├── MainForm.cs            # 账号设置 / TCP 监听 / 系统托盘
│   └── Program.cs
├── RemoteClient/              # 独立客户端（控制端）
│   ├── LoginForm.cs           # IP+端口+账号密码登录
│   ├── RemoteDesktopForm.cs   # 远程桌面显示与控制
│   └── Program.cs
├── RemoteLauncher/            # 专业版集成启动器 🆕
│   ├── MainForm.cs            # 服务端/客户端模式切换 + 主题 + 日志
│   └── SplashScreen.cs        # 启动画面动画
├── KeyGenerator/              # 授权码生成器 🆕
│   └── Program.cs             # 单个/批量生成授权码
├── Setup/                     # 安装程序
├── Uninstaller/               # 卸载程序
├── build.bat                  # 一键编译脚本
├── run_server.bat             # 启动服务端
└── run_client.bat             # 启动客户端
```
---
## 💻 环境要求
- **操作系统**：Windows 10/11（仅支持 Windows）
- **运行时**：.NET 8.0 Desktop Runtime（不需要 SDK）
- **编译**（如需）：.NET 8.0 SDK
  - 下载：https://dotnet.microsoft.com/download/dotnet/8.0
- **管理员权限**：服务端需要管理员权限（屏幕捕获 + 键鼠模拟）
---
## 🎮 使用方法
### 专业版（RemoteLauncher）
| 步骤 | 操作 |
|:--:|------|
| 1 | 运行 RemoteLauncher，启动画面动画后进入主界面 |
| 2 | **激活**：输入 20 位授权码完成激活（首次使用，请联系制作者QQ：3949192157**免费索取**授权码） |
| 3 | **服务端模式**：设置账号密码 → 启动服务，等待控制端连接 |
| 4 | **客户端模式**：输入 IP + 端口 + 账号密码 → 连接 |
| 5 | 连接成功后可使用：聊天 💬、文件传输 📁、系统信息 🖥️、截图 📸、Ping 📶 |
---
## ⚠️ 安全说明

| 项目 | 说明 |
|------|------|
| 数据传输 | 账号密码明文传输，**建议仅在局域网使用** |
| 公网使用 | 务必配合 **VPN** 或 **SSH 隧道** |
| 授权码 | HMAC-SHA256 加密，硬件绑定防伪造 |
| 授权存储 | Base64 编码，非明文保存 |
| 文件传输 | 仅支持控制端 → 被控端单向，限制 50MB |
---
## 📊 性能指标（参考值）
| 指标 | 默认值 | 可调范围 |
|------|:--:|:--:|
| 帧率 | 10 FPS | 2-20 FPS |
| JPEG 画质 | 40% | 10%-90% |
| 单帧带宽 | ~50-200KB | 取决于画质和屏幕内容 |
| 心跳间隔 | 5s | 固定 |
| 最大消息 | 10MB | 固定 |
---
## 🐛 已知问题（Beta）
- [ ] 仅捕获主屏幕（`Screen.PrimaryScreen`），多显示器支持待开发
- [ ] 账号密码明文传输（后续计划加入 TLS 加密）
- [ ] 文件传输仅支持单向（控制端 → 被控端）
- [ ] 不支持剪贴板文件粘贴（仅支持文本）
- [ ] 不支持声音传输
- [ ] 移动端（Android/iOS）尚未实现
---
## 📄 许可证    © 2026 易捷教育（小猪猪教育）。保留所有权利。
---
## 常见问题
<details>
<summary><b>Q: 连接提示"目标计算机积极拒绝"？</b></summary>
检查服务端是否已启动、IP 地址和端口是否正确、防火墙是否放行。
</details>
<details>
<summary><b>Q: 鼠标键盘操作不生效？</b></summary>
服务端必须**以管理员权限运行**。
</details>

<details>
<summary><b>Q: 画面卡顿/延迟高？</b></summary>

降低画质（Quality）或帧率（FPS Delay），在网络较差环境下使用低画质低帧率。
</details>

<details>
<summary><b>Q: 授权码提示无效？</b></summary>

确保授权码与当前机器的硬件 ID 匹配，且未超过过期日期。硬件变更（如重装系统）会导致硬件 ID 变化，需要重新申请授权码。
</details>

