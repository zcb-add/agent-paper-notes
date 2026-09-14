# Agent 论文精读笔记

> **zcb-add** · 新疆大学 · 2026 级硕士（研一）
> 方向：**大模型 × Agent** ｜ 毕业线：一篇三区 ｜ 目标：研二早实习

一份**边读边攒**的文献库。研一上（2026.9–2027.1）的目标是精读 20 篇（含 3 篇综述），
攒够"现有工作的缝"，产出一页纸选题提案。

## 论文路线

| | 内容 |
|---|---|
| **① 领域应用 + 评测** | C1 领域数据集 / 评测基准（开源） |
| **② 单点机制改进** | C2 **只选一个**机制点 → **记忆**（备选：工具调用 / 检索） |
| C3 实验 | 裸模型 vs prompt 工程 vs RAG vs 通用 Agent 框架 + 消融 + 人工评估 |
| C4 系统与开源 | 求职线复用：MCP、并发、可观测性 |

> ⚠️ **领域 X 待定** —— 取决于导师横向项目的具体领域。
> 好消息：下面 **A/B/C 三组不依赖领域 X**，现在就能开读。

## 怎么用这个仓库

1. 打开 `cards/` 里**序号最小、状态还是 ⬜** 的那张卡
2. 对照论文填 1–7 节 —— **第 5 节「我没被说服的地方」最重要**，攒 20 条就是 C1 的立项理由
3. 填完把本表状态改成 ✅

状态图例：⬜ 未读 ｜ 🔄 在读 ｜ ✅ 已读

---

## A. 综述（3 篇，先读）

| # | 论文 | 链接 | 状态 |
|---|---|---|---|
| 01 | Memory in the Age of AI Agents: A Survey | [2512.13564](https://arxiv.org/abs/2512.13564) | 🔄 |
| 02 | Survey on Evaluation of LLM-based Agents（Yehudai et al.） | [2503.16416](https://arxiv.org/abs/2503.16416) | ⬜ |
| 03 | Evaluation and Benchmarking of LLM Agents: A Survey | [2507.21504](https://arxiv.org/abs/2507.21504) | ⬜ |

**读法**：01 划出 C2 的边界（它明确把 agent memory 与 RAG、context engineering 切开）；
02/03 是 C1 的靶子图，看它们怎么给"记忆能力"分类、怎么评。

## B. 记忆 · 机制（C2 的候选池）

| # | 论文 | 链接 | 状态 |
|---|---|---|---|
| 04 | MemGPT — 虚拟上下文分页（working / recall / archival 三层） | [2310.08560](https://arxiv.org/abs/2310.08560) | ⬜ |
| 05 | A-MEM — Zettelkasten 式自组织记忆网络 | [2502.12110](https://arxiv.org/abs/2502.12110) | ⬜ |
| 06 | Mem0 — 原子化事实 + 写入时去重 + 图记忆（生产向） | [2504.19413](https://arxiv.org/abs/2504.19413) | ⬜ |
| 07 | MemoryBank — 艾宾浩斯遗忘曲线（少数认真做**遗忘**的） | [2305.10250](https://arxiv.org/abs/2305.10250) | ⬜ |
| 08 | Zep / Graphiti — 时序知识图谱做记忆 | [2501.13956](https://arxiv.org/abs/2501.13956) | ⬜ |
| 09 | LightMem — 轻量高效路线（成本视角） | [2510.18866](https://arxiv.org/abs/2510.18866) | ⬜ |

## C. 记忆 · 评测（C1 / C3 的地基）

| # | 论文 | 链接 | 状态 |
|---|---|---|---|
| 10 | LongMemEval — 500 题 / 5 种能力（含**知识更新**与**拒答**） | [2410.10813](https://arxiv.org/abs/2410.10813) | ⬜ |
| 11 | LoCoMo — 多会话对话记忆（ACL 2024，**已饱和**，别当唯一评测） | [2402.17753](https://arxiv.org/abs/2402.17753) | ⬜ |
| 12 | Anatomy of Agentic Memory — 批判现有评测：规模不足、指标脱节、忽略延迟 | 搜标题 | ⬜ |

**读法**：重点记 12 的批评清单 —— 它说现在的记忆评测**主要在评"读"（检索）**，
几乎不评写入、遗忘、巩固、token 经济性。**这句话就是 C1 的立项理由。**

## D. 工具调用 / 检索（备选线，若 C2 不选记忆）

| # | 论文 | 链接 | 状态 |
|---|---|---|---|
| 13 | StableToolBench — 缓存/模拟 API，解决工具评测不可复现 | ACL 2024 Findings | ⬜ |
| 14 | τ-bench（τ²-bench）— 端到端任务完成，跨轮维持状态 | Sierra | ⬜ |
| 15 | BFCL v3 / v4 — 函数调用主榜 | Berkeley Gorilla | ⬜ |
| 16 | ComplexFuncBench — 多步 + 长上下文函数调用 | 2025 | ⬜ |
| 17 | Self-RAG — 反思 token 自评（ICLR 2024 Oral） | [2310.11511](https://arxiv.org/abs/2310.11511) | ⬜ |
| 18 | CRAG — 轻量检索评估器，正确/模糊/错误三分支 | [2401.15884](https://arxiv.org/abs/2401.15884) | ⬜ |
| 19 | GraphRAG — 实体关系图 + 社区摘要 | [2404.16130](https://arxiv.org/abs/2404.16130) | ⬜ |
| 20 | RAG 评测综述 | [2504.14891](https://arxiv.org/abs/2504.14891) | ⬜ |

---

## 别人的索引仓库（常扫，别当权威）

| 仓库 | 星 | 最后更新 | 用途 |
|---|---|---|---|
| [Shichun-Liu/Agent-Memory-Paper-List](https://github.com/Shichun-Liu/Agent-Memory-Paper-List) | 2380 | 2026-03 | 01 号综述的官方配套论文清单 |
| [NirDiamant/Agent_Memory_Techniques](https://github.com/NirDiamant/Agent_Memory_Techniques) | 1054 | 2026-09 | ⭐ **30 个可跑 notebook**，MemGPT/Mem0/Letta/Zep/LoCoMo 全有 → 复现 baseline 用 |
| [Asaf-Yehudai/LLM-Agent-Evaluation-Survey](https://github.com/Asaf-Yehudai/LLM-Agent-Evaluation-Survey) | 110 | 2026-09 | 02 号综述配套，C3 评测框架参考 |
| [quchangle1/LLM-Tool-Survey](https://github.com/quchangle1/LLM-Tool-Survey) | 491 | 2026-09 | 工具调用线 |
| [philschmid/ai-agent-benchmark-compendium](https://github.com/philschmid/ai-agent-benchmark-compendium) | 194 | 2025-10 | 50+ benchmark 速查（已停更 10 个月） |

> ⚠️ list 里混着未中会议的 preprint，**引用前回查原论文**。

## 复现 baseline（2 个）

- [ ] **A（通用记忆方案）**：Mem0 或 MemGPT —— 从上面 1054 星的 notebook 仓库起步
- [ ] **B（领域已有方法）**：领域 X 定了才能选

## 进度

- [ ] 领域 X 确定 ← **卡在**：问导师横向是哪个领域、什么系统
- [ ] 精读 20 篇（当前 0/20）
- [ ] 复现 2 个 baseline（当前 0/2）
- [ ] 搭评测框架
- [ ] 一页纸选题提案给导师
