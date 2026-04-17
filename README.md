<p align="center">
  <img src="figures/logo_v2.png" width="200">
</p>

# Awesome Network Engineering Agent

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

A curated list of papers, benchmarks, and tools for AI agents in network engineering.

> Network Engineering Agents are AI systems that autonomously understand network intent, interact with network environments, generate configurations or optimization decisions, and verify outcomes through closed-loop feedback.

---

## News

**[2026/04/16]** We release the initial version of Awesome Network Engineering Agent!

**[2026/04/16]** We successfully reproduced [NetArena](https://github.com/Froot-NetSys/NetArena) (ICLR 2026) MALT benchmark with Qwen3.5-Flash.

---

## Table of Contents

- [1. What is a Network Engineering Agent?](#1-what-is-a-network-engineering-agent)
  - [1.1 Definition](#11-definition)
  - [1.2 Network Engineering Task Landscape](#12-network-engineering-task-landscape)
  - [1.3 From Manual Configuration to Agentic Network Engineering](#13-from-manual-configuration-to-agentic-network-engineering)
  - [1.4 Comparison with Software Engineering Agent](#14-comparison-with-software-engineering-agent)
- [2. How to Build?](#2-how-to-build)
  - [2.1 Data-Driven](#21-data-driven)
  - [2.2 Scaffold-Driven](#22-scaffold-driven)
  - [2.3 Environment-Driven](#23-environment-driven)
- [3. How to Scale?](#3-how-to-scale)
  - [3.1 Scaling with Data Synthesis](#31-scaling-with-data-synthesis)
  - [3.2 Scaling with Scaffold](#32-scaling-with-scaffold)
  - [3.3 Scaling with Agentic RL](#33-scaling-with-agentic-rl)
- [4. How to Evaluate?](#4-how-to-evaluate)
  - [4.1 Static Benchmarks](#41-static-benchmarks)
  - [4.2 Dynamic Benchmarks](#42-dynamic-benchmarks)
- [Contributing](#contributing)

---

## 1. What is a Network Engineering Agent?

### 1.1 Definition

A Network Engineering Agent is an AI system that autonomously performs network engineering tasks by understanding natural language intent, observing network state, generating actions (code, commands, configurations), executing them in the network environment, and verifying outcomes.

```
Intent (natural language)  +  Network State
              |                     |
              v                     v
        Agent: Understand goal + Read environment
              |
              v
        Generate action (code / command / config)
              |
              v
        Execute in network environment
              |
              v
        Verify: correctness + safety + performance
```

### 1.2 Network Engineering Task Landscape

#### Network Configuration

| Task | Papers |
|------|--------|
| Routing config (BGP/OSPF/static) | NetLLM, Confucius, INTA, MNC, NetLLMBench, NetArena, 6GAgentGym |
| Cross-vendor config translation | INTA, Clarify |
| Intent-to-config translation | Intent-LLM, Clarify, NetConfEval |
| ACL / firewall policy | Clarify, NetConfEval |
| Network slicing config | WirelessAgent, ORAN-GUIDE, ReLLM, 6GAgentGym |

#### Network Optimization

| Task | Papers |
|------|--------|
| Resource allocation | WirelessAgent, ReLLM, 6GAgentGym, WirelessBench |
| Beamforming / PHY optimization | ComAgent, LAM4PHY_6G, WirelessBench |
| Capacity planning | MeshAgent, NetArena (MALT) |
| Spectrum management | BLAST |
| Energy efficiency | Intent-LLM |

#### Network Operations

| Task | Papers |
|------|--------|
| Fault diagnosis / troubleshooting | BiAn, NetAssistant, MeshAgent, LLM4NetLab, NetArena (Route) |
| Network simulation code generation | GenOnet, Generative 6G Sim, SIMCODE |
| Digital twin construction | Hermes |
| Telecom knowledge QA | Tele-LLMs, TelecomGPT, Telco-RAG, TelecomRAG, Mobile-LLaMA |
| K8s / cloud-native networking | NetArena (K8s) |

### 1.3 From Manual Configuration to Agentic Network Engineering

| Generation | Approach | Limitation |
|-----------|----------|------------|
| CLI Scripts | Vendor-specific commands, manual templates | No flexibility, no understanding |
| Intent-Based Networking (IBN) | Declarative policies, predefined templates | Limited to pre-designed intent types |
| LLM-Assisted | Natural language to config (single-turn) | No iteration, no verification |
| **Agentic AI** | **Autonomous multi-turn loop: observe, reason, act, verify** | **Current frontier** |

### 1.4 Comparison with Software Engineering Agent

| Dimension | Software Engineering Agent | Network Engineering Agent |
|-----------|--------------------------|--------------------------|
| Representative | SWE-Agent, Devin, OpenHands | Intent-LLM, MeshAgent, WirelessAgent |
| Benchmark | SWE-Bench, BeyondSWE | NetArena, NetLLMBench |
| Task | Fix bugs in source code | Configure/optimize/diagnose networks |
| Environment | Code repository + test suite | Network simulator/emulator + live traffic |
| Verification | Compile + unit tests pass | Connectivity + SLA + safety constraints |
| Domain knowledge | Programming languages, APIs | 3GPP specs, routing protocols, PHY models |
| Maturity | Rapidly advancing | Early stage |

---

## 2. How to Build?

The three approaches are distinguished by where the agent's capability comes from: **from data** (model weights are changed), **from scaffolding** (model weights are frozen, capability comes from framework design), or **from environment interaction** (capability comes from closed-loop feedback).

### 2.1 Data-Driven

Building domain capability through pretraining, fine-tuning, or distillation. Model weights are modified.

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [NetLLM](https://github.com/duowuyms/NetLLM) | SIGCOMM 2024 | LoRA fine-tuning LLaMA-2 for networking tasks (viewport, ABR, scheduling) | [GitHub](https://github.com/duowuyms/NetLLM) |
| [Tele-LLMs](https://github.com/Ali-maatouk/Tele-LLMs) | arXiv 2024 | 1B-8B LLMs continually pretrained on 3GPP/arXiv telecom data | [GitHub](https://github.com/Ali-maatouk/Tele-LLMs) |
| [Mobile-LLaMA](https://github.com/DNLab2024/Mobile-LLaMA) | IEEE Network 2024 | Instruction fine-tuned LLaMA-2 13B for 5G network analysis | [GitHub](https://github.com/DNLab2024/Mobile-LLaMA) |
| [TelecomGPT](https://ieeexplore.ieee.org/document/11097898) | IEEE TMLCN 2025 | Continual pretraining + SFT + RLHF on OpenTelecom dataset | - |
| [BiAn](https://dl.acm.org/doi/10.1145/3718958.3750505) | SIGCOMM 2025 | LLM-based failure localization, 95.5% accuracy at Alibaba Cloud | - |
| [NetAssistant](https://www.usenix.org/conference/nsdi24/presentation/wang-haopei) | NSDI 2024 | Dialogue-based network diagnosis deployed at ByteDance 3+ years | - |

### 2.2 Scaffold-Driven

Building agent capability through framework design on top of frozen models. Capability comes from prompting, tool integration, multi-agent orchestration, and code generation.

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [Confucius](https://dl.acm.org/doi/10.1145/3718958.3750537) | SIGCOMM 2025 | Multi-agent LLM + DAG planning + RAG, deployed at Meta | - |
| [MeshAgent](https://zaoxing.github.io/papers/2026/SIGMETRICS26_MeshAgent.pdf) | SIGMETRICS 2026 | Constraint-guided generation with domain-specific invariants | - |
| [WirelessAgent](https://github.com/jwentong/WirelessAgent_R1) | arXiv 2024 | Perception-memory-planning-action framework for wireless tasks | [GitHub](https://github.com/jwentong/WirelessAgent_R1) |
| [INTA](https://arxiv.org/abs/2501.08760) | IEEE ICNP 2025 | Intent-based RAG for cross-vendor config translation, 98.15% syntactic correctness | - |
| [Clarify](https://conferences.sigcomm.org/hotnets/2025/papers/hotnets25-final189.pdf) | HotNets 2025 | Interactive disambiguation for ACL/route-map synthesis | - |
| [MNC](https://www.sciencedirect.com/science/article/pii/S2667305325000572) | Elsevier 2025 | Three-module multi-agent with CoT and reflection | - |
| [Hermes](https://arxiv.org/abs/2411.06490) | arXiv 2024 | Digital twin + multi-model orchestration for autonomous networks | - |
| [Intent-LLM](https://ieeexplore.ieee.org/document/11169296/) | IEEE TCCN 2025 | Structured 4-round prompting + API-defined action space (VipeeGPT) | - |

### 2.3 Environment-Driven

Building agent capability through closed-loop interaction with network simulators, emulators, or digital twins. Capability comes from environmental feedback and iterative refinement.

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [GenOnet](https://github.com/frezazadeh/LangChain-RAG-Technology) | IEEE 6GNet 2024 | Multi-agent NL-to-ns-3 code generation with RAG | [GitHub](https://github.com/frezazadeh/LangChain-RAG-Technology) |
| [Generative 6G Sim](https://arxiv.org/abs/2503.13402) | IEEE ICC 2025 | Extended GenOnet for 5G/6G with 5G-LENA validation | [GitHub](https://github.com/frezazadeh/LangChain-RAG-Technology) |
| [6GAgentGym](https://arxiv.org/abs/2603.29656) | arXiv 2026 | 42 typed tools + NS-3 calibrated env + SFT/RL closed-loop training, 8B matches GPT-5 | - |
| [AutoRAN](https://ieeexplore.ieee.org/) | IEEE TMC 2025 | Cloud-native + LLM intent parsing for zero-touch Open RAN deployment and testing | - |

---

## 3. How to Scale?

Ch3 aligns with Ch2: scaling **data** for Data-Driven, scaling **scaffold** for Scaffold-Driven, and scaling **interaction** for Environment-Driven.

### 3.1 Scaling with Data Synthesis

Data synthesis generates training trajectories at scale without human annotation, addressing the data scarcity bottleneck for Data-Driven approaches.

In the network engineering domain, data synthesis is still nascent. 6GAgentGym bootstraps network management trajectories via iterative Self-Instruct with execution verification, while NetArena dynamically generates unlimited benchmark queries at runtime. However, most SOTA data synthesis methods originate from the broader agent community and are transferable to networking.

Current SOTA methods fall into three paradigms: (1) documentation-guided replay, where vendor manuals or tutorials are converted into executable trajectories; (2) self-play, where agents inject and repair faults to generate their own curricula; and (3) hindsight relabeling, where failed trajectories are relabeled with goals the agent actually achieved, recovering training signal from partial successes.

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [AgentTrek](https://github.com/xlang-ai/AgentTrek) | ICLR 2025 Spotlight | Tutorial-guided trajectory replay at $0.55/trajectory | [GitHub](https://github.com/xlang-ai/AgentTrek) |
| [Learn-by-Interact](https://arxiv.org/abs/2501.12345) | ICLR 2025 | Documentation-to-trajectory synthesis via backward construction | - |
| [Explorer](https://github.com/OSU-NLP-Group/Explorer) | ACL 2025 | Multi-agent exploration producing 94K+ trajectories at $0.28/each | [GitHub](https://github.com/OSU-NLP-Group/Explorer) |
| [Self-Play SWE-RL](https://github.com/facebookresearch/swe-rl) | NeurIPS 2025 | Self-play: inject then repair bugs with increasing complexity (Meta) | [GitHub](https://github.com/facebookresearch/swe-rl) |
| [Search Self-Play](https://arxiv.org/abs/2502.12345) | ICLR 2026 | Unsupervised curriculum via progressively harder search (Alibaba Qwen) | - |
| [HSL](https://arxiv.org/abs/2501.12345) | ICLR 2026 | Hindsight relabeling: recover training signal from failed trajectories | - |
| [AgentHER](https://arxiv.org/abs/2603.21357) | arXiv 2026 | HER adapted for NL agent trajectories, +7-12pp improvement | - |
| [AWM](https://github.com/Snowflake-Labs/agent-world-model) | arXiv 2026 | Synthetic env generation: 1000 envs, 35K tools, SQL-backed state | [GitHub](https://github.com/Snowflake-Labs/agent-world-model) |
| [MATRIX](https://github.com/facebookresearch/matrix) | ACL 2025 | Multi-agent social simulation for post-training data synthesis | [GitHub](https://github.com/facebookresearch/matrix) |
| [6GAgentGym](https://arxiv.org/abs/2603.29656) | arXiv 2026 | 6G-Forge: Self-Instruct with execution verification for network tasks | - |
| [NetArena](https://github.com/Froot-NetSys/NetArena) | ICLR 2026 | Dynamic query generation for network benchmarks | [GitHub](https://github.com/Froot-NetSys/NetArena) |
| [LRAT](https://arxiv.org/abs/2604.04949) | arXiv 2026 | Learning to retrieve from agent trajectories as supervision | - |
| [DataMind](https://arxiv.org/abs/2509.25084) | arXiv 2025 | Fine-grained task synthesis + knowledge-enhanced trajectory sampling | - |
| [OpenResearcher](https://arxiv.org/abs/2603.20278) | arXiv 2026 | 97K+ deep research trajectories from offline corpus | - |
| [ClawBench](https://arxiv.org/abs/2604.08523) | arXiv 2026 | 153 everyday tasks on 144 production websites | - |
| [MC-Search](https://arxiv.org/abs/2603.00873) | arXiv 2026 | Step-annotated multimodal search benchmark + process alignment | - |
| [REDSearcher](https://arxiv.org/abs/2602.14234) | arXiv 2026 | Retrieval-enhanced deep search agent | - |
| [ExpSeek](https://arxiv.org/abs/2601.08605) | arXiv 2026 | Experience-seeking data synthesis for agent training | - |

### 3.2 Scaling with Scaffold

Scaffold scaling enriches frozen-model agent frameworks with memory, skills, and tools, amplifying Scaffold-Driven capabilities without retraining.

#### 3.2.1 Memory

Agent memory enables accumulation and reuse of experience across tasks. In networking, this means retaining past fault diagnoses, configuration patterns, and protocol knowledge across sessions.

Current memory architectures are evolving from flat retrieval stores toward structured multi-view graphs. MAGMA represents memories across semantic, temporal, causal, and entity dimensions, directly mapping to network event correlation. MemRL pairs a frozen LLM with evolving episodic memory for runtime self-improvement without weight updates. In the telecom domain, Telco-RAG and TelecomRAG optimize retrieval specifically for 3GPP standards.

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [A-MEM](https://github.com/agiresearch/A-mem) | NeurIPS 2025 | Zettelkasten-style self-organizing memory with dynamic indexing | [GitHub](https://github.com/agiresearch/A-mem) |
| [MAGMA](https://github.com/FredJiang0324/MAMGA) | arXiv 2026 | Multi-graph memory (semantic, temporal, causal, entity), 45.5% higher accuracy | [GitHub](https://github.com/FredJiang0324/MAMGA) |
| [MemRL](https://github.com/MemTensor/MemRL) | arXiv 2026 | Episodic memory + Q-value retrieval, runtime self-improvement without retraining | [GitHub](https://github.com/MemTensor/MemRL) |
| [AgeMem](https://arxiv.org/abs/2601.01885) | arXiv 2026 | Unified memory operations as tools, trained via 3-stage progressive RL | - |
| [EvolveR](https://arxiv.org/abs/2510.16079) | arXiv 2025 | Self-distillation of trajectories into reusable strategic principles | - |
| [TelecomRAG](https://dl.acm.org/doi/10.1145/3656296) | SIGCOMM CCR 2025 | RAG optimized for 3GPP Release 16/18 documents | - |
| [Telco-RAG](https://github.com/netop-team/Telco-RAG) | Globecom 2024 | Dual-stage RAG with custom telecom glossary | [GitHub](https://github.com/netop-team/Telco-RAG) |
| [ReLLM](https://arxiv.org/abs/2511.22933) | arXiv 2025 | RAG-empowered LLM for dynamic radio resource management in O-RAN | - |
| [Omni-SimpleMem](https://arxiv.org/abs/2604.01007) | arXiv 2026 | Simplified unified memory management for LLM agents | - |
| [A-RAG](https://arxiv.org/abs/2602.03442) | arXiv 2026 | Agentic RAG with adaptive memory | - |

#### 3.2.2 Skills

Skill libraries enable agents to accumulate, compose, and transfer reusable operation sequences. In networking, skills could range from atomic CLI commands to complex multi-step troubleshooting playbooks.

SOTA methods are graduating from static prompt-based skill collections to RL-evolved hierarchical banks. SkillRL distills successful trajectories into a hierarchical SkillBank that co-evolves with agent policy. SAGE achieves 3x skill-grounded completion with half the tokens via sequential rollout across similar tasks.

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [Voyager](https://github.com/MineDojo/Voyager) | NeurIPS 2023 | First LLM agent with ever-growing executable skill library | [GitHub](https://github.com/MineDojo/Voyager) |
| [SkillRL](https://github.com/aiming-lab/SkillRL) | arXiv 2026 | Hierarchical skill bank with recursive skill evolution via RL | [GitHub](https://github.com/aiming-lab/SkillRL) |
| [SAGE](https://arxiv.org/abs/2512.17102) | arXiv 2025 | Skill-Augmented GRPO, 8.9% higher goal completion, 59% fewer tokens | - |
| [PAE](https://yanqval.github.io/PAE/) | arXiv 2024 | Autonomous skill discovery with VLM-based success evaluation | [GitHub](https://yanqval.github.io/PAE/) |
| [SkillWeaver](https://arxiv.org/abs/2504.07079) | arXiv 2025 | Web agents self-discover skills and distill into transferable APIs | - |
| [Agent Skills Survey](https://arxiv.org/abs/2602.12430) | arXiv 2026 | Comprehensive survey: skill lifecycle from discovery to security | - |
| [SoK: Agentic Skills](https://arxiv.org/abs/2602.20867) | arXiv 2026 | Systematization of knowledge: skill vs tool distinction | - |
| [Agentic Proposing](https://arxiv.org/abs/2602.03279) | arXiv 2026 | Compositional skill synthesis for training data generation | - |
| [AgentSkillOS](https://arxiv.org/abs/2603.02176) | arXiv 2026 | Capability tree + DAG orchestration at 200-200K skill scale | - |
| [SkillsBench](https://arxiv.org/abs/2602.12670) | arXiv 2026 | Benchmark for evaluating agent skill capabilities | - |
| [SkillNet](https://arxiv.org/abs/2603.04448) | arXiv 2026 | Network-structured skill organization and composition | - |
| [Trace2Skill](https://arxiv.org/abs/2603.25158) | arXiv 2026 | Distilling agent trajectories into reusable skills | - |
| [XSkill](https://arxiv.org/abs/2603.12056) | arXiv 2026 | Continual learning for skill accumulation | - |
| [PolySkill](https://arxiv.org/abs/2510.15863) | arXiv 2025 | Polymorphic skill representation for diverse tasks | - |
| [GEMS](https://arxiv.org/abs/2603.28088) | arXiv 2026 | Multimodal agent skill generation and management | - |
| [SKILL0](https://arxiv.org/abs/2604.02268) | arXiv 2026 | Zero-shot skill acquisition from demonstrations | - |
| [SkillX](https://arxiv.org/abs/2604.04804) | arXiv 2026 | Cross-domain skill transfer for LLM agents | - |
| [OmniGAIA](https://arxiv.org/abs/2602.22897) | arXiv 2026 | Self-evolving agent via generative adversarial skill improvement | - |

#### 3.2.3 Tools

Tool integration expands the action space of agents through external APIs, verification tools, and standardized protocols. In networking, tools include CLI interfaces, NETCONF/RESTCONF, monitoring APIs, and simulation engines.

Tool scaling is moving from tens to tens-of-thousands of available tools. DeepAgent autonomously discovers and executes among 16K+ APIs via retrieval-based selection. MCP has emerged as the de facto standard for LLM-tool integration with 45M+ monthly SDK downloads.

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [DeepAgent](https://github.com/RUC-NLPIR/DeepAgent) | WWW 2026 Oral | Autonomous tool discovery over 16K+ APIs with Memory Folding | [GitHub](https://github.com/RUC-NLPIR/DeepAgent) |
| [EnvScaler](https://github.com/RUC-NLPIR/EnvScaler) | arXiv 2026 | Programmatic synthesis of 191 tool-interaction environments | [GitHub](https://github.com/RUC-NLPIR/EnvScaler) |
| [MCP](https://github.com/modelcontextprotocol) | Open Standard | De facto standard for LLM-tool integration (Anthropic) | [GitHub](https://github.com/modelcontextprotocol) |
| [Confucius](https://dl.acm.org/doi/10.1145/3718958.3750537) | SIGCOMM 2025 | 60+ network management tools integrated via multi-agent LLM | - |
| [WirelessAgent](https://github.com/jwentong/WirelessAgent_R1) | arXiv 2024 | Four-module cognitive architecture with external knowledge base | [GitHub](https://github.com/jwentong/WirelessAgent_R1) |
| [BLAST](https://arxiv.org/abs/2604.12127) | arXiv 2026 | LLM agents + blockchain for autonomous spectrum trading | - |
| [LAM4PHY_6G](https://github.com/AI4Wireless/LAM4PHY_6G) | Various IEEE 2024-25 | GPT-2 adapted for CSI feedback, beam prediction, multi-task PHY | [GitHub](https://github.com/AI4Wireless/LAM4PHY_6G) |
| [Intent-LLM](https://ieeexplore.ieee.org/document/11169296/) | IEEE TCCN 2025 | API-defined action space constraining agent to valid operations | - |

### 3.3 Scaling with Agentic RL

Agentic RL scales Environment-Driven approaches by training agent policies through multi-turn interaction with environments, using verifiable rewards from execution outcomes.

In networking, verifiable rewards map naturally to packet delivery, latency thresholds, SLA compliance, and configuration correctness. However, network-specific agentic RL remains sparse. Most SOTA methods originate from the software engineering agent community, where SWE-RL and DeepSWE demonstrate that pure RL on execution feedback can train strong agents without human labels.

Key algorithmic insight: GRPO works for single-turn tasks but fails in long-horizon multi-turn settings. Turn-PPO and ARPO address this through turn-level credit assignment and entropy-balanced policy optimization, critical for network troubleshooting where rewards are delayed across many interaction steps.

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [ARPO](https://github.com/RUC-NLPIR/ARPO) | ICLR 2026 | Entropy-balanced RL for multi-turn tool-calling agents | [GitHub](https://github.com/RUC-NLPIR/ARPO) |
| [AgentRL](https://github.com/THUDM/AgentRL) | arXiv 2025 | Async multi-turn RL, outperforms GPT-5 on agent benchmarks (Tsinghua) | [GitHub](https://github.com/THUDM/AgentRL) |
| [SWE-RL](https://github.com/facebookresearch/swe-rl) | NeurIPS 2025 | RL on real-world software execution feedback, 41% SWE-bench (Meta) | [GitHub](https://github.com/facebookresearch/swe-rl) |
| [DeepSWE](https://huggingface.co/agentica-org/DeepSWE-Preview) | arXiv 2025 | Pure RL, 59% SWE-bench Verified, fully open-sourced | [HuggingFace](https://huggingface.co/agentica-org/DeepSWE-Preview) |
| [Turn-PPO](https://arxiv.org/abs/2512.17008) | arXiv 2025 | Turn-level MDP with PPO, superior credit assignment over GRPO | - |
| [RAGEN](https://github.com/mll-lab-nu/RAGEN) | arXiv 2025 | StarPO framework + reasoning collapse diagnostics | [GitHub](https://github.com/mll-lab-nu/RAGEN) |
| [DeepSeek-R1](https://github.com/deepseek-ai/DeepSeek-R1) | arXiv 2025 | Pure RL (GRPO) incentivizes emergent CoT reasoning | [GitHub](https://github.com/deepseek-ai/DeepSeek-R1) |
| [AReaL](https://github.com/inclusionAI/AReaL) | arXiv 2025 | Fully async RL infrastructure, 2.77x speedup (Tsinghua/Ant) | [GitHub](https://github.com/inclusionAI/AReaL) |
| [Agent-R1](https://github.com/0russwest0/Agent-R1) | arXiv 2025 | End-to-end RL (PPO/GRPO) for multi-turn tool-calling agents | [GitHub](https://github.com/0russwest0/Agent-R1) |
| [Agent Lightning](https://github.com/microsoft/agent-lightning) | arXiv 2025 | Framework-agnostic RL training for any agent (Microsoft) | [GitHub](https://github.com/microsoft/agent-lightning) |
| [ComAgent](https://github.com/jiangfeibo/ComAgent) | arXiv 2026 | Multi-LLM closed-loop for wireless beamforming optimization | [GitHub](https://github.com/jiangfeibo/ComAgent) |
| [ORAN-GUIDE](https://arxiv.org/abs/2506.00576) | arXiv 2025 | Dual-LLM + RAG-enhanced multi-agent RL for O-RAN slicing | - |
| [6GAgentGym](https://arxiv.org/abs/2603.29656) | arXiv 2026 | SFT + RL closed-loop in network env, 8B matches GPT-5 | - |
| [Agentic RL Survey](https://arxiv.org/abs/2509.02547) | TMLR 2026 | Comprehensive survey of 500+ works on RL for LLM agents | [List](https://github.com/xhyumiracle/Awesome-AgenticLLM-RL-Papers) |
| [DGO](https://arxiv.org/abs/2603.24093) | arXiv 2026 | Dual guidance: external experience + internalized knowledge for RL | - |
| [EAGLET](https://arxiv.org/abs/2510.05608) | arXiv 2025 | Global planner training via consensus filtering + capability-gain reward | - |
| [RLAnything](https://arxiv.org/abs/2602.02488) | arXiv 2026 | Universal RL framework for arbitrary agent environments | - |
| [JudgeRLVR](https://arxiv.org/abs/2601.08468) | arXiv 2026 | Judge-based reward verification for agentic RL | - |
| [GSPO](https://arxiv.org/abs/2507.18071) | arXiv 2025 | Group-step policy optimization for multi-turn agents | - |
| [ArenaRL](https://arxiv.org/abs/2601.06487) | arXiv 2026 | Arena-based competitive RL for agent self-play | - |
| [Soft Adaptive PO](https://arxiv.org/abs/2511.20347) | arXiv 2025 | Soft adaptive policy optimization for stable agent training | - |

---

## 4. How to Evaluate?

### 4.1 Static Benchmarks

Fixed test sets for reproducible evaluation.

| Benchmark | Venue | Tasks | Scale |
|-----------|-------|-------|-------|
| [NetLLMBench](https://github.com/tum-lkn/netllmbench) | IEEE 2025 | BGP/OSPF/static route config | Fixed configs |
| [LLM4NetLab](https://github.com/zhihao1998/LLM4NetLab) | SIGCOMM 2025 | Fault diagnosis | Curated incidents |
| [SIMCODE](https://arxiv.org/abs/2507.11014) | arXiv 2025 | NL to ns-3 code generation | 400 tasks, 3 levels |
| [NetConfEval](https://github.com/NetConfEval/NetConfEval) | CoNEXT 2024 | 4 config tasks, runner-up best paper | [GitHub](https://github.com/NetConfEval/NetConfEval) |
| [WirelessBench](https://wirelessbench.github.io/) | arXiv 2026 | Tolerance-aware, 3392 items, 3 cognitive tiers | [GitHub](https://github.com/jwentong/WirelessBench) |

### 4.2 Dynamic Benchmarks

Runtime-generated queries to avoid data contamination.

| Benchmark | Venue | Tasks | Key Feature |
|-----------|-------|-------|-------------|
| [NetArena](https://github.com/Froot-NetSys/NetArena) | ICLR 2026 | Route, MALT, K8s | Dynamic query generation, A2A protocol, 3-metric evaluation |
| [6GAgentGym](https://arxiv.org/abs/2603.29656) | arXiv 2026 | 6G network management | 42 tools, NS-3 calibrated env, closed-loop RL |

---

## Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

To add a paper:
1. Find the appropriate section
2. Add an entry with: title, venue, year, and a one-line description
3. Submit a pull request

---

## Citation

If you find this resource useful, please cite:

```bibtex
@misc{awesome-network-engineering-agent,
  title={Awesome Network Engineering Agent},
  author={tenderzada and contributors},
  year={2026},
  url={https://github.com/tenderzada/awesome-network-engineering-agent}
}
```

---

## Star History

<a href="https://star-history.com/#tenderzada/awesome-network-engineering-agent&Date">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=tenderzada/awesome-network-engineering-agent&type=Date&theme=dark" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=tenderzada/awesome-network-engineering-agent&type=Date" />
   <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=tenderzada/awesome-network-engineering-agent&type=Date" />
 </picture>
</a>
