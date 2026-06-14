# CyberEdu Agent

CyberEdu Agent 是一个赛博朋克风格的全栈 Web 学习平台。用户输入书籍名称、作者、章节或少量原文后，系统会自动将知识内容转化为以下三种学习形式：

- 小游戏：Canvas 问答闯关 / 选择题战斗
- 视频音乐：微视频脚本、字幕与配乐提示
- 漫画：多画格知识分镜与悬停解释气泡

项目包含 React + Vite 前端、Express API、JWT 登录认证，以及基于 SQLite 的历史记录持久化。

## 技术栈

- 前端：React 18 + TypeScript + Vite + Tailwind CSS + Framer Motion + Zustand
- 后端：Node.js + Express + TypeScript
- 数据库：SQLite + `better-sqlite3`
- 认证：`bcrypt` + `jsonwebtoken`
- AI 生成：默认 mock 结果，可通过 OpenAI API Key 切换为真实模型

## 已实现功能

- 赛博朋克首页与动态粒子背景
- 用户注册 / 登录，返回 JWT
- 生成工作台，支持输入书籍信息并切换三种模式
- 结果页按模式渲染小游戏、视频音乐或漫画
- 历史记录列表页与详情页
- SQLite 持久化用户和生成记录
- OpenAI 可插拔生成层，无 Key 时回退到 mock

## 环境要求

- Node.js 18 及以上
- npm 9 及以上

## 快速开始

1. 安装依赖

```bash
npm install
```

2. 复制环境变量模板

```bash
copy .env.example .env
```

3. 按需修改 `.env`

- 如果只想本地演示，可以只保留 `JWT_SECRET`
- 如果要接入真实 LLM，请填写 `OPENAI_API_KEY`

4. 启动开发环境

```bash
npm run dev
```

默认行为：

- 前端 Vite 开发服务器运行在 `http://localhost:5173`
- 后端 Express 服务运行在 `http://localhost:3001`
- Vite 已配置 `/api` 代理到后端

## 可用脚本

```bash
npm run dev
npm run check
npm run test
npm run build
```

脚本说明：

- `npm run dev`：同时启动前后端开发环境
- `npm run check`：执行 TypeScript 类型检查
- `npm run test`：运行当前测试
- `npm run build`：构建前端产物

## 环境变量

复制 `.env.example` 并重命名为 `.env`：

```env
JWT_SECRET=your_jwt_secret_key_here

# 使用豆包（火山引擎）生成内容
ARK_API_KEY=your_ark_api_key_here
ARK_ENDPOINT_ID=your_ark_endpoint_id_here

# 备选：使用 OpenAI 生成内容
OPENAI_API_KEY=your_openai_api_key_here
OPENAI_MODEL=gpt-4o-mini
```

**说明**：系统会优先检测 `ARK_API_KEY`（豆包），如果没配置则检测 `OPENAI_API_KEY`。如果都没配置，会回退到使用内置的 Mock 数据进行演示。

## 数据存储

SQLite 文件会在首次启动后自动生成到：

```text
api/data/cyberedu.sqlite
```

当前会创建两张表：

- `users`
- `generations`

## API 概览

- `POST /api/auth/register`：注册
- `POST /api/auth/login`：登录
- `POST /api/generate`：鉴权后生成学习内容
- `GET /api/history`：获取当前用户历史记录
- `GET /api/history/:id`：获取某条历史详情

## 项目结构

```text
src/
  components/
  pages/
  store/
  utils/
api/
  data/
  lib/
  middleware/
  routes/
shared/
  cyberedu.ts
```

## 注意事项

- 当前代码已完成结构重构，但你需要先在本地安装 Node.js 才能运行。
- 如果本地尚未安装依赖，`package-lock.json` 可能不会立即反映最新依赖版本，执行一次 `npm install` 后会自动更新。
- 如果没有配置 `OPENAI_API_KEY`，系统会自动回退到 mock 生成结果，便于演示。
