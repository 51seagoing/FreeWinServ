# RDP 连接问题诊断指南

## 错误代码 0x904 含义
- 这个错误通常与 RDP 认证或安全层配置有关
- 扩展错误码 0x7 表示认证协议不匹配

## 诊断步骤

### 1. 检查服务器端（GitHub Actions）
- 查看工作流日志中的诊断信息
- 确认 RDP 服务是否正常运行
- 验证端口 3389 是否开放
- 检查用户权限配置

### 2. 检查客户端（本地 Windows）
在 PowerShell（管理员）中运行：
```powershell
# 检查 RDP 客户端配置
Get-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp'

# 修改客户端设置
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\CredSSP\Parameters" /v AllowEncryptionOracle /t REG_DWORD /d 2 /f
```

### 3. 连接测试
1. 使用 telnet 测试端口：
   ```
   telnet win.51seagoing.us.kg 3389
   ```

2. 使用不同的连接方法：
   ```
   mstsc /v:win.51seagoing.us.kg /admin
   ```
   或
   ```
   mstsc /v:win.51seagoing.us.kg /restrictedAdmin
   ```

### 4. 检查 Cloudflare 隧道
1. 确认隧道状态是否为 "Active"
2. 检查隧道配置是否正确
3. 验证域名解析是否正常

## 常见解决方案

1. 降低客户端安全设置：
   - 打开远程桌面连接
   - 点击"显示选项"
   - 在"高级"选项卡中禁用网络级别身份验证

2. 使用 RDP 配置文件：
   创建 `connect.rdp` 文件：
   ```
   authentication level:i:0
   prompt for credentials:i:0
   negotiate security layer:i:0
   full address:s:win.51seagoing.us.kg
   username:s:runneradmin
   ```

3. 重置连接：
   ```cmd
   cmdkey /delete:win.51seagoing.us.kg
   cmdkey /generic:win.51seagoing.us.kg /user:runneradmin /pass:P@ssw0rd!
   ```

## 日志分析
请检查工作流输出中的以下信息：
1. RDP 服务状态
2. 防火墙规则配置
3. 端口测试结果
4. 用户权限设置
5. 网络配置

## 需要的信息
如果问题持续，请提供：
1. 工作流完整日志
2. 本地系统信息
3. 具体的错误信息和时间戳
4. 使用的连接方法 