# Multi-Agent Supply Chain Control Tower

<p align="center">
  <strong>基于 LangGraph 的多智能体供应链控制塔 | RAG 增强决策 | 可解释 AI</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/LangGraph-0.2-blue" alt="LangGraph">
  <img src="https://img.shields.io/badge/RAG-Enabled-green" alt="RAG">
  <img src="https://img.shields.io/badge/Multi--Agent-5_Agents-orange" alt="Multi-Agent">
  <img src="https://img.shields.io/badge/Python-3.11%2B-blue" alt="Python">
  <img src="https://img.shields.io/badge/License-Apache_2.0-green" alt="License">
</p>

---

## 核心亮点

| 创新点 | 说明 |
|--------|------|
| **LangGraph 工作流** | 基于状态机的有向图编排，支持条件分支、循环、人工审批，实现可追溯的决策链路 |
| **RAG 检索增强** | ChromaDB + 远端检索服务双引擎，支持自动/远程/本地三种检索模式，失败自动回退 |
| **多智能体协同** | 5 个专业 Agent（需求/库存/风险/物流/协调）分工协作，模拟真实供应链决策流程 |
| **图谱证据追踪** | Neo4j 知识图谱 + SQLite 关系数据联动，每条决策建议都可溯源到具体证据 |
| **数学建模引擎** | 业务问题 → 意图识别 → 方法推荐 → 可解释报告，覆盖库存优化、供应商选择等 6 大场景 |
| **统一问答入口** | 自然语言提问 → 意图路由 → 多能力整合 → 结构化输出，一站式解决供应链问题 |

---

## 架构概览

```
┌─────────────────────────────────────────────────────────────┐
│                      Streamlit UI                            │
│                   (ui/app.py)                                │
└──────────────────────────────┬──────────────────────────────┘
                               │
                    ┌──────────▼──────────┐
                    │  backend_interface   │
                    │  (统一入口)           │
                    └──────────┬──────────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
     ┌────────▼────────┐ ┌────▼─────┐ ┌───────▼────────┐
     │  Control Tower   │ │  Total   │ │  Modeling      │
     │  LangGraph       │ │  Chat    │ │  Engine        │
     │  (graph.py)      │ │  Graph   │ │  (modeling_*)  │
     └─────────────────┘ │(total_   │ └───────┬────────┘
              │           │ graph.py) │         │
     ┌────────▼─────────┐ └────┬─────┘ ┌───────▼────────┐
     │  Multi-Agent     │      │       │  Intent        │
     │  • Demand        │      │       │  Recognition   │
     │  • Inventory     │      │       │  Method        │
     │  • Risk          │      │       │  Recommendation│
     │  • Logistics     │      │       └────────────────┘
     │  • Coordinator   │      │
     └────────┬─────────┘      │
              │                │
     ┌────────▼────────────────▼────────┐
     │         Data Layer                │
     │  SQLite ↔ Neo4j ↔ ChromaDB       │
     │  (db_service + graph_evidence)    │
     └───────────────────────────────────┘
```

---

## 快速开始

### 1. 环境准备

```bash
# 创建虚拟环境
python -m venv .venv
.\.venv\Scripts\Activate.ps1  # Windows PowerShell
# source .venv/bin/activate   # macOS/Linux

# 安装依赖
pip install -r requirements.txt
```

### 2. 配置环境变量

在项目根目录创建 `.env` 文件：

```env
OPENAI_API_KEY=your_key_here
OPENAI_MODEL=gpt-4o-mini
U3_RETRIEVAL_MODE=auto
U3_REMOTE_RETRIEVAL_BASE_URL=http://127.0.0.1:8000
```

### 3. 运行项目

```bash
# 方式一：启动 Streamlit 前端（推荐）
run_ui.bat
# 浏览器访问 http://127.0.0.1:8501

# 方式二：运行控制塔后端
python -m src.main

# 方式三：统一问答入口
python -m src.main_total --question "当前应该优先补货还是更换供应商？" --product-id 1
```

---

## 核心模块

### LangGraph 控制塔工作流 (`src/graph.py`)

基于 LangGraph 构建的有向状态图，实现供应链决策的完整链路：

```
问题输入 → 决策闸门 → [Demand Agent] → [Inventory Agent] → [Risk Agent] → [Logistics Agent] → Coordinator → 输出建议
                        ↑                    ↑                  ↑                ↑
                   条件分支              条件分支            条件分支          条件分支
```

**关键特性：**
- **条件路由**：根据问题类型动态激活相关 Agent
- **人工审批**：关键决策节点支持人工介入
- **状态追溯**：每个节点的输入输出均可追溯

### RAG 检索增强 (`src/graph_evidence.py`, `src/u3_rag.py`)

双引擎检索架构，支持三种模式：

| 模式 | 行为 | 适用场景 |
|------|------|----------|
| `auto` | 优先远端检索，失败回退本地 Chroma | 生产环境 |
| `remote` | 仅远端检索 | 有稳定检索服务 |
| `local` | 仅本地 Chroma/词法检索 | 离线环境 |

### 多智能体架构 (`src/agents/`)

| Agent | 职责 | 输出 |
|-------|------|------|
| **Demand Agent** | 需求风险识别 | 需求波动分析、预测偏差 |
| **Inventory Agent** | 库存与补货判断 | 安全库存、补货建议 |
| **Risk Agent** | 供应与履约风险 | 供应商风险评分、替代方案 |
| **Logistics Agent** | 物流与加急建议 | 运输方案、加急成本评估 |
| **Coordinator** | 汇总决策 | 最终建议、优先级排序 |

### 数学建模引擎 (`src/modeling_*.py`)

```
业务问题 → 意图识别 → 约束抽取 → 方法推荐 → 可解释报告
```

覆盖场景：库存优化、供应商选择、需求预测、物流优化、风险仿真、网络影响传播

---

## 项目结构

```
Multi-Agent-Supply-Chain-Control-Tower/
├── src/
│   ├── agents/           # 5 个专业 Agent 实现
│   ├── nodes/            # LangGraph 节点（决策闸门/审批/执行）
│   ├── graph.py          # 控制塔 LangGraph 工作流
│   ├── total_graph.py    # 统一问答图
│   ├── backend_interface.py  # 统一 API 入口
│   ├── modeling_*.py     # 数学建模引擎
│   ├── graph_evidence.py # 图谱证据检索
│   ├── db_*.py           # 数据库服务
│   └── u3_rag.py         # RAG 检索模块
├── ui/
│   ── app.py            # Streamlit 前端
├── utils/                # 通用工具模块
├── output/               # 可视化输出（架构图/仪表盘）
└── requirements.txt
```

---

## 技术栈

- **工作流编排**：LangGraph / LangChain
- **前端**：Streamlit
- **数据库**：SQLite / Neo4j
- **向量检索**：ChromaDB
- **API 框架**：FastAPI / Uvicorn
- **数据模型**：Pydantic
- **数据处理**：Pandas

---

## 许可证

本项目采用 [Apache License 2.0](LICENSE) 开源协议。

**你可以：**
- 查看、学习和研究本项目代码
- 基于本项目进行二次开发和修改
- 将本项目用于个人或商业用途

**你需要：**
- 在分发时保留原始版权声明和许可证
- 对修改过的文件添加明显的修改说明
- 保留所有版权、专利、商标和归属声明

**你不能：**
- 使用本项目的商标、服务标记或产品名称（除非合理描述来源）
- 要求作者承担任何担保或责任

如需商业授权或有其他合作需求，请联系 [Neko-Agents](https://github.com/Neko-Agents)。
