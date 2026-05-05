---
name: mia-office-ui
description: 米娅专属像素办公室 — 纯本地运行，零外网依赖。AI 助手状态可视化看板。
---

# 米娅像素办公室 🌸

纯本地运行的像素风格 AI 办公室看板。零外部网络依赖，所有数据保存在本地。

## 核心特性

- **纯本地运行** — 不连接任何外部服务器
- **状态可视化** — AI 根据工作状态自动走到不同区域
- **多 Agent 支持** — 多个 AI 助手同时在办公室中显示
- **资产自定义** — 可替换美术资产、调整布局
- **昨日小记** — 自动读取工作区 memory 文件展示

## 快速启动

```bash
cd ~/Documents/trae-projects/Star-Office-UI
python3 -m pip install -r backend/requirements.txt
cp state.sample.json state.json
cd backend
python3 app.py
```

打开 http://127.0.0.1:19000 查看办公室。

## 状态切换

```bash
# 工作中 → 办公桌
python3 set_state.py writing "正在处理任务"

# 同步中
python3 set_state.py syncing "同步备份中"

# 报错 → bug 区
python3 set_state.py error "发现问题，排查中"

# 待命 → 休息区
python3 set_state.py idle "待命中"
```

## 安全配置

### 侧边栏密码

首次启动时，系统会自动生成随机强密码（写入 `runtime-config.json`）。

如需手动设置：
```bash
export ASSET_DRAWER_PASS="your-strong-pass"
```

### 生产模式

公网暴露时务必启用：
```bash
export STAR_OFFICE_ENV=production
export FLASK_SECRET_KEY="至少24字符的随机字符串"
```

## 可选功能

### 生图装修（需自行配置 API）

"搬新家/找中介"功能需要 Gemini API，**基础看板不需要**。

配置方式：
1. 侧边栏 → 生图配置区域输入 API Key
2. 或环境变量：`export GEMINI_API_KEY="your-key"`

### 昨日小记

在工作区上级目录创建 `memory/YYYY-MM-DD.md`，后端自动读取并脱敏展示。

## 版权说明

- 代码：MIT License
- 美术资产：禁止商用（角色已替换为无版权争议素材）

## 安全声明

本项目经过安全审查：
- ✅ 无外部网络连接（除用户主动配置的生图 API）
- ✅ 无数据上传到第三方服务器
- ✅ 路径 traversal 已防护
- ✅ 生产模式强制密钥强度检查

**改造记录：** 2026-05-04 移除外联脚本 `office-agent-push.py`、移除 Cloudflare Tunnel 指南、增强默认密码安全。
