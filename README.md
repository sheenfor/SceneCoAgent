# SceneCoAgent

### 本地 AI 短剧协作工作台

**从一本小说、一份剧本，走向可控的分镜、素材和成片。**

[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?style=flat-square&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Node.js](https://img.shields.io/badge/Node.js-23%2B-339933?style=flat-square&logo=nodedotjs&logoColor=white)](https://nodejs.org/)
[![Electron](https://img.shields.io/badge/Electron-40-47848F?style=flat-square&logo=electron&logoColor=white)](https://www.electronjs.org/)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg?style=flat-square)](./LICENSE)
[![Version](https://img.shields.io/badge/version-1.1.8-0EA5E9?style=flat-square)](https://github.com/sheenfor/SceneCoAgent)

[![Stars](https://img.shields.io/github/stars/sheenfor/SceneCoAgent?style=for-the-badge&logo=github)](https://github.com/sheenfor/SceneCoAgent/stargazers)
[![Issues](https://img.shields.io/github/issues/sheenfor/SceneCoAgent?style=for-the-badge)](https://github.com/sheenfor/SceneCoAgent/issues)
[![Last commit](https://img.shields.io/github/last-commit/sheenfor/SceneCoAgent?style=for-the-badge)](https://github.com/sheenfor/SceneCoAgent/commits)

> **SceneCoAgent 是一个本地运行的 AI 短剧生产项目。** 它把小说改编、剧本生成、角色与分镜、素材生产和视频拼接放进同一套工作台：决策层、执行层、监督层 Agent 协同推进，过程可检查、可回溯、可继续跑。

> 本仓库基于 [HBAI-Ltd/Toonflow-app](https://github.com/HBAI-Ltd/Toonflow-app)（Apache-2.0，含上游补充商业协议）二次开发，保留原作者版权与许可证全文。上游项目请以 Toonflow 官方仓库为准。

---

## 目录

- [核心特性](#核心特性)
- [它解决什么问题](#它解决什么问题)
- [快速开始](#快速开始)
- [开发与打包](#开发与打包)
- [Docker / 服务器](#docker--服务器)
- [项目结构](#项目结构)
- [技术栈](#技术栈)
- [端到端流程](#端到端流程)
- [相关仓库](#相关仓库)
- [许可证](#许可证)

---

## 核心特性

|  |  |
| --- | --- |
| ### 无限画布生产工作台 | ### 三层 Agent 协作 |
| 以节点方式组织剧本、角色、分镜、素材与视频，支持自由编排、回溯和并行生产，不受线性步骤锁死。 | 决策层、执行层、监督层协同：拆解任务、生成内容、审阅修订，提升稳定性和成片一致性。 |
| ### 持久化 Agent 记忆 | ### 可编程供应商系统 |
| 基于本地 ONNX 向量检索的跨会话记忆：短期消息、长期摘要、语义召回，保证多轮创作不断层。 | 在设置中心编写供应商 TypeScript 逻辑并即时生效，便于私有化和多模型接入。 |
| ### 章节事件图谱改编 | ### Skill 文件化配置 |
| 自动提取原著章节事件并结构化存储，改编时按图谱调用上下文，减少长文本信息丢失。 | ScriptAgent 与 ProductionAgent 的核心提示词外化为 Markdown Skill，支持在线编辑和快速调优。 |

---

## 它解决什么问题

> AI 短剧最难的不是「生成一段视频」，而是让角色、分镜、风格和叙事在整条片子里站得住。

| 挑战 | SceneCoAgent 的做法 |
| --- | --- |
| 长小说改编丢失细节 | 章节事件图谱 + 按事件召回上下文 |
| 角色 / 画风漂移 | 素材节点、画风管理和可回溯工作台 |
| 模型绑定单一供应商 | 可编程供应商，文本 / 图像 / 视频可分开配置 |
| 黑盒一键出片无法修改 | Agent 分阶段产出，可检查、可改、可续跑 |
| 提示词散落在代码里 | Skill 文件化，制作流程可迭代 |

---

## 快速开始

### 环境要求

| 工具 | 版本 | 说明 |
| --- | --- | --- |
| Node.js | `>= 23.11.1`（推荐 24.x） | 运行时 |
| Yarn | 1.x | 包管理 |
| 模型服务 | — | 文本、图像、视频 API |

还需要准备：大语言模型接口、视频生成接口（如 Sora / 豆包）、图片生成接口（如 Nano Banana Pro）。

### 克隆与安装

```bash
git clone https://github.com/sheenfor/SceneCoAgent.git
cd SceneCoAgent
yarn install
```

### 启动

| 方式 | 命令 | 说明 |
| --- | --- | --- |
| 桌面客户端（推荐） | `yarn dev:gui` | 同时拉起后端和 Electron 窗口 |
| 仅后端 API | `yarn dev` | 端口 `10588`，不含完整前端界面 |
| 生产模式 | `yarn build` 后 `yarn start` | 运行编译后的服务 |

首次登录默认账号：`admin` / `admin123`。请在设置中心配置模型供应商后再开始创作。

建议流程：

1. 启动并登录，完成文本 / 图像 / 视频供应商配置  
2. 新建项目，导入原著，执行章节事件提取  
3. 用 ScriptAgent 生成故事骨架、改编策略和结构化剧本  
4. 切换到 ProductionAgent，在无限画布中组织分镜、素材与视频节点  
5. 精调分镜后回流工作台，完成拼接与导出  

---

## 开发与打包

```bash
yarn lint          # 类型检查
yarn build         # 编译 TypeScript
yarn dist:win      # Windows 安装包
yarn dist:mac      # macOS 安装包
yarn dist:linux    # Linux 安装包
yarn debug:ai      # AI SDK 调试面板（可选）
```

> 前端源码不在本仓库。如需改界面，请使用上游 [Toonflow-web](https://github.com/HBAI-Ltd/Toonflow-web)，构建后将 `dist` 复制到本项目的 `data/web`。

---

## Docker / 服务器

```bash
docker build -t scenecoagent .
docker run -d -p 10588:10588 -v <本地数据路径>:/app/data scenecoagent
```

界面默认在 `http://localhost:10588/index.html`。

| 变量 | 说明 |
| --- | --- |
| `NODE_ENV` | `prod` 表示生产环境 |
| `PORT` | 服务端口，默认 `10588` |
| `OSSURL` | 静态资源访问地址 |

云端可用 PM2 托管 `data/serve/app.js`。更完整的部署说明见上游 Toonflow 文档。

---

## 项目结构

```
src/
├── agents/              # ScriptAgent / ProductionAgent
├── lib/                 # 数据库初始化、供应商模板
├── middleware/
├── routes/              # 小说、剧本、分镜、素材、设置等 API
├── socket/              # 实时通信
├── utils/               # AI、记忆、OSS、图像等
├── app.ts               # 入口
└── core.ts
data/
├── serve/               # 生产环境编译入口
├── skills/              # Agent Skill 提示词
├── vendor/              # 供应商脚本
└── web/                 # 内置前端构建产物
scripts/                 # 构建与 Electron 入口
docs/                    # 文档资源
```

---

## 技术栈

| 类别 | 选型 |
| --- | --- |
| 运行时 | Node.js 23.11.1+ |
| 语言 | TypeScript 5.x |
| 后端 | Express 5 |
| 数据库 | SQLite（better-sqlite3 / knex） |
| AI | Vercel AI SDK（OpenAI / Anthropic / Google / DeepSeek / 智谱 / MiniMax / 通义千问 / xAI） |
| 本地推理 | @huggingface/transformers（ONNX） |
| 实时通信 | Socket.IO |
| 桌面客户端 | Electron 40 |
| 图像 | Sharp |
| 容器 | Docker |

---

## 端到端流程

```
        ┌─────────────────────────────────┐
        │  小说 / 剧本 / 创作需求           │
        └──────────────┬──────────────────┘
                       ▼
        ┌─────────────────────────────────┐
        │  章节事件提取 · 叙事规划          │
        │  故事骨架 · 角色 · 结构化剧本     │
        └──────────────┬──────────────────┘
                       ▼
        ┌─────────────────────────────────┐
        │  无限画布生产                     │
        │  分镜 · 素材 · 画风 · 镜头节点    │
        └──────────────┬──────────────────┘
                       ▼
        ┌─────────────────────────────────┐
        │  视频生成 · 精调 · 拼接导出       │
        └─────────────────────────────────┘
```

每个阶段都可以停下来检查、修改，再继续。

---

## 相关仓库

| 仓库 | 说明 |
| --- | --- |
| **[SceneCoAgent](https://github.com/sheenfor/SceneCoAgent)** | 本仓库，本地短剧工作台 |
| **[Toonflow-app](https://github.com/HBAI-Ltd/Toonflow-app)** | 上游完整客户端 |
| **[Toonflow-web](https://github.com/HBAI-Ltd/Toonflow-web)** | 上游前端源码 |

---

## 许可证

本项目沿用 [Apache License 2.0](./LICENSE)。

源代码衍生自 [HBAI-Ltd/Toonflow-app](https://github.com/HBAI-Ltd/Toonflow-app)。上游 LICENSE 含 **HBAI-Ltd 补充商业协议**：若将本软件或其衍生版本以产品形式分发给 **2 个及以上独立第三方**，须事先取得 HBAI-Ltd 书面商业授权。不得删除或修改控制台 / 应用中的 Toonflow 标识与版权信息。

个人学习、研究、团队内部使用等场景，请以 `LICENSE` 原文为准。第三方依赖见 `NOTICES.txt`。

---

Built on [Toonflow](https://github.com/HBAI-Ltd/Toonflow-app) · published by [sheenfor](https://github.com/sheenfor)

如果这个项目对你有帮助，欢迎 Star。
