# Windows Server 2022 with RDP via GitHub Actions

此项目使用 GitHub Actions 和 Cloudflared 隧道来设置 Windows Server 环境。

## 前提条件

1. GitHub 账户: [注册](https://github.com/)
2. Cloudflare 账户: [注册](https://dash.cloudflare.com/sign-up)

## 设置步骤

1. 创建私有仓库
2. 获取 Cloudflared 令牌:
   - 登录 [Cloudflare Zero Trust](https://one.dash.cloudflare.com/)
   - 转到 Access > Tunnels
   - 创建新隧道并复制令牌

3. 添加仓库密钥:
   - 转到仓库 Settings > Secrets and variables > Actions
   - 添加新密钥:
     - 名称: `CLOUDFLARED_TOKEN`
     - 值: 您的 Cloudflared 令牌

4. 设置 GitHub Actions 工作流:
   - 创建 `.github/workflows/main.yml`
   - 复制下面面提供的工作流配置



```
name: Windows-RDP

on: [push, workflow_dispatch]

jobs:
  build:
    runs-on: windows-latest
    timeout-minutes: 360
    
    steps:
    - name: Download Cloudflared
      run: |
        Invoke-WebRequest -Uri "https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-windows-amd64.exe" -OutFile "cloudflared.exe"
    
    - name: Enable RDP
      run: |
        Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server'-name "fDenyTSConnections" -Value 0
        Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
        Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "UserAuthentication" -Value 1
        Set-LocalUser -Name "runneradmin" -Password (ConvertTo-SecureString -AsPlainText "P@ssw0rd!" -Force)
    
    - name: Create Tunnel
      run: |
        .\cloudflared.exe tunnel --no-autoupdate --url tcp://localhost:3389
      env:
        TUNNEL_TOKEN: ${{ secrets.CLOUDFLARED_TOKEN }}
```



## 访问服务器

1. 运行工作流（在 Actions 标签页中）
2. 默认凭据:
   - 用户名: `runneradmin`
   - 密码: `P@ssw0rd!`
3. 使用 Cloudflared 提供的地址通过远程桌面连接访问

## 重要说明

- GitHub Actions 运行器有时间限制
- 此设置仅用于测试和教育目的
- 请确保遵守 GitHub Actions 和 Cloudflare 的服务条款
- 保管好您的凭据和令牌



