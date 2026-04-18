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

Key references from the SWE agent community:

| Paper | Venue | Focus |
|-------|-------|-------|
| [SWE-Bench](https://arxiv.org/abs/2310.06770) | ICLR 2024 | Foundational benchmark: real GitHub issues as agent tasks |
| [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401) | arXiv 2025 | Skill injection evaluation: 80% of skills provide zero improvement |
| [SWE-Bench Mobile](https://arxiv.org/abs/2602.09540) | arXiv 2025 | iOS/Swift extension with diff-based intent tests |
| [SWE-Next](https://arxiv.org/abs/2603.20691) | arXiv 2026 | Scalable task synthesis via execution-driven PR mining |
| [SWE-EVO](https://arxiv.org/abs/2512.18470) | arXiv 2025 | Evolutionary self-improvement for SWE agents |
| [SWE-MiniSandbox](https://arxiv.org/abs/2602.11210) | arXiv 2026 | Lightweight sandboxed evaluation environments |
| [ScaleSWE](https://arxiv.org/abs/2602.09892) | arXiv 2026 | Scaling SWE agent training with synthetic data |
| [SWE-Universe](https://arxiv.org/abs/2602.02361) | arXiv 2026 | Multi-language, multi-repo SWE benchmark |
| [SWE-World](https://arxiv.org/abs/2602.03419) | arXiv 2026 | End-to-end SWE agent evaluation framework |
| [Agentic Rubrics](https://arxiv.org/abs/2601.04171) | arXiv 2026 | Fine-grained evaluation rubrics for SWE agents |

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

Ch3 aligns with Ch2: scaling **data** for Data-Driven, scaling **scaffold** for Scaffold-Driven, and scaling **interaction** for Environment-Driven. Papers are grouped into two tiers: 🌐 applied in networking, and 🔷 SOTA methods from other domains transferable to networking.

### 3.1 Scaling with Data Synthesis

Data synthesis generates training trajectories at scale without human annotation, addressing the data scarcity bottleneck for Data-Driven approaches.

**🌐 In Network Engineering**

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [6GAgentGym](https://arxiv.org/abs/2603.29656) | arXiv 2026 | 6G-Forge: Self-Instruct with execution verification for network tasks | - |
| [NetArena](https://github.com/Froot-NetSys/NetArena) | ICLR 2026 | Dynamic query generation for network benchmarks | [GitHub](https://github.com/Froot-NetSys/NetArena) |

**🔷 Transferable SOTA Methods**

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [AgentTrek](https://github.com/xlang-ai/AgentTrek) | ICLR 2025 Spotlight | Tutorial-guided trajectory replay at $0.55/trajectory | [GitHub](https://github.com/xlang-ai/AgentTrek) |
| [Explorer](https://github.com/OSU-NLP-Group/Explorer) | ACL 2025 | Multi-agent exploration producing 94K+ trajectories at $0.28/each | [GitHub](https://github.com/OSU-NLP-Group/Explorer) |
| [AWM](https://github.com/Snowflake-Labs/agent-world-model) | arXiv 2026 | Synthetic env generation: 1000 envs, 35K tools, SQL-backed state | [GitHub](https://github.com/Snowflake-Labs/agent-world-model) |
| [MATRIX](https://github.com/facebookresearch/matrix) | ACL 2025 | Multi-agent social simulation for post-training data synthesis | [GitHub](https://github.com/facebookresearch/matrix) |
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

**🌐 In Network Engineering**

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [TelecomRAG](https://dl.acm.org/doi/10.1145/3656296) | SIGCOMM CCR 2025 | RAG optimized for 3GPP Release 16/18 documents | - |
| [Telco-RAG](https://github.com/netop-team/Telco-RAG) | Globecom 2024 | Dual-stage RAG with custom telecom glossary | [GitHub](https://github.com/netop-team/Telco-RAG) |
| [ReLLM](https://arxiv.org/abs/2511.22933) | arXiv 2025 | RAG-empowered LLM for dynamic radio resource management in O-RAN | - |

**🔷 Transferable SOTA Methods**

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [A-MEM](https://github.com/agiresearch/A-mem) | NeurIPS 2025 | Zettelkasten-style self-organizing memory with dynamic indexing | [GitHub](https://github.com/agiresearch/A-mem) |
| [MemRL](https://github.com/MemTensor/MemRL) | arXiv 2026 | Episodic memory + Q-value retrieval, runtime self-improvement without retraining | [GitHub](https://github.com/MemTensor/MemRL) |
| [AgeMem](https://arxiv.org/abs/2601.01885) | arXiv 2026 | Unified memory operations as tools, trained via 3-stage progressive RL | - |
| [EvolveR](https://arxiv.org/abs/2510.16079) | arXiv 2025 | Self-distillation of trajectories into reusable strategic principles | - |
| [Omni-SimpleMem](https://arxiv.org/abs/2604.01007) | arXiv 2026 | Simplified unified memory management for LLM agents | - |
| [A-RAG](https://arxiv.org/abs/2602.03442) | arXiv 2026 | Agentic RAG with adaptive memory | - |

#### 3.2.2 Skills

**🔷 Transferable SOTA Methods**

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [Voyager](https://github.com/MineDojo/Voyager) | NeurIPS 2023 | First LLM agent with ever-growing executable skill library | [GitHub](https://github.com/MineDojo/Voyager) |
| [SkillRL](https://github.com/aiming-lab/SkillRL) | arXiv 2026 | Hierarchical skill bank with recursive skill evolution via RL | [GitHub](https://github.com/aiming-lab/SkillRL) |
| [SAGE](https://arxiv.org/abs/2512.17102) | arXiv 2025 | Skill-Augmented GRPO, 8.9% higher goal completion, 59% fewer tokens | - |
| [PAE](https://yanqval.github.io/PAE/) | arXiv 2024 | Autonomous skill discovery with VLM-based success evaluation | [GitHub](https://yanqval.github.io/PAE/) |
| [SkillWeaver](https://arxiv.org/abs/2504.07079) | arXiv 2025 | Web agents self-discover skills and distill into transferable APIs | - |
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

**🌐 In Network Engineering**

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [Confucius](https://dl.acm.org/doi/10.1145/3718958.3750537) | SIGCOMM 2025 | 60+ network management tools integrated via multi-agent LLM | - |
| [WirelessAgent](https://github.com/jwentong/WirelessAgent_R1) | arXiv 2024 | Four-module cognitive architecture with external knowledge base | [GitHub](https://github.com/jwentong/WirelessAgent_R1) |
| [BLAST](https://arxiv.org/abs/2604.12127) | arXiv 2026 | LLM agents + blockchain for autonomous spectrum trading | - |
| [LAM4PHY_6G](https://github.com/AI4Wireless/LAM4PHY_6G) | Various IEEE 2024-25 | GPT-2 adapted for CSI feedback, beam prediction, multi-task PHY | [GitHub](https://github.com/AI4Wireless/LAM4PHY_6G) |
| [Intent-LLM](https://ieeexplore.ieee.org/document/11169296/) | IEEE TCCN 2025 | API-defined action space constraining agent to valid operations | - |

**🔷 Transferable SOTA Methods**

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [DeepAgent](https://github.com/RUC-NLPIR/DeepAgent) | WWW 2026 Oral | Autonomous tool discovery over 16K+ APIs with Memory Folding | [GitHub](https://github.com/RUC-NLPIR/DeepAgent) |
| [EnvScaler](https://github.com/RUC-NLPIR/EnvScaler) | arXiv 2026 | Programmatic synthesis of 191 tool-interaction environments | [GitHub](https://github.com/RUC-NLPIR/EnvScaler) |

### 3.3 Scaling with Agentic RL

Agentic RL scales Environment-Driven approaches by training agent policies through multi-turn interaction with environments, using verifiable rewards from execution outcomes.

**🌐 In Network Engineering**

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [ComAgent](https://github.com/jiangfeibo/ComAgent) | arXiv 2026 | Multi-LLM closed-loop for wireless beamforming optimization | [GitHub](https://github.com/jiangfeibo/ComAgent) |
| [ORAN-GUIDE](https://arxiv.org/abs/2506.00576) | arXiv 2025 | Dual-LLM + RAG-enhanced multi-agent RL for O-RAN slicing | - |
| [6GAgentGym](https://arxiv.org/abs/2603.29656) | arXiv 2026 | SFT + RL closed-loop in network env, 8B matches GPT-5 | - |

**🔷 Transferable SOTA Methods**

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [ARPO](https://github.com/RUC-NLPIR/ARPO) | ICLR 2026 | Entropy-balanced RL for multi-turn tool-calling agents | [GitHub](https://github.com/RUC-NLPIR/ARPO) |
| [RAGEN](https://github.com/mll-lab-nu/RAGEN) | arXiv 2025 | StarPO framework + reasoning collapse diagnostics | [GitHub](https://github.com/mll-lab-nu/RAGEN) |
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
