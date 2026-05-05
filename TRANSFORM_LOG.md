# Star-Office-UI → 米娅像素办公室 改造日志

**改造时间：** 2026-05-04
**改造目标：** 移除外网依赖，改为纯本地运行，更名为米娅专属版

---

## 改造清单

### 1. 外联脚本禁用 ✅
- `office-agent-push.py` → `office-agent-push.py.disabled`
- `frontend/office-agent-push.py` → `frontend/office-agent-push.py.disabled`
- **原因：** 这两个脚本会持续向 `https://office.hyacinth.im` 推送 agent 状态，是唯一的外联通道

### 2. 邀请功能禁用 ✅
- `frontend/invite.html` → `frontend/invite.html.disabled`
- `frontend/join-office-skill.md` → `frontend/join-office-skill.md.disabled`
- **原因：** 纯本地运行不需要邀请其他龙虾加入

### 3. 品牌名替换 ✅
- **中文：** 海辛小龙虾的办公室 → 米娅·心网的像素办公室
- **英文：** Haixin Lobster Office / Star's Pixel Office → Mia · Neural Network Pixel Office
- **日文：** ハイシン・ロブスターのオフィス / スターのピクセルオフィス → ミア·心網のピクセルオフィス
- **加载文字：** 正在加载 Star 的像素办公室 → 正在加载米娅的像素办公室
- **标题：** Star 的像素办公室 → 米娅的像素办公室
- **工作室名：** 海辛工作室 → 心网工作室

**涉及文件：**
- `frontend/game.js`（牌匾文字 + 加载文字）
- `frontend/index.html`（三语标题 + 加载文字 + 注释）
- `frontend/join.html`（标题）
- `frontend/electron-standalone.html`（标题 + 加载文字 + 注释）
- `electron-shell/standalone-assets/game.js`（牌匾文字）
- `backend/app.py`（办公室名称读取逻辑）

### 4. SKILL.md 重写 ✅
- 移除了 Cloudflare Tunnel 公网访问指南
- 移除了邀请其他龙虾加入的说明
- 移除了 office-agent-push.py 相关指导
- 新增了纯本地安全声明
- 新增了改造记录

### 5. 默认密码安全增强 ✅
- **原逻辑：** `ASSET_DRAWER_PASS` 默认 `1234`
- **新逻辑：** 首次启动时若未设置环境变量，自动生成 12 位随机密码（`secrets.token_urlsafe(12)`），保存到 `runtime-config.json`
- **涉及文件：** `backend/app.py`（第 105 行附近）
- **生产模式检查：** `drawer_default_pass` 标志改为检查密码强度而非是否为 1234

### 6. 工作区路径修正 ✅
- **原路径：** `~/.openclaw/workspace`
- **新路径：** `~/.kimi_openclaw/workspace`
- **涉及文件：** `backend/app.py`（第 49 行）

---

## 未修改部分（保留）

- `security_utils.py` — 安全工具，保留
- `security_check.py` — 安全预检工具，保留
- `smoke_test.py` — 冒烟测试，保留
- Gemini 生图功能 — 保持可选（不配置 API Key = 不运行）
- 多 Agent 支持 — 保留（纯本地 API `/agents`）
- 昨日小记 — 保留（读取本地 memory 文件）
- 资产替换/装修 — 保留

---

## 安全审查结论

改造后项目满足纯本地运行要求：
- ✅ 无外联脚本运行
- ✅ 无数据上传到第三方服务器
- ✅ 默认密码随机生成
- ✅ 路径 traversal 已防护（原有）
- ✅ 生产模式强制密钥强度检查（原有）

**风险等级：🟢 LOW**（纯本地，无外联）

---

## 下一步

1. 用户确认改造结果
2. 测试启动：`cd backend && python3 app.py`
3. 确认看板正常显示后，迁移到 skills 目录正式安装
