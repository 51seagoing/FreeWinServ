# Linux 远程桌面连接指南

## 支持的系统
1. Ubuntu Latest
   - 最新的 Ubuntu LTS 版本
   - 完整的 XFCE4 桌面环境
   - 最佳兼容性和稳定性

2. Debian Latest
   - 稳定的 Debian 系统
   - 类似 Ubuntu 的体验
   - 适合长期使用

3. Fedora Latest
   - 最新的 Fedora 版本
   - 更新的软件包
   - 适合开发使用

4. CentOS Latest
   - 企业级 Linux 系统
   - 稳定性优先
   - 适合服务器环境

## 选择系统
1. 在 GitHub Actions 页面：
   - 点击 "Run workflow"
   - 从下拉菜单选择系统类型：
     * ubuntu
     * debian
     * fedora
     * centos
   - 点击绿色的 "Run workflow" 按钮

## 系统特点对比

### Ubuntu/Debian
- 优点：
  * 最佳软件兼容性
  * 完整的包管理
  * 稳定的桌面体验
- 缺点：
  * 可能不是最新软件版本

### Fedora
- 优点：
  * 最新的软件包
  * 现代化的系统组件
  * 良好的开发工具支持
- 缺点：
  * 可能存在稳定性问题

### CentOS
- 优点：
  * 企业级稳定性
  * 长期支持
  * 安全性优先
- 缺点：
  * 软件包可能较旧
  * 桌面体验相对简单

## 系统信息
- 操作系统：Ubuntu Latest
- 桌面环境：XFCE4
- 远程协议：XRDP
- 端口：3390 (注意：不是默认的 3389)

## 前期准备

### 1. Cloudflare 隧道配置
1. 登录 [Cloudflare Zero Trust 控制台](https://one.dash.cloudflare.com/)
2. 转到 `Networks` > `Tunnels`
3. 创建或编辑隧道：
   - 点击 `Configure`
   - 在 Public Hostname 配置中设置：
     ```
     Type: TCP
     Subdomain: 您想要的子域名
     Domain: 选择您的域名
     URL: localhost:3390  # 必须使用 3390 端口
     ```
   - 保存配置

### 2. GitHub 仓库设置
1. 创建私有仓库
2. 添加 Secret：
   - 转到 Settings > Secrets and variables > Actions
   - 添加新的 secret：
     * 名称：`CLOUDFLARED_TOKEN`
     * 值：从 Cloudflare 隧道配置页面获取的令牌

## 连接信息
- 用户名：`runner`
- 密码：`P@ssw0rd!`
- 地址：使用 Cloudflare 隧道提供的地址
- 端口：由 Cloudflare 自动分配

## 连接步骤

### Windows 用户
1. 打开远程桌面连接（mstsc）
2. 点击"显示选项"
3. 在"常规"标签：
   - 计算机：输入 Cloudflare 提供的地址
   - 用户名：`runner`
4. 在"显示"标签：
   - 建议先选择较低分辨率（如 1024x768）
5. 点击连接，输入密码：`P@ssw0rd!`
6. 在会话选择界面：
   - 必须选择 "Xorg" 会话类型
   - 不要选择默认的 Xvnc

### Linux 用户
1. 安装 Remmina：
   ```bash
   sudo apt install remmina remmina-plugin-rdp
   ```
2. 创建新连接：
   - 协议：RDP
   - 服务器：Cloudflare 提供的地址
   - 用户名：runner
   - 密码：P@ssw0rd!
   - 色彩深度：高彩（24位）
   - 分辨率：自动

### macOS 用户
1. 从 App Store 下载 Microsoft Remote Desktop
2. 点击"添加桌面"
3. 填写连接信息：
   - PC 名称：Cloudflare 提供的地址
   - 用户账户：选择"添加用户账户"
     * 用户名：runner
     * 密码：P@ssw0rd!
4. 点击保存并连接

## 故障排除

### 1. 连接被拒绝
检查 Cloudflare 隧道配置：
- 确认端口设置为 3390
- 检查隧道状态是否为 "Active"
- 验证 Public Hostname 配置是否正确

### 2. 黑屏或显示问题
- 确保选择 "Xorg" 会话类型
- 尝试以下步骤：
  1. 断开连接
  2. 重新连接时选择较低分辨率
  3. 确认选择 "Xorg" 而不是 Xvnc

### 3. 认证失败
- 确保使用正确的用户名（runner）和密码
- 检查大小写输入
- 如果使用非英文键盘，注意切换到英文输入

### 4. 性能问题
- 在远程桌面连接的"体验"选项卡中：
  * 调整性能设置
  * 取消不需要的视觉效果
- ���择较低的色彩深度（如16位）
- 使用有线网络连接

## 注意事项
1. 会话超时时间为 6 小时（GitHub Actions 限制）
2. 建议定期保存工作内容
3. 系统重启后需要重新运行工作流
4. 每次工作流运行都是全新的环境
5. 请勿在系统中存储敏感信息

## 安全建议
1. 使用私有仓库
2. 定期更改密码
3. 不要分享您的连接信息
4. 使用完毕后停止工作流