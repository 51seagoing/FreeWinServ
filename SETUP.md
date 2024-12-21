# GitHub Actions 配置教程

## 运行工作流程

### 1. 进入 Actions 页面
![Step 1](images/step1.png)
- 点击仓库顶部的 "Actions" 标签
- 确保您已经将工作流文件添加到仓库中

### 2. 选择工作流
![Step 2](images/step2.png)
- 在左侧边栏找到 "Linux-RDP" 工作流
- 点击进入工作流详情页

### 3. 运行工作流
![Step 3](images/step3.png)
- 点击 "Run workflow" 按钮
- 会弹出配置面板

### 4. 配置选项
![Step 4](images/step4.png)
在配置面板中设置：
- **选择操作系统**：
  * ubuntu - Ubuntu 最新版（推荐）
  * debian - Debian 最新版
  * fedora - Fedora 最新版
  * centos - CentOS 最新版

- **选择桌面环境**：
  * xfce - 轻量级（推荐）
  * mate - 传统风格
  * lxde - 超轻量级

- **选择浏览器**：
  * firefox - Firefox（推荐）
  * chrome - Google Chrome
  * chromium - Chromium

- **设置密码**：
  * 默认：P@ssw0rd!
  * 可以设置自定义密码

- **运行时间**：
  * 默认：360 分钟
  * 范围：60-360 分钟

### 5. 启动工作流
- 确认配置无误后
- 点击绿色的 "Run workflow" 按钮
- 等待工作流启动和配置（3-5分钟）

## 推荐配置

### 首次使用推荐：
- 操作系统：ubuntu
- 桌面环境：xfce
- 浏览器：firefox
- 密码：使用默认密码
- 运行时间：360分钟

### 性能优先配置：
- 操作系统：debian
- 桌面环境：lxde
- 浏览器：chromium
- 运行时间：根据需要设置

### 开发环境配置：
- 操作系统：fedora
- 桌面环境：mate
- 浏览器：chrome
- 运行时间：360分钟

## 注意事项
1. 确保在运行前已经正确配置了 Cloudflare 隧道
2. 建议首次使用时使用推荐配置
3. 可以同时运行多个不同配置的实例
4. 使用完毕后记得停止工作流 