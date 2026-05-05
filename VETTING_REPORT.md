SKILL VETTING REPORT
═══════════════════════════════════════
Skill: star-office-ui
Source: GitHub (ringhyacinth/Star-Office-UI)
Author: ringhyacinth
Version: 未明确标注（最新更新 2026-03-05）
───────────────────────────────────────
METRICS:
• Downloads/Stars: GitHub 网络不稳定，API 查询失败（本地已下载 v2026-03-05 版本）
• Last Updated: 2026-03-05（docs/UPDATE_REPORT_2026-03-05.md 确认）
• Files Reviewed: 12 个核心文件
  - backend/app.py (81KB, Flask 后端主服务)
  - backend/security_utils.py (密钥强度校验)
  - backend/store_utils.py (JSON 存储工具)
  - backend/memo_utils.py (memo 脱敏处理)
  - backend/run.sh (启动脚本)
  - scripts/gemini_image_generate.py (Gemini 生图 CLI)
  - scripts/security_check.py (安全预检工具)
  - scripts/smoke_test.py (冒烟测试)
  - set_state.py (本地状态写入)
  - office-agent-push.py (Agent 状态外推脚本)
  - frontend/game.js (Phaser 前端游戏逻辑)
  - SKILL.md + pyproject.toml
───────────────────────────────────────
RED FLAGS:

1. 🔴 office-agent-push.py — 外联风险
   - 默认连接 `https://office.hyacinth.im`（米娅公网服务器）
   - 持续每 15 秒推送 agent 状态到外部服务器
   - 包含 JOIN_KEY / AGENT_NAME 外发
   - 这是唯一会主动连外网的文件

2. 🟡 scripts/gemini_image_generate.py — 可选外联
   - 需要 GEMINI_API_KEY 才能调用 Google Gemini API
   - 但 SKILL.md 明确说明：不配置 API 也能用基础看板
   - 属于用户主动开启的功能，非强制

3. 🟡 app.py 读取 IDENTITY.md
   - 读取 `~/.openclaw/workspace/IDENTITY.md` 获取办公室名称
   - 纯本地文件读取，无外发
   - 已做路径安全检查（resolve + relative_to 校验）

4. 🟢 无其他红旗项
   - 无 eval/exec 外部输入
   - 无 base64 解码可疑内容（仅 gemini 脚本用于标准图片编码）
   - 无系统文件修改
   - 无浏览器 cookie 访问
   - 无 SSH/AWS 凭证读取
───────────────────────────────────────
PERMISSIONS NEEDED:
• Files:
  - 读取：IDENTITY.md（工作区身份文件）、state.json、memory/*.md、frontend/ 静态资源
  - 写入：state.json、agents-state.json、asset-positions.json、asset-defaults.json、runtime-config.json（chmod 600）、assets/ 子目录
• Network:
  - 🔴 office-agent-push.py → https://office.hyacinth.im（外推状态，持续连接）
  - 🟡 gemini_image_generate.py → Google Gemini API（仅当用户配置了 API Key 时）
  - 🟢 其余所有 API（/status、/agents、/assets 等）纯本地 127.0.0.1
• Commands:
  - subprocess.run（调用 gemini 生图脚本）
  - os.system（调用 ImageMagick/ffmpeg 做精灵表转换）
───────────────────────────────────────
RISK LEVEL: 🟡 MEDIUM（含外联功能，但均为用户可控）

VERDICT: ⚠️ INSTALL WITH CAUTION
  - 基础看板功能（状态可视化、多 Agent、资产替换）纯本地运行，安全
  - 必须移除/禁用 office-agent-push.py 才能满足「不连外网」需求
  - Gemini 生图为可选功能，不配置 Key = 不运行
  - 代码质量较高，有安全硬化措施（session 保护、路径校验、生产模式检查）

NOTES:
  - 项目有完善的安全检查工具（security_check.py、security_utils.py）
  - 生产模式会强制检查密钥强度（FLASK_SECRET_KEY >=24 字符、ASSET_DRAWER_PASS >=8 字符且非 1234）
  - runtime-config.json 自动 chmod 0o600，保护 API Key
  - 前端无恶意代码，纯 Phaser.js 游戏渲染
  - 美术资产已改为无版权争议的小猫角色
═══════════════════════════════════════
