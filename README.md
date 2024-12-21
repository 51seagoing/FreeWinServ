# Windows Server 2022 RDP via GitHub Actions

此项目提供使用 GitHub Actions 设置 Windows Server 环境的指南，支持通过 Ngrok 或 Cloudflared 隧道进行访问。

## 选择您的连接方式

我们提供两种连接方案，您可以根据需求选择：

### [方案一：使用 Ngrok](ngrok.md)
- 优点：配置简单，连接稳定
- 缺点：需要信用卡验证或付费计划
- 适合：需要稳定性的用户
- [点击查看详细设置指南](ngrok.md)

### [方案二：使用 Cloudflared](Cloudflared%20.md)
- 优点：完全免费，无需信用卡
- 缺点：配置相对复杂
- 适合：注重隐私和安全的用户
- [点击查看详细设置指南](Cloudflared%20.md)

## 通用信息

### 系统信息
- 操作系统：Windows Server 2022
- 运行环境：GitHub Actions
- 连接方式：远程桌面（RDP）

### 默认凭据
- 用户名：`runneradmin`
- 密码：`P@ssw0rd!`

### 使用限制
1. GitHub Actions 运行器时间限制为 6 小时
2. 建议使用私有仓库以保护安全
3. 仅用于测试和教育目的

### 文件结构
```
.
├── README.md           # 本文件
├── ngrok.md           # Ngrok 设置指南
├── Cloudflared .md    # Cloudflared 设置指南
└── .github
    └── workflows
        ├── ngrok.yml      # Ngrok 工作流配置
        └── cloudflared.yml # Cloudflared 工作流配置
```

## 快速开始

1. 选择您偏好的连接方式：
   - [Ngrok 设置指南](ngrok.md)
   - [Cloudflared 设置指南](Cloudflared%20.md)

2. 按照选定指南完成配置

3. 运行工作流：
   - 转到仓库的 "Actions" 标签页
   - 选择相应的工作流
   - 点击 "Run workflow"

4. 使用远程桌面连接：
   - 等待工作流执行完成
   - 在日志中找到连接地址
   - 使用默认凭据进行连接

## 注意事项

- 请确保遵守相关服务条款
- 保护好您的凭据和令牌
- 定期更新配置以确保安全
- 如遇问题，请查看相应指南的故障排除部分

## 相关链接

- [GitHub Actions 文档](https://docs.github.com/cn/actions)
- [Ngrok 官网](https://ngrok.com/)
- [Cloudflare 官网](https://www.cloudflare.com/)

## 贡献

欢迎提交 Issue 和 Pull Request 来改进本项目。

## 免责声明

本项目仅供学习和测试使用，请勿用于生产环境或其他非法用途。使用本项目造成的任何问题，项目维护者概不负责。