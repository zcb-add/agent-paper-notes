# Memory in the Age of AI Agents: A Survey

- **标题**：Memory in the Age of AI Agents: A Survey
- **会议/年份**：arXiv 2512.13564, 2025.12
- **链接**：https://arxiv.org/abs/2512.13564
- **类型**：综述
- **分组**：综述
- **状态**：🔄 在读

> **为什么读它**：最新记忆综述。用 Forms（token/参数/隐式）× Functions（事实/经验/工作）× Dynamics（形成/演化/检索）三轴分类，明确把 agent memory 与 RAG、context engineering 切开 —— 用它划 C2 的边界。

---

## 1. 解决什么问题（一句话）

社区里"agent memory"这个词已经**指代不清** —— 有人拿它指 LLM 内部的 KV cache / 架构改造，有人指 RAG，有人指 context 压缩；于是"都叫 agent memory 的论文，实现、目标、假设差得离谱"。本文重下一套定义与分类，并**明确划出边界**（哪些不算 agent memory）。

作者自陈两个动机：
1. **旧分类过时** —— 2025 年的新方向（从经验里蒸馏可复用工具、记忆增强的 test-time scaling）在早先综述里没位置
2. **概念碎片化** —— declarative / episodic / semantic / parametric 等术语泛滥

## 2. 核心机制（≤5 行）

**三轴（Forms–Functions–Dynamics）：**

| 轴 | 问的问题 | 取值 |
|---|---|---|
| **Forms** | 什么承载记忆？ | ① **token 级**（平面 2D / 分层 3D）② **参数化**（内部 / 外部）③ **隐式/latent**（生成 / 重用 / 变换） |
| **Functions** | 为什么需要？ | ① **事实记忆**（环境事实）② **经验记忆**（case / policy / skill / 混合）③ **工作记忆**（单轮 / 多轮） |
| **Dynamics** | 怎么运作演化？ | **形成** → **演化** → **检索** |

**边界三条（本文最值钱的部分）：**

- **vs LLM memory**：agent memory **几乎完全包含**了传统说的 "LLM memory"（MemGPT、MemoryBank 早期自称 LLM memory，按现代定义就是 agent memory）。**但不算的那部分**：直接改模型内部状态的 —— KV cache 管理、cache 重写、循环状态持久化、注意力稀疏、架构改动（RWKV / Mamba / diffusion LM）、长上下文机制。理由：它们**不跨任务持久化、不参与形成/演化/检索、不做环境驱动的适应**。
- **vs RAG**：**技术栈高度重合**（向量索引、语义检索、图结构、context 扩展模块）。差别不在技术，在**任务域**：RAG 服务**单次推理**检索外部静态知识（HotpotQA / 2WikiMQA / MuSiQue）；agent memory 在**多轮持续交互**里积累（LoCoMo / LongMemEval / GAIA / BrowseComp / SWE-bench / StreamBench）。**作者自己承认边界在糊**（HippoRAG/HippoRAG2 被 RAG 和记忆两个社区**同时认领**）。最近的是 **agentic RAG**（PlanRAG、Self-RAG）—— 唯一区别：classical agentic RAG 操作**外部、任务专属**库；agent memory 是**内部、持久、自演化**库。
- **vs context engineering**：**不是包含关系，是交叉**。context engineering 把 context window 当**受限资源**调度（工具调用格式、工具选择、通信协议 MCP、token 剪枝、KV 压缩）；agent memory 管的是**认知状态**（知道什么 / 经历过什么 / 怎么演化）。作者原话：context engineering 是"外部脚手架"，agent memory 是"内部基质"。**共享原语**：rolling summary 同时是 buffer 管理策略**和**瞬时情节记忆 —— 短程上两者边界**直接消失**。

## 3. 评测怎么做

- **数据集**：论文分两类（Table 8）：
  - **① 记忆/终身学习/自演化专用**：MemBench、**LoCoMo**(300 s)、**LongMemEval**(5 能力/500 题)、MemoryBank、MemoryBench、LifelongAgentBench、StreamBench、DialSim、PersonaMem、PerLTQA、PrefEval、HaluMem（记忆幻觉）、LOCCO、StoryBench、Madial-Bench、RULER / BABILong / LongBench(v2) / MM-Needle（长上下文）、MemoryAgentBench、Evo-Memory
  - **② 本来为别的目的、但顺带压测记忆**：ALFWorld / ScienceWorld / BabyAI（具身）、WebShop / WebArena / MMInA / ToolBench（web + 工具）、GAIA / xBench-DS / BrowseComp（deep research）、**SWE-Bench Verified**（代码）
  - 另一个维度：**Fac.（事实）/ Exp.（经验）/ MM.（多模态）/ Env.（simulated vs real）**
- **指标**：⚠️ **论文没有给统一的指标清单**。Table 8 的列只有 Fac./Exp./MM./Env./Feature/Scale —— **没有"指标"这一列**。实际评测各基准各用各的（accuracy / F1 / LLM-judge）。**这是我读出来的一个明确空白，见第 5 节。**
- **baseline**：Table 9 列了 ~25 个开源框架。**报出来的评测基准高度集中在 LoCoMo** —— MemGPT、Mem0、Memobase、MIRIX、MemoryOS、LangMem、SuperMemory、Memary、Pinecone、Chroma、Weaviate 等；少数报了 LongMemEval（MemOS）/ MemoryBank / PrefEval / PersonaMem（MemOS）/ PerLTQA。
- **是否开源**：综述开源，配套论文清单仓库 **Shichun-Liu/Agent-Memory-Paper-List（2380 星）**；Table 9 里的框架绝大多数有 GitHub。
- ⚠️ **提取警告**：pdftotext 把 Table 8 / Table 9 的列**打乱错位了**，框架 ↔ 结构 ↔ 基准的配对**不可靠**。要引用具体某框架报哪个基准，**回原 PDF 的表格核对**。

## 4. 我能复现吗

- **成本**：综述**本身没有可复现实验**。它的价值是**选型地图**（Table 9 直接告诉你哪个框架报了什么基准），不是可跑的 artifact。
- **有无 harness（现成评测脚本）**：综述无。但 README 里那个 **NirDiamant/Agent_Memory_Techniques（1054 星，30 个可跑 notebook）** 覆盖 MemGPT / Mem0 / Letta / Zep / LoCoMo —— 复现 baseline 从那里起步最快。
- **预估工时**：通读 + 填卡 2–3 天（已花半天）。复现另算，见下。

## 5. 我没被说服的地方

<!-- 最重要的一节。攒 20 条，就是 C1 的立项理由。 -->
<!-- 下面是我（用户）自己要判断的地方，Claude 只列了读出来的"松动点"，结论请自己下 -->

**读出来的松动点（待我自己判断）：**

1. **边界是"叙事性"的，不是可操作的。** 作者承认 HippoRAG 两个社区都认领、承认 RAG 和记忆的边界"越来越糊"，最后给的区分标准退化成"**看你投哪个 benchmark**"。—— 一个靠**会议/数据集习俗**而不是靠机制来定义的边界，能不能作为 C2 的立项依据？
2. **评测的"指标"整节缺失。** Table 8 有 Fac./Exp./MM./Env./Feature/Scale，**唯独没有"怎么打分"**。综述敢下"这是记忆基准"的判断，却不给统一的度量口径 —— 这跟第 12 篇（Anatomy of Agentic Memory）说"现有评测规模不足、指标脱节、忽略延迟"是不是同一件事？**两条独立来源指向同一个洞，这个洞可能就是 C1。**
3. **baseline 被 LoCoMo 垄断，但 README 说 LoCoMo 已饱和。** 综述 Table 9 里十几个框架只报 LoCoMo；你自己在 README 里已经写了"LoCoMo 已饱和，别当唯一评测"。**综述还在拿一个饱和基准当事实标准** —— 是综述滞后，还是社区确实没得选？没得选 = C1 的机会。
4. **三轴之间没打通。** Forms × Functions × Dynamics 是三个正交切面，但论文**没有回答"哪种 Form 配哪种 Function 最好"** —— 作者自己在 Contribution (2) 里说"provides an in-depth discussion on the suitability and interplay"，可真到正文是否有实验/证据支撑这个"适配矩阵"，我还没读到。如果只是拍脑袋的分类表，那 C2 的"哪个机制点值得改"就还得自己找。
5. **"遗忘"几乎没位置。** 整篇读下来 Forms/Functions/Dynamics 三轴里**没有独立的"遗忘"轴** —— 演化（evolution）里带一点，但 MemoryBank 那种认真做遗忘曲线的只作为被引用的例子。README 里你标的 C1 靶子"几乎不评写入、遗忘、巩固、token 经济性"，在这里**被再次印证**。

## 6. 对我的用处（C1 数据集 / C2 机制 / C3 实验 / C4 系统）

- **C2（记忆机制点）→ 直接用三轴当选题坐标系。** "我要改记忆"这句话必须落到某一格：
  - Form 选哪层？（token 级分层 / 参数化 / latent —— latent 和参数化是**新**的、人少）
  - Function 选哪个？（事实 / 经验 / 工作 —— **经验记忆**里的 skill 和 policy 是 2025 新方向）
  - Dynamics 的三个环节里改哪个？（**形成、演化**比检索人少；检索最卷）
- **C1（数据集/评测）→ Table 8 就是现成的靶子图**，且**缺指标列**这一点本身可能就是立项切口（见第 5 节第 2 条）。
- **C3（实验）→ baseline 已被 LoCoMo 垄断**，意味着**换一套评测协议（写入 / 遗忘 / 巩固 / token 成本）本身就有贡献空间**，不用赢在分数上。
- **C4（系统）→ Table 9 的 ~25 个框架是现成的系统层**，MemOS / MemGPT / Zep 可直接当后端。
- **划界用途**：写开题时"我的工作和 RAG 的区别"这一段，可以直接引本文 2.3.2 的**任务域**论证，比自造边界省事。

## 7. 顺藤摸瓜（它引用的、我要接着读的）

| 优先级 | 论文 | 为什么 |
|---|---|---|
| ★★★ | **Anatomy of Agentic Memory**（卡片 12） | 本文第 5 节的松动点 2/5 就是它在批评的东西，两篇要对着读 |
| ★★★ | **LongMemEval**（卡片 10）、**LoCoMo**（卡片 11） | 综述的事实 baseline；11 已知饱和，10 是替代候选 |
| ★★☆ | MemGPT（卡片 04）、Mem0（卡片 06）、Zep（卡片 08） | Table 9 里的主力框架，也是 README 定的复现 baseline A |
| ★★☆ | **LightMem**（卡片 09） | 本文没重点讲**成本视角**，但那可能是缝 |
| ★☆☆ | MemoryBank（卡片 07） | 唯一认真做**遗忘曲线**的 —— 对应松动点 5 |
| ★☆☆ | MemOS（Li et al., 2025l） | 综述里少数报 LongMemEval + PersonaMem 的框架 |
| ★☆☆ | Zhang et al. 2025s / Wu et al. 2025g | 本文点名批评的**旧分类综述** —— 看它怎么过时，也是缝隙所在 |
| ★☆☆ | NirDiamant/Agent_Memory_Techniques | 不是论文，是 30 个 notebook 的复现起点 |

---
**读完标记**：⬜ 待办 → 回原 PDF 核对 Table 8 / Table 9 的错位；补第 5 节自己的结论
