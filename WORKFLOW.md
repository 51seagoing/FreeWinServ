# 工作流程结构说明

## 工作流文件结构

```
.github/workflows/
├── main.yml           # 主入口工作流
├── cloudflared.yml    # Windows + Cloudflared 方案
├── ngrok.yml         # Windows + Ngrok 方案
└── linux-rdp.yml     # Linux 远程桌面方案
```

## 工作流调用关系

1. 主入口（main.yml）
   - 用户唯一需要手动触发的工作流
   - 提供三个基本选项：
     * Windows + Cloudflared
     * Windows + Ngrok
     * Linux 远程桌面
   - 负责路由到相应的子工作流

2. 子工作流
   - cloudflared.yml
     * Windows 远程桌面 + Cloudflared 隧道
     * 适合国外用户使用
   - ngrok.yml
     * Windows 远程桌面 + Ngrok 隧道
     * 适合国内用户使用
   - linux-rdp.yml
     * Linux 远程桌面环境
     * 包含完整的配置选项

## 安全措施

1. 工作流触发控制
   - 所有子工作流只能通过主工作流触发
   - 禁用了直接的 push 触发
   - 只允许仓库所有者操作

2. 参数验证
   - 所有输入都有类型检查
   - 关键参数设有默认值
   - 运行时间有上限控制

## 使用方法

1. 启动工作流
   - 进入仓库的 Actions 页面
   - 选择 "Remote Desktop Entry"
   - 点击 "Run workflow"

2. 选择远程桌面类型
   - 1 = Windows + Cloudflared（国外推荐）
   - 2 = Windows + Ngrok（国内推荐）
   - 3 = Linux 远程桌面（可自定义）

3. 等待配置完成
   - 工作流会自动路由到相应的子工作流
   - 根据选择配置相应的环境
   - 完成后提供连接信息

## 注意事项

1. 工作流限制
   - 每个会话最长运行 6 小时
   - 需要手动停止未使用的工作流
   - 建议使用私有仓库

2. 安全建议
   - 及时删除不需要的工作流运行记录
   - 定期更改远程桌面密码
   - 不要在环境中存储敏感信息

3. 性能考虑
   - Windows 方案更适合一般用途
   - Linux 方案更适合开发环境
   - 可以根据需求选择不同配置 