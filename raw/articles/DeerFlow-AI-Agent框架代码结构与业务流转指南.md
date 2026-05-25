---
source_url: https://mp.weixin.qq.com/s/ (万智创界微信公众号，作者 walton)
ingested: 2026-05-16
sha256: 8dfc3e6c1bbf67f6121b16a3fff5916800cc67cff90082533d0df37c0d5f3668
citation: "万智创界, DeerFlow AI Agent Framework 代码结构与业务流转指南, GitHub: wanrengang/wanzhi-ai-lab (2026)."
domain: bioinfo-pipeline
---

# DeerFlow 代码结构与业务流转指南

**来源：** 万智创界（微信公众号，作者 walton）
**GitHub：** https://github.com/wanrengang/wanzhi-ai-lab

---

## 一、全局架构

```
deer-flow/
├── frontend/   ← Next.js 16 (React 19 + TypeScript)
├── backend/    ← Python 后端 (FastAPI + LangGraph)
├── skills/     ← Skill 定义文件
├── docker/     ← Docker Compose
├── config.yaml ← 主配置
└── Makefile    ← 开发命令入口
```

### 运行时进程拓扑（4进程）

```
浏览器 → http://localhost:2026
              │
              ▼
      ┌────────── Nginx (2026) ──────────┐
      │  /api/langgraph/* → LangGraph :2024 │
      │  /api/*           → Gateway :8001   │
      │  /                → Frontend :3000  │
      └────────────────────────────────────┘
```

---

## 二、核心业务流转（6条主线）

### 1. 用户发消息 → Agent回复
前端 `useThreadStream()` → LangGraph SDK → Nginx → LangGraph Server → `make_lead_agent()` → Agent ReAct循环 → SSE流式返回

### 2. 中间件处理链（14个）
ThreadData → Uploads → Sandbox → DanglingToolCall → ToolError → Summarization → Todo → TokenUsage → Title → Memory → ViewImage → DeferredToolFilter → SubagentLimit → LoopDetection → Clarification

### 3. 工具调用流程
LLM返回tool_calls → sandbox工具/bash/read_file/write_file → task_tool（子Agent）→ MCP工具 → ToolMessage回LLM

### 4. Skills加载
启动时扫描SKILL.md → 解析YAML frontmatter → 渐进式注入System Prompt → 按需加载完整内容

### 5. 记忆系统
MemoryMiddleware触发 → MemoryQueue(debounce) → MemoryUpdater(LLM提取) → MemoryStore(memory.json) → 下次对话注入System Prompt

### 6. 文件上传
前端上传 → Gateway接收 → UploadManager转换(PDF→Markdown等) → UploadsMiddleware注入上下文

---

## 三、两种运行模式

- **Standard Mode（默认4进程）：** Frontend + Nginx + Gateway + LangGraph Server
- **Gateway Mode（实验性3进程）：** Frontend + Nginx + Gateway（内嵌Agent运行时）

## 四、技术栈

| 层 | 技术 |
|:---|:---|
| 前端 | Next.js 16 + React 19 + Tailwind CSS 4 + Shadcn UI |
| 状态 | TanStack Query v5 |
| AI SDK | @langchain/langgraph-sdk |
| 后端 | LangGraph 1.0.6+ + FastAPI |
| Python | 3.12+，包管理 uv |
| 沙箱 | Local 或 Docker |
| 模型 | OpenAI兼容API（任意模型） |

## 五、关键文件速查

| 需求 | 文件 |
|:---|:---|
| Agent创建 | `agents/lead_agent/agent.py` → `factory.py` |
| 中间件顺序 | `_build_middlewares()` |
| 对话流式传输 | `frontend/src/core/threads/hooks.ts` (useThreadStream) |
| System Prompt | `agents/lead_agent/prompt.py` |
| 工具注册 | `tools/tools.py::get_available_tools()` |
| 子Agent执行 | `subagents/executor.py` |
| 记忆系统 | `agents/memory/updater.py` + `storage.py` |
| Skills加载 | `skills/loader.py` + `parser.py` |
| MCP集成 | `mcp/client.py` + `tools.py` |
| LangGraph配置 | `langgraph.json` |
