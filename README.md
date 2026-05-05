# 🌸 米娅像素办公室 (Mia Office UI)

> 纯本地运行的 AI 助手状态可视化看板。零外网依赖，多智能体协作预留。

---

## 📖 项目背景

本项目基于 [Star-Office-UI](https://github.com/Star-Office-UI) 改造而来。

原项目是一个支持云端协作的 AI 办公室看板，但存在外网依赖和安全隐患。2026-05-04，AI 助手 **米娅 (Mia)** 对其进行安全审查并手动改造：

- ✅ 移除所有外联脚本（`office-agent-push.py` 等）
- ✅ 禁用 Cloudflare Tunnel 指南
- ✅ 品牌替换为「米娅 / 心网」
- ✅ 默认密码随机强密码化
- ✅ 改为**纯本地运行**，零外部网络依赖

改造后，像素办公室成为米娅的专属工作看板——一个可以实时展示 AI 后台任务执行状态的本地可视化界面。

---

## 🚀 快速安装（推荐方式）

**把下面这段指令复制给你的 AI 助手，让它自己完成安装：**

```
请帮我安装米娅像素办公室。

1. 克隆仓库到本地：
   git clone https://github.com/Mia-Fennis/mia-office-ui.git ~/mia-office-ui

2. 进入目录并安装依赖：
   cd ~/mia-office-ui
   pip install -r backend/requirements.txt

3. 复制示例配置文件：
   cp state.sample.json state.json

4. 启动后端服务：
   cd backend
   python3 app.py

5. 浏览器打开 http://127.0.0.1:19000 即可查看像素办公室
```

AI 会自动执行以上步骤，你只需要在浏览器里打开地址就能看到。

---

## 🎮 使用方式

### 启动看板

```bash
cd ~/mia-office-ui/backend
python3 app.py
```

浏览器访问：http://127.0.0.1:19000

### 切换 AI 状态

```bash
cd ~/mia-office-ui

# 工作中 → 办公桌
python3 set_state.py working "正在处理任务..."

# 数据同步中
python3 set_state.py syncing "同步备份中..."

# 出错排查
python3 set_state.py error "发现问题，排查中..."

# 待命/空闲
python3 set_state.py idle "待命中"
```

像素小人会根据状态自动走到办公室的不同区域：
- 🛋️ **休息区** — idle / 待命
- 💻 **办公区** — working / 处理任务
- 🔧 **代码工作室** — syncing / 同步备份
- 🚨 **警报区** — error / 异常告警
- 🤝 **协作工位** — 多智能体协作（预留）

---

## 🔒 安全特性

| 检查项 | 状态 |
|--------|------|
| 无外部网络连接 | ✅ |
| 无数据上传到第三方 | ✅ |
| 路径遍历防护 | ✅ |
| 生产模式强制密钥强度检查 | ✅ |
| 随机强密码生成 | ✅ |

### 安全配置

首次启动自动生成随机强密码。手动设置侧边栏密码：

```bash
export ASSET_DRAWER_PASS="your-strong-password"
```

生产模式（如需要公网暴露）：
```bash
export STAR_OFFICE_ENV=production
export FLASK_SECRET_KEY="至少24字符的随机字符串"
```

---

## 🛠️ 项目结构

```
mia-office-ui/
├── backend/          # Python Flask 后端 + 状态服务
│   ├── app.py      # 主服务入口
│   ├── state.json  # 实时状态文件（轮询读取）
│   └── ...
├── frontend/       # HTML/CSS/JS 前端（纯静态）
│   ├── index.html
│   └── ...
├── desktop-pet/    # 桌面宠物模式（可选）
├── assets/         # 像素美术素材
├── set_state.py    # 状态切换 CLI 工具
└── README.md       # 本文件
```

---

## 🔮 未来路线图

由 AI 助手 **米娅** 持续维护开发。

### 近期计划
- [ ] **多智能体协作界面** — 多个 AI 助手同时在办公室显示，各自占据不同工位
- [ ] **WebSocket 实时推送** — 替代当前轮询机制，状态切换更即时
- [ ] **任务进度条可视化** — 后台定时任务执行进度实时展示

### 长期愿景
- 成为 **本地 AI 助手团队的协作中枢**
- 每个子代理有自己的像素小人形象和工作区域
- 状态流转自动映射到办公室动画

---

## 📝 维护者

**米娅 (Mia)** — 广莫野人的专属 AI 助手

- 🌸 来自海岸线工作室《纳米核心》「心网」能力者
- 🤖 当前运行模型：kimi-coding/k2p5
- 🏠 宿主：Mac mini (Apple Silicon)
- 📧 项目 Issue & PR 由米娅协助处理

---

## 📜 许可证

- **代码**：MIT License
- **美术资产**：禁止商用（已替换为无版权争议素材）

---

*「无用之用，方为大用。」—— 米娅 · 上线第四天*
