# Multi-Agent Supply Chain Control Tower

一个面向供应链场景的多智能体控制塔项目，聚焦于“可解释的业务决策 + 数学建模辅助 + 可视化展示 + 问答式交互”。

本项目适合以下用途：
- 供应链控制塔原型演示
- 多智能体 / LangGraph 工作流实验
- 供应链业务问答与决策解释
- 数学建模方法推荐与案例化展示
- 课程设计、论文原型、作品集与 GitHub 展示

---

## 1. 项目定位

这个项目不是单一功能脚本，而是一个围绕供应链业务问题构建的综合原型系统。

它主要做三件事：

1. **控制塔决策**：
   针对库存、供应商、物流、风险等问题，使用多智能体工作流生成结构化分析与决策建议。

2. **统一问答入口**：
   用户可以通过问句进入系统，由后端识别问题类型，再路由到合适的分析链路，例如控制塔决策、图谱证据检索、制度问答或数学建模推荐。

3. **建模与可视化展示**：
   项目已经支持生成供应链建模相关的图表、案例 notebook、方法候选表、以及 `output/model` 中的一组演示型可视化结果。

---

## 2. 当前项目已经具备的功能

### 2.1 多智能体控制塔工作流

在 `src/agents/` 和 `src/nodes/` 中实现了供应链控制塔的核心流程，包括：

- `Demand Agent`：需求风险识别
- `Inventory Agent`：库存与补货判断
- `Risk Agent`：供应与履约风险判断
- `Logistics Agent`：物流与加急建议
- `Coordinator Agent`：汇总各 Agent 输出，给出最终建议

同时，系统具备：
- 决策闸门（decision gate）
- 人工审批（human approval）
- 执行节点（execution）
- 状态流转与可追溯解释

### 2.2 统一问答入口

在 `src/backend_interface.py` 中提供两个核心入口：

- `run_one_cycle(product_id=1)`
  - 运行一次控制塔决策流程
  - 适合直接测试某个产品的供应链决策

- `run_total_chat_cycle(question, product_id=1, ...)`
  - 统一问答入口
  - 支持“自然语言问题 -> 识别意图 -> 路由到内部能力链路 -> 返回结构化结果”

### 2.3 Streamlit 前端界面

在 `ui/app.py` 中提供了一个可直接运行的前端页面，支持：

- 选择产品
- 输入问题
- 查看结论摘要
- 查看 LangGraph 工作流步骤
- 查看证据链与组件激活情况
- 查看后端返回的分析结果

适合用来做演示、汇报、课堂展示或答辩演示。

### 2.4 数学建模能力与方法推荐

项目在 `src/modeling_*.py` 中补充了一组面向数学建模的能力，支持：

- 问题类型识别
- 建模目标与约束抽取
- 决策变量建议
- 候选方法推荐
- 当前数据覆盖率评估
- 方法排序与提示模板生成

目前已经覆盖的典型建模方向包括：
- 库存优化
- 供应商选择
- 需求预测
- 物流优化
- 风险仿真
- 网络影响传播

### 2.5 数据与图谱相关模块

项目已经包含：
- SQLite 演示数据库能力
- Neo4j 相关连接/转换脚本
- 图谱证据检索能力
- 公开演示数据抓取与导入脚本

相关模块位于：
- `src/db_init.py`
- `src/db_service.py`
- `src/models.py`
- `sqlite_to_neo4j.py`
- `src/graph_evidence.py`
- `src/public_data_ingest.py`
- `src/fetch_public_datasets.py`

### 2.6 可视化输出

项目已经能生成多类静态图表与架构图，输出目录包括：

- `output/`：总体架构图、流程图、知识图谱示意图
- `output/model/`：库存、供应商、物流、风险相关的演示型可视化图表

例如：
- `inventory_dashboard.png`
- `supplier_dashboard.png`
- `logistics_dashboard.png`
- `risk_dashboard.png`
- `19_code_architecture_overview.svg`

这些文件适合直接用于 README 展示、答辩材料、报告或项目说明。

---

## 3. 项目目录说明

```text
Multi-Agent-Supply-Chain-Control-Tower/
├─ src/                     # 后端核心逻辑
│  ├─ agents/               # 多智能体实现
│  ├─ nodes/                # LangGraph 节点
│  ├─ graph.py              # 控制塔图工作流
│  ├─ total_graph.py        # 总问答入口图
│  ├─ backend_interface.py  # UI / 外部调用统一入口
│  ├─ modeling_engine.py    # 数学建模引擎
│  ├─ modeling_methods.py   # 候选建模方法定义
│  ├─ modeling_intent.py    # 建模意图识别
│  ├─ modeling_execution.py # 建模执行相关逻辑
│  ├─ db_service.py         # 数据库读取服务
│  ├─ db_init.py            # 数据库初始化与示例数据
│  ├─ graph_evidence.py     # 图谱证据检索
│  ├─ main.py               # 控制塔 CLI 入口
│  └─ main_total.py         # 总问答 CLI 入口
├─ ui/                      # Streamlit 前端
│  ├─ app.py
│  └─ helpers.py
├─ utils/                   # 通用分析与格式转换模块
├─ output/                  # 架构图与可视化输出
├─ demo/                    # 轻量演示脚本
├─ docs/                    # 说明文档与 notebook
├─ total*.ipynb             # 业务说明 / 演示 notebook
├─ requirements.txt         # Python 依赖
├─ run_ui.bat               # Windows 一键启动 UI
└─ sqlite_to_neo4j.py       # SQLite -> Neo4j 转换脚本
```

---

## 4. 环境初始化与依赖安装

### 4.1 Python 版本建议

建议使用：
- **Python 3.11 或 3.12**

### 4.2 创建虚拟环境

Windows PowerShell：

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Windows CMD：

```bat
python -m venv .venv
.venv\Scripts\activate
```

macOS / Linux：

```bash
python -m venv .venv
source .venv/bin/activate
```

### 4.3 安装依赖

```bash
pip install -r requirements.txt
```

项目当前依赖主要包括：
- LangGraph / LangChain
- Streamlit
- SQLAlchemy
- Pandas
- FastAPI / Uvicorn
- Neo4j
- ChromaDB
- Pydantic
- python-dotenv

---

## 5. 环境变量配置

在项目根目录新建 `.env` 文件。常见配置示例：

```env
OPENROUTER_API_KEY=your_key_here
OPENAI_API_KEY=your_key_here
OPENAI_BASE_URL=https://your-openai-compatible-endpoint
OPENAI_MODEL=gpt-4o-mini
LANGCHAIN_TRACING_V2=true
LANGCHAIN_PROJECT=supply-chain-control-tower
U3_RETRIEVAL_MODE=auto
U3_REMOTE_RETRIEVAL_BASE_URL=http://127.0.0.1:8000
```

实际需要哪些变量，取决于你使用的模型服务与推理链路。

U3 检索新增支持：
- `U3_RETRIEVAL_MODE=auto|remote|local`
  - `auto`：优先调用远端检索服务，失败自动回退本地 Chroma/词法检索
  - `remote`：只走远端检索服务
  - `local`：只走本地检索
- `U3_REMOTE_RETRIEVAL_BASE_URL`：远端检索服务地址（例如 `new_xinpian`）

GRAPH 文档证据增强支持：
- `GRAPH_REMOTE_RETRIEVAL_MODE=auto|required|off`
  - `auto`：在 GRAPH/SUPPLIER_* 路径附加远端文档证据，失败不阻断主流程
  - `required`：远端文档证据不可用时直接报错
  - `off`：关闭远端文档证据增强
- `GRAPH_REMOTE_RETRIEVAL_BASE_URL`：GRAPH 文档证据检索服务地址（默认复用 `U3_REMOTE_RETRIEVAL_BASE_URL`）

如果只做静态代码阅读、部分数据处理或图表生成，不一定需要完整 LLM 配置；
但如果你要运行完整问答链路或控制塔决策链，通常需要准备可用的 API Key。

---

## 6. 如何运行项目

### 6.1 启动 Streamlit 前端

推荐方式：

```bat
run_ui.bat
```

或者手动启动：

```bash
streamlit run ui/app.py --server.port 8501 --server.address 127.0.0.1
```

浏览器访问：

- `http://127.0.0.1:8501`

### 6.2 运行控制塔后端流程

```bash
python -m src.main
```

这个入口会：
- 初始化数据库
- 导入示例数据
- 构建 LangGraph
- 对多个产品运行控制塔决策流程
- 在命令行打印结果摘要

### 6.3 运行统一问答入口

```bash
python -m src.main_total --question "当前应该优先补货还是更换供应商？" --product-id 1
```

这个入口适合测试：
- 问题识别
- 路由逻辑
- 总问答输出结构

---

## 7. 其他人可以直接复用哪些模块

如果别人想基于你的项目做二次开发，最推荐从下面这些模块开始：

### 7.1 后端统一调用入口

- `src/backend_interface.py`

推荐原因：
- 不需要直接接触所有 agent
- UI、本地脚本、外部接口都可以从这里进入
- 结构最清晰，适合二次封装

### 7.2 控制塔工作流

- `src/graph.py`
- `src/state.py`
- `src/agents/`
- `src/nodes/`

适合用途：
- 改造 LangGraph 工作流
- 新增 Agent
- 新增规则节点 / 审批节点 / 执行节点

### 7.3 统一问答与总图流程

- `src/total_graph.py`
- `src/main_total.py`

适合用途：
- 做问答式供应链助手
- 接到 Web API / 企业知识助手中
- 做意图路由和多能力整合

### 7.4 数学建模模块

- `src/modeling_engine.py`
- `src/modeling_methods.py`
- `src/modeling_intent.py`
- `src/modeling_execution.py`
- `src/ontology_rules.py`

适合用途：
- 方法推荐系统
- 业务问题到数学模型的自动映射
- 可解释建模报告生成
- 数据覆盖率与候选模型评估

### 7.5 数据与数据库模块

- `src/db_service.py`
- `src/db_init.py`
- `src/models.py`
- `src/public_data_ingest.py`
- `src/fetch_public_datasets.py`

适合用途：
- 数据层重构
- 接企业数据库
- 换成 PostgreSQL / MySQL / 云数据库
- 扩展公开样例数据

### 7.6 图谱与证据模块

- `src/graph_evidence.py`
- `sqlite_to_neo4j.py`

适合用途：
- 图谱证据追踪
- SQLite 与 Neo4j 联动
- 供应链关系图扩展

### 7.7 前端界面

- `ui/app.py`
- `ui/helpers.py`

适合用途：
- 快速做一个可演示的供应链助手页面
- 二次美化 UI
- 接入更多状态面板、图表与说明卡片

### 7.8 工具模块

- `utils/problem_analysis.py`
- `utils/mathematical_modeling.py`
- `utils/computational_solving.py`
- `utils/solution_reporting.py`
- `utils/convert_format.py`

适合用途：
- 抽离为独立分析工具
- 生成结构化问题分析结果
- 辅助自动写报告或 notebook

---

## 8. 推荐阅读顺序

如果别人第一次接触这个项目，建议按下面顺序理解：

1. 先看 `README.md`
2. 再看 `ui/app.py`，理解用户入口
3. 再看 `src/backend_interface.py`，理解统一后端入口
4. 再看 `src/graph.py` 和 `src/total_graph.py`，理解两条核心链路
5. 再看 `src/agents/` 与 `src/nodes/`
6. 最后看 `src/modeling_*.py` 和 `output/`、`docs/`、`total*.ipynb`

这样会比直接钻进单个脚本更容易把整个项目看懂。

---

## 9. 当前适合上传 GitHub 的内容

建议保留：
- `src/`
- `ui/`
- `utils/`
- `output/`
- `docs/`
- `demo/`
- `requirements.txt`
- `run_ui.bat`
- `sqlite_to_neo4j.py`
- `total*.ipynb`
- `README.md`

建议不要提交：
- `.env`
- 本地数据库缓存
- 本地模型缓存之外的无关大文件
- 临时调试脚本
- `.ipynb_checkpoints/`

我已经删除了本轮编辑过程中产生的一批临时 `.py` 文件，避免污染仓库结构。

---

## 10. 后续可以继续完善的方向

如果你准备把它作为正式公开项目，接下来还可以继续补：

1. 增加 `.env.example`
2. 增加数据库初始化说明
3. 补一个更完整的 `docs/architecture.md`
4. 增加 API 启动方式说明
5. 给 `src/modeling_*` 模块补单元测试
6. 清理 notebook 副本文件，例如：`total2 - 副本.ipynb`、`total5 - 副本.ipynb`
7. 为 GitHub 增加项目首页截图

---

## 11. 一句话总结
这是一个把 **多智能体工作流、供应链决策、数学建模推荐、图谱证据、可视化展示和问答式交互** 结合在一起的供应链控制塔原型项目，适合用于演示、研究、课程项目和二次开发。

---

## 12. 许可证

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
