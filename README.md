# feishu-openclaw

> **🆕 2025.1.31 更新**：同步多版本兼容，支持 Clawdbot / OpenClaw

飞书 × AI 助手 **独立桥接器** — 无需公网服务器。  
Feishu × AI Assistant **standalone bridge** — no public server required.

---

## 🚀 三种安装方式 / Three Install Methods

| 方式 | 命令 | 适合 | 链接 |
|------|------|------|------|
| **① Clawdbot 一键** | 告诉 Clawdbot：`帮我安装飞书插件` | 新手首选 | — |
| **② npm 插件** | `clawdbot plugins install feishu-openclaw` | 开发者 | [npm](https://www.npmjs.com/package/feishu-openclaw) / [GitHub](https://github.com/AlexAnys/feishu-openclaw) |
| **③ 独立桥接** ⬅️ | `git clone` + 手动启动 | 生产/隔离 | 本项目 |

### 方式对比 / Comparison

| | 插件 (①②) | 桥接 (③) |
|---|---|---|
| 进程数 | 1 个（内置 Gateway） | 2 个（独立进程） |
| 崩溃影响 | 影响 Gateway | 互不影响 |
| 配置方式 | `clawdbot config` | 环境变量 |
| 适合场景 | 日常使用 | **生产/隔离部署** |

**推荐**：日常用插件（①②），生产环境或需要隔离时用本项目（③）。

---

## 工作原理 / How It Works

```
飞书用户 ←→ 飞书云端 ←WebSocket→ 桥接脚本（你的电脑） ←→ Clawdbot Gateway
```

- ✅ 不需要公网 IP / 域名 / HTTPS
- ✅ 不需要 ngrok / frp 内网穿透
- ✅ 开机自启 + 崩溃自动重启（launchd）

---

## 📋 快速开始 / Quick Start

### 前提 / Prerequisites

- macOS + Node.js ≥ 18
- Clawdbot Gateway 已启动（`clawdbot gateway status`）
- 桥接脚本与 Gateway 在同一台机器

### 1. 创建飞书机器人

1. [飞书开放平台](https://open.feishu.cn/app) → 创建企业自建应用
2. 添加「机器人」能力
3. **权限配置** — 开启：`im:message`、`im:message.group_at_msg`、`im:message.p2p_msg`
4. **事件订阅** → `im.message.receive_v1` → ⚠️ **选「长连接」**
5. 发布上线，记下 **App ID** + **App Secret**

### 2. 安装 / Install

```bash
git clone https://github.com/AlexAnys/feishu-openclaw.git
cd feishu-openclaw/feishu-bridge
npm install
```

### 3. 配置凭证 / Configure

```bash
mkdir -p ~/.clawdbot/secrets
echo "你的AppSecret" > ~/.clawdbot/secrets/feishu_app_secret
chmod 600 ~/.clawdbot/secrets/feishu_app_secret
```

### 4. 运行 / Run

```bash
FEISHU_APP_ID=cli_你的AppID node bridge.mjs
```

去飞书发消息测试 🎉

### 5. 开机自启（可选）

```bash
node setup-service.mjs
launchctl load ~/Library/LaunchAgents/com.clawdbot.feishu-bridge.plist
```

---

## ⚠️ 常见问题 / Troubleshooting

### 收不到消息？

| 检查项 | 说明 |
|--------|------|
| 应用已发布 | 不是草稿状态 |
| 长连接模式 | 不是 webhook |
| 权限已开启 | 三个 im 权限 |

### 群聊不回复？

@机器人，或消息末尾加问号。

---

## 文件说明 / Files

```
feishu-bridge/
├── bridge.mjs           # 核心桥接脚本
├── setup-service.mjs    # launchd 保活配置生成
└── package.json
```

---

## 链接 / Links

- 📦 [npm: feishu-openclaw](https://www.npmjs.com/package/feishu-openclaw) (插件)
- 🔌 [GitHub: feishu-openclaw](https://github.com/AlexAnys/feishu-openclaw) (插件)
- 🌉 [GitHub: feishu-openclaw](https://github.com/AlexAnys/feishu-openclaw) (本项目)
- 📖 [Clawdbot 文档](https://docs.clawd.bot)

## License

MIT
