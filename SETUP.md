# 远程桌面配置指南

## 选择远程桌面类型

1. 转到仓库的 "Actions" 标签页
2. 选择 "Remote Desktop Selector" 工作流
3. 点击 "Run workflow"
4. 在配置面板中选择：

### 基本选项
- **远程桌面类型** (rdp_type)：
  * linux-xfce - Linux with XFCE (推荐)
  * linux-mate - Linux with MATE
  * linux-lxde - Linux with LXDE
  * windows-rdp - Windows with Cloudflared
  * ngrok-windows - Windows with Ngrok

### Linux 专用选项
(仅当选择 Linux 类型时有效)
- **操作系统** (os_type)：
  * ubuntu - Ubuntu 最新版
  * debian - Debian 最新版
  * fedora - Fedora 最新版
  * centos - CentOS 最新版

- **浏览器** (browser)：
  * firefox - Firefox
  * chrome - Google Chrome
  * chromium - Chromium

### 通用选项
- **密码**：自定义远程桌面密码（默认：P@ssw0rd!）
- **运行时间**：60-360 分钟（默认：360）

## 推荐配置

### Linux 开发环境：
- 远程桌面类型：linux-xfce
- 操作系统：ubuntu
- 浏览器：firefox

### Windows 环境：
- 远程桌面类型：windows-rdp（国外推荐）
- 或 ngrok-windows（国内推荐）

## 注意事项
1. 不同类型的远程桌面使用不同的连接方式
2. Linux 环境提供更多自定义选项
3. Windows 环境提供更好的兼容性
4. 使用完毕后记得停止工作流