## Cloudflared 令牌获取详细步骤

1. 访问 [Cloudflare Zero Trust Dashboard](https://one.dash.cloudflare.com/)
   - 登录您的 Cloudflare 账户
   - 如果是首次使用，需要先设置组织名称

2. 在左侧菜单中找到 `Networks`
   - 如果看不到 `Networks`，点击左上角的汉堡菜单展开
   - 在 `Networks` 下找到 `Tunnels`

3. 创建新隧道
   - 点击 `Create a tunnel`
   - 输入隧道名称（例如：`windows-rdp`）
   - 点击 `Save tunnel`

4. 在隧道配置页面
   - 选择操作系统为 `Windows`
   - 您会看到一个命令和令牌，类似：
     ```bash
     cloudflared.exe tunnel run <令牌>
     ```
   - 复制这个令牌（一串长字符串）

5. 配置路由
   - 在隧道详情页面，找到 `Public Hostname` 或 `Configure`
   - 点击 `Add a public hostname`
   - 填写以下信息：
     * Subdomain: 输入您想要的子域名（例如：`rdp`）
     * Domain: 选择您的 Cloudflare 域名
     * Type: 选择 `TCP`
     * URL: 输入 `localhost:3389`
   
   配置示例：
   ```
   Subdomain: rdp
   Domain: yourdomain.com
   Type: TCP
   URL: localhost:3389
   ```

   如果没有域名：
   - 在 `Public Hostname` 中选择 `Random`
   - Type: 选择 `TCP`
   - URL: 输入 `localhost:3389`
   - Cloudflare 会自动生成一个类似 `random-words.trycloudflare.com` 的地址

### 连接信息
- 如果使用自定义域名，连接地址将是：`rdp.yourdomain.com`
- 如果使用随机域名，使用 Cloudflare 生成的地址
- 端口：Cloudflare 会自动分配
- 协议：TCP
- 本地端口：3389（RDP 默认端口）

### 重要提示
- 确保在保存配置后查看连接地址和端口
- 连接时使用 Cloudflare 提供的完整地址
- 如果使用随机域名，建议保存生成的地址

6. 添加到 GitHub
   - 转到您的 GitHub 仓库
   - 进入 `Settings` > `Secrets and variables` > `Actions`
   - 点击 `New repository secret`
   - 名称填写：`CLOUDFLARED_TOKEN`
   - 值填写：刚才复制的令牌
   - 点击 `Add secret`

### 故障排除

如果在左侧菜单找不到选项：
1. 确保已完成 Zero Trust 设置
2. 尝试点击左上角的展开菜单
3. 检查是否需要先创建组织或团队

### 注意事项
- 令牌是敏感信息，请勿分享或公开
- 一个隧道可以重复使用
- 确保隧道状态为活跃（Active）
- 如果遇到问题，检查 Cloudflare 控制台的错误信息

## Cloudflared 连接方���

您可以选择以下两种方式之一来设置 Cloudflared 连接：

### 方式一：快速隧道（推荐新手使用）

1. 无需配置，直接在工作流中使用以下命令：
```yaml
.\cloudflared.exe tunnel --no-autoupdate --url tcp://localhost:3389
```

2. 运行后，您将获得一个类似这样的地址：
```
https://random-words.trycloudflare.com
```

优点：
- 设置简单，无需注册
- 无需配置域名
- 适合测试使用

缺点：
- 每次运行会生成新的地址
- 没有持久性保证
- 不建议用于生产环境

### 方式二：命名隧道（适合长期使用）

1. 访问 [Cloudflare Zero Trust Dashboard](https://one.dash.cloudflare.com/)
   - 登录您的 Cloudflare 账户
   - 如果是首次使用，需要先设置组织名称

2. 创建命名隧道
   - 在左侧菜单中找到 `Networks` > `Tunnels`
   - 点击 `Create a tunnel`
   - 输入隧道名称：`windows-rdp`
   - 点击 `Save tunnel`

3. 获取隧道令牌
   - 在隧道创建后的页面，选择 `Windows` 平台
   - 您会看到一个配置命令，类似：
     ```bash
     cloudflared.exe service install XXXX-YOUR-TUNNEL-TOKEN-XXXX
     ```
   - 复制这个令牌（就是 XXXX-YOUR-TUNNEL-TOKEN-XXXX 这部分）

4. 配置隧道路由
   - 在隧道详情页面点击 `Configure`
   - 点击 `Add public hostname`
   - 填写配置：
     ```
     Type: TCP
     Subdomain: rdp (或其他您想要的名称)
     Domain: 选择您的域名
     URL: localhost:3389
     ```
   - 如果没有自己的域名，选择 `Random Subdomain`

5. 添加到 GitHub Secrets
   - 转到您的 GitHub 仓库
   - 进入 Settings > Secrets and variables > Actions
   - 点击 `New repository secret`
   - 名称：`CLOUDFLARED_TOKEN`
   - 值：第 3 步复制的令牌
   - 点击 `Add secret`

6. 验证配置
   - 确保隧道状态显示为 `Active`
   - 记录下 Cloudflare 分配的域名或您配置的自定义域名

### 连接信息
- 地址：使用步骤 4 中配置的域名
- 用户名：`runneradmin`
- 密码：`P@ssw0rd!`

### 故障排除
1. 如果连接失败：
   - 检查隧道状态是否为 `Active`
   - 确认令牌是否正确配置在 GitHub Secrets 中
   - 等待约 30 秒后重试连接

2. 如果看不到隧道选项：
   - 确保已完成 Zero Trust 设置
   - 检查左侧菜单是否完全展开
   - 可能需要刷新页面

### 优势
- 固定的访问地址
- 更好的稳定性和安全性
- 可以配置自定义域名
- 支持长期使用

### 注意事项
- 保管好隧道令牌，它类似于密码
- 不要在公开场合分享配置信息
- 建议定期更新 RDP 密码
- 使用完毕后可以在面板中暂停隧道

## 使用说明

### 快速隧道
1. 运行工作流后，在日志中查找类似这样的输出：
```
Your quick Tunnel has been created! Visit it at (it may take some time to be reachable):
https://vincent-ceremony-displaying-lopez.trycloudflare.com
```

2. 使用远程桌面连接：
   - 地址：使用生成的 trycloudflare.com 地址
   - 用户名：`runneradmin`
   - 密码：`P@ssw0rd!`

### 注意事项
- 快速隧道的地址每次运行都会改变
- 连接可能需要等待几秒钟才能建立
- 如果连接失败，请等待 30 秒后重试
- 建议保存生成的地址，因为在日志滚动后可能难以找到