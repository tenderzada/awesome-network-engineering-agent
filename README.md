<p align="center">
  <img src="figures/logo_v2.png" width="200">
</p>

# Awesome Network Engineering Agent

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

Papers, benchmarks, and tools for building AI agents that autonomously configure, optimize, and operate communication networks.

> Network Engineering Agents are LLM-powered systems that read network state, reason about intent, generate actions (configurations, resource allocations, or diagnostic commands), execute them in network environments, and verify outcomes through closed-loop feedback, covering the full spectrum of network configuration, optimization, and operations.

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
- [5. Future Directions](#5-future-directions)
  - [5.1 Standardized Benchmarking and Evaluation](#51-standardized-benchmarking-and-evaluation)
  - [5.2 Long-Horizon Network Tasks](#52-long-horizon-network-tasks)
  - [5.3 Computational Efficiency and Real-Time Performance](#53-computational-efficiency-and-real-time-performance)
  - [5.4 Omni-Modal Network Engineering Agents](#54-omni-modal-network-engineering-agents)
  - [5.5 World Models for Network Agents](#55-world-models-for-network-agents)
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
| Beamforming / PHY optimization | ComAgent, WirelessBench |
| Capacity planning | MeshAgent, NetArena (MALT) |
| Spectrum management | BLAST |
| Energy efficiency | Intent-LLM |
| RF signal intelligence | RF-GPT, RadioLLM, RFF-LLM, Seeing Radio, SCA-LLM |

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
| [BeyondSWE](https://arxiv.org/abs/2603.03194) | arXiv 2026 | Cross-repo reasoning, domain-specific tasks, Docker-based evaluation |

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
| [RF-GPT](https://arxiv.org/abs/2602.14833) | arXiv 2026 | End-to-end RF language model: spectrogram to RF tokens with instruction tuning | - |
| [RadioLLM](https://arxiv.org/abs/2501.17888) | arXiv 2025 | Hybrid prompt + token reprogramming for IQ signal denoising/classification | - |
| [RFF-LLM](https://arxiv.org/abs/2508.12597) | arXiv 2025 | GPT-2-based RF fingerprinting for UAV identification in ISAC | - |
| [Seeing Radio](https://arxiv.org/abs/2601.13157) | arXiv 2026 | VLM + RF-to-image pipeline for 57-class modulation recognition | - |
| [Let RFF do the talking](https://link.springer.com/article/10.1007/s11432-024-4463-0) | Sci China Info Sci 2025 | LLM-distilled lightweight RFFI for 6G edge IoT | - |
| [LLM-Driven Spectrum Access](https://arxiv.org/abs/2604.13132) | arXiv 2026 | Hierarchical state serialization for spectral constraint reasoning | - |

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
| [SCA-LLM](https://arxiv.org/abs/2509.08139) | arXiv 2025 | Spectral-attentive LLM world model for sense-predict-plan agentic communications | - |
| [Autonomous O-RAN Agentic AI](https://arxiv.org/abs/2602.14117) | arXiv 2026 | Multi-scale LLM + SLM + WPFM agents across Non-RT / Near-RT RIC / DU | - |

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
| [TSLAM-Mini](https://arxiv.org/abs/2505.07877) | arXiv 2025 | 100K-sample telecom instruction dataset via DigiTwin simulation + RFC ingestion pipeline across 20 use-cases for QLoRA fine-tuning | - |
| [Think Less Label Better](https://arxiv.org/abs/2509.25736) | arXiv 2025 | KG-grounded QA pair synthesis via HippoRAG retrieval, base generation and refinement | - |

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
| [REDSearcher](https://arxiv.org/abs/2602.14234) | arXiv 2026 | KG-grounded multi-hop task synthesis: Wikidata subgraph sampling with complexity/dispersion controls + local simulated search environment | - |
| [LMM-Searcher](https://arxiv.org/abs/2604.12890) | arXiv 2026 | KG-skeleton cross-modal multi-hop synthesis: images bound to entities; 12K filtered long-horizon trajectories | - |
| [ExpSeek](https://arxiv.org/abs/2601.08605) | arXiv 2026 | Experience-seeking data synthesis for agent training | - |
| [TOUCAN](https://arxiv.org/abs/2510.01179) | arXiv 2025 | 1.5M multi-tool multi-turn trajectories from ~500 real MCP servers spanning 2,000+ tools with execution verification | [GitHub](https://github.com/TheAgentArk/Toucan) |
| [APIGen-MT](https://arxiv.org/abs/2504.03601) | arXiv 2025 | Committee-reviewed task blueprints + POMDP-style agent-human interplay for verified multi-turn trajectories | - |
| [AgentScaler](https://arxiv.org/abs/2509.13311) | NeurIPS 2025 | Database-backed tool graphs with API community detection and two-phase foundational + domain-specialized fine-tuning | [GitHub](https://github.com/Alibaba-NLP/DeepResearch) |
| [Self-Challenging Agents](https://arxiv.org/abs/2506.01716) | NeurIPS 2025 | Self-Challenging framework featuring a Challenger generating Code-as-Task formulations with verifiers, and an Executor applying RL on these self-synthesized tasks | - |

### 3.2 Scaling with Scaffold

Scaffold scaling enriches frozen-model agent frameworks with memory, skills, and tools, amplifying Scaffold-Driven capabilities without retraining.

#### 3.2.1 Memory

**🌐 In Network Engineering**

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [TelecomRAG](https://dl.acm.org/doi/10.1145/3656296) | SIGCOMM CCR 2025 | RAG optimized for 3GPP Release 16/18 documents | - |
| [Telco-RAG](https://github.com/netop-team/Telco-RAG) | Globecom 2024 | Dual-stage RAG with custom telecom glossary | [GitHub](https://github.com/netop-team/Telco-RAG) |
| [ReLLM](https://arxiv.org/abs/2511.22933) | arXiv 2025 | RAG-empowered LLM for dynamic radio resource management in O-RAN | - |
| [TelcoAI](https://arxiv.org/abs/2601.16984) | IJCNLP-AACL 2025 | Agentic multi-modal RAG over 3GPP specs with section-aware chunking, structured query planning, and text+diagram fusion | - |
| [6G RAN Compliance Agent](https://arxiv.org/abs/2512.12400) | arXiv 2025 | LLM agents + RAG over O-RAN Alliance and 3GPP standards for explainable compliance audit and remediation | - |

**🔷 Transferable SOTA Methods**

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [A-MEM](https://github.com/agiresearch/A-mem) | NeurIPS 2025 | Zettelkasten-style self-organizing memory with dynamic indexing | [GitHub](https://github.com/agiresearch/A-mem) |
| [MemRL](https://github.com/MemTensor/MemRL) | arXiv 2026 | Episodic memory + Q-value retrieval, runtime self-improvement without retraining | [GitHub](https://github.com/MemTensor/MemRL) |
| [AgeMem](https://arxiv.org/abs/2601.01885) | arXiv 2026 | Unified memory operations as tools, trained via 3-stage progressive RL | - |
| [EvolveR](https://arxiv.org/abs/2510.16079) | arXiv 2025 | Self-distillation of trajectories into reusable strategic principles | - |
| [Omni-SimpleMem](https://arxiv.org/abs/2604.01007) | arXiv 2026 | Simplified unified memory management for LLM agents | - |
| [A-RAG](https://arxiv.org/abs/2602.03442) | arXiv 2026 | Agentic RAG with adaptive memory | - |
| [GAM](https://arxiv.org/abs/2604.12285) | arXiv 2026 | Two-tier graph memory: global Topic Associative Network over local Event Progression Graphs with top-down retrieval | - |
| [ACON](https://arxiv.org/abs/2510.00615) | arXiv 2025 | Natural-language compression guidelines learned by contrasting success/failure trajectories, 26–54% peak-token reduction | [GitHub](https://github.com/microsoft/acon) |

#### 3.2.2 Skills

**🌐 In Network Engineering**

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [KubeIntellect](https://arxiv.org/abs/2509.02449) | arXiv 2025 | Supervisor-orchestrated K8s agent featuring a Code Generator Agent that dynamically synthesizes and registers reusable tools | [GitHub](https://github.com/MSKazemi/KubeIntellect) |
| [SkillForge](https://arxiv.org/abs/2604.08618) | arXiv 2026 | Domain-Contextualized Skill Creator + three-stage self-evolution pipeline refining agent skills from execution failures across 1,883 cloud support tickets | - |

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
| [MACLA](https://arxiv.org/abs/2512.18950) | AAMAS 2026 | Hierarchical procedural memory with Bayesian reliability tracking; 2,851 trajectories distilled into 187 procedures | [GitHub](https://github.com/S-Forouzandeh/MACLA-LLM-Agents-AAMAS-2026-Conference) |
| [CoEvoSkills](https://arxiv.org/abs/2604.01687) | arXiv 2026 | Skill Generator co-evolves with a Surrogate Verifier to provide actionable feedback without access to ground-truth test content | [GitHub](https://github.com/Zhang-Henry/CoEvoSkills) |
| [SoK Agentic Skills](https://arxiv.org/abs/2602.20867) | arXiv 2026 | Skill lifecycle model and dual taxonomies + supply-chain threat model with ~1,200 malicious-skill ClawHavoc case study | - |
| [Skill-Usage](https://arxiv.org/abs/2604.04323) | arXiv 2026 | In-the-wild skill retrieval and query-specific refinement evaluation over 34K community skills | [GitHub](https://github.com/UCSB-NLP-Chang/Skill-Usage) |

#### 3.2.3 Tools

**🌐 In Network Engineering**

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [Confucius](https://dl.acm.org/doi/10.1145/3718958.3750537) | SIGCOMM 2025 | 60+ network management tools integrated via multi-agent LLM | - |
| [WirelessAgent](https://github.com/jwentong/WirelessAgent_R1) | arXiv 2024 | Four-module cognitive architecture with external knowledge base | [GitHub](https://github.com/jwentong/WirelessAgent_R1) |
| [BLAST](https://arxiv.org/abs/2604.12127) | arXiv 2026 | LLM agents + blockchain for autonomous spectrum trading | - |
| [Intent-LLM](https://ieeexplore.ieee.org/document/11169296/) | IEEE TCCN 2025 | API-defined action space constraining agent to valid operations | - |
| [NetMCP](https://arxiv.org/abs/2510.13467) | arXiv 2025 | SONAR: an MCP tool routing algorithm that jointly optimizes semantic similarity and real-time network QoS metrics for adaptive tool selection | [GitHub](https://github.com/NICE-HKU/NetMCP) |

**🔷 Transferable SOTA Methods**

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [DeepAgent](https://github.com/RUC-NLPIR/DeepAgent) | WWW 2026 Oral | Autonomous tool discovery over 16K+ APIs with Memory Folding | [GitHub](https://github.com/RUC-NLPIR/DeepAgent) |
| [EnvScaler](https://github.com/RUC-NLPIR/EnvScaler) | arXiv 2026 | Programmatic synthesis of 191 tool-interaction environments | [GitHub](https://github.com/RUC-NLPIR/EnvScaler) |
| [VerlTool](https://arxiv.org/abs/2509.01055) | arXiv 2025 | Modular agentic RL framework for multi-modal tool use (code, search, SQL) | - |
| [ToolGen](https://arxiv.org/abs/2410.03439) | ICLR 2025 | Per-tool virtual tokens unify retrieval and calling as single generation step, scaling to 47,000+ tools without retriever | [GitHub](https://github.com/Reason-Wang/ToolGen) |
| [ToolRL](https://arxiv.org/abs/2504.13958) | NeurIPS 2025 | Principled reward design for tool-use RL with GRPO, +15% over SFT and +17% over base | [GitHub](https://github.com/qiancheng0/ToolRL) |
| [Chain-of-Tools](https://arxiv.org/abs/2503.16779) | arXiv 2025 | Frozen-LLM semantic tool vectors for ICL-prompted CoT reasoning over massive pools of unseen tools | [GitHub](https://github.com/fairyshine/Chain-of-Tools) |

### 3.3 Scaling with Agentic RL

Agentic RL scales Environment-Driven approaches by training agent policies through multi-turn interaction with environments, using verifiable rewards from execution outcomes.

**🌐 In Network Engineering**

| Paper | Venue | Method | Code |
|-------|-------|--------|------|
| [ComAgent](https://github.com/jiangfeibo/ComAgent) | arXiv 2026 | Multi-LLM closed-loop for wireless beamforming optimization | [GitHub](https://github.com/jiangfeibo/ComAgent) |
| [ORAN-GUIDE](https://arxiv.org/abs/2506.00576) | arXiv 2025 | Dual-LLM + RAG-enhanced multi-agent RL for O-RAN slicing | - |
| [6GAgentGym](https://arxiv.org/abs/2603.29656) | arXiv 2026 | SFT + RL closed-loop in network env, 8B matches GPT-5 | - |
| [6G IoT LLM PHY](https://arxiv.org/abs/2602.06819) | arXiv 2026 | Dual-LLM loop: optimization-LLM refines prompts, agent-LLM solves PHY tasks | - |
| [QoE-Slice Agent](https://arxiv.org/abs/2512.20997) | arXiv 2025 | RAG-driven intent inference + QAPPO slice orchestrator + incremental memory for QoE-reward-shaped Industrial IoT slicing | - |
| [PA-MRL](https://arxiv.org/abs/2506.00574) | arXiv 2025 | Prompt-Augmented Multi-agent RL with learnable prompts over ORANSight for dynamic O-RAN slicing | - |
| [LLM-DTNet](https://arxiv.org/abs/2506.18293) | arXiv 2025 | Hierarchical multi-layer digital twin framework with LLM orchestration and reinforcement learning for 6G radio resource allocation | - |

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
| [DAPO](https://arxiv.org/abs/2503.14476) | arXiv 2025 | Decoupled-Clip + Dynamic-Sampling PO with Clip-Higher, token-level PG loss, and overlong reward shaping at 32B scale | [GitHub](https://github.com/BytedTsinghua-SIA/DAPO) |
| [REINFORCE++](https://arxiv.org/abs/2501.03262) | arXiv 2025 | Critic-free RLHF with global-batch advantage normalization debiasing the prompt-local GRPO estimator | - |
| [ToRL](https://arxiv.org/abs/2503.23383) | arXiv 2025 | Tool-integrated RL from base LLMs with rule-based rewards; emergent tool invocation without prior SFT | [GitHub](https://github.com/GAIR-NLP/ToRL) |
| [OPRL](https://arxiv.org/abs/2509.19199) | arXiv 2025 | Alternating implicit PRM and policy via trajectory-level DPO for dense step-wise rewards + episode advantages | - |

---

## 4. How to Evaluate?

### 4.1 Static Benchmarks

Fixed test sets for reproducible evaluation.

| Benchmark | Venue | Tasks | Scale |
|-----------|-------|-------|-------|
| [NetLLMBench](https://github.com/tum-lkn/netllmbench) | IEEE 2025 | BGP/OSPF/static route config | Fixed configs |
| [SWE-Bench 5G](https://huggingface.co/datasets/tenderzada/SWEBench5G) | Preprint 2026 | AI coding agents for 5G core network bug fixing | 210 tasks from free5GC, Open5GS, and Magma |
| [NIKA](https://arxiv.org/abs/2507.01997) | SIGCOMM NetObs Workshop 2025 | Fault diagnosis | Kathará playground + chaos engineering; standardized agent–env API; trajectory logging ([repo](https://github.com/zhihao1998/LLM4NetLab)) |
| [SIMCODE](https://arxiv.org/abs/2507.11014) | arXiv 2025 | NL to ns-3 code generation | 400 tasks, 3 levels |
| [NetConfEval](https://github.com/NetConfEval/NetConfEval) | CoNEXT 2024 | 4 config tasks, runner-up best paper | [GitHub](https://github.com/NetConfEval/NetConfEval) |
| [WirelessBench](https://wirelessbench.github.io/) | arXiv 2026 | Tolerance-aware, 3392 items, 3 cognitive tiers | [GitHub](https://github.com/jwentong/WirelessBench) |
| [PeeringLLM-Bench](https://dl.acm.org/doi/10.1145/3763400.3763451) | AINTEC 2025 | NL-to-BGP config translation, multi-vendor syntax | Multi-peer, multi-vendor topologies |
| [TeleQnA](https://arxiv.org/abs/2310.15051) | arXiv 2023 | Telecom knowledge QA over 3GPP standards and research papers | 10,000 MCQs |
| [TelBench](https://aclanthology.org/2024.emnlp-industry.45/) | EMNLP-Industry 2024 | Telco-specific LLM knowledge and operations | Multi-task telco suite |
| [TelAgentBench](https://aclanthology.org/2025.emnlp-industry.83/) | EMNLP-Industry 2025 | Reasoning / planning / action / IF / RAG for Korean telecom | 5-capability agent suite |
| [TeleTables](https://arxiv.org/abs/2601.04202) | arXiv 2026 | Table interpretation in 3GPP technical specifications | 3GPP table corpus |

### 4.2 Dynamic Benchmarks

Runtime-generated queries to avoid data contamination.

| Benchmark | Venue | Tasks | Key Feature |
|-----------|-------|-------|-------------|
| [NetArena](https://github.com/Froot-NetSys/NetArena) | ICLR 2026 | Route, MALT, K8s | Dynamic query generation, A2A protocol, 3-metric evaluation |
| [6GAgentGym](https://arxiv.org/abs/2603.29656) | arXiv 2026 | 6G network management | 42 tools, NS-3 calibrated env, closed-loop RL |
| [TelcoAgent-Bench](https://arxiv.org/abs/2604.06209) | arXiv 2026 | Multilingual telecom troubleshooting | Process correctness, tool alignment, blueprint stability |
| [WirelessAgent++](https://arxiv.org/abs/2603.00501) | arXiv 2026 | Wireless agent workflow design and evaluation | Automated agentic workflow generation |
| [Continual NetOps Bench](https://dl.acm.org/doi/10.1145/3744969.3748425) | SIGCOMM 2025 | Networking operations continual evaluation | Continual benchmark generation |

---

## 5. Future Directions

### 5.1 Standardized Benchmarking and Evaluation

The network engineering agent community lacks unified evaluation protocols. Existing benchmarks cover narrow slices of the task landscape: NetArena evaluates routing, capacity planning, and K8s tasks with dynamic query generation; NetLLMBench and LLM4NetLab target static routing configuration and fault diagnosis. However, no benchmark spans the full Configuration-Optimization-Operations spectrum, and wireless optimization tasks remain entirely uncovered. Standardized evaluation must also go beyond correctness to include safety (will the agent break the network?), efficiency (token and latency cost), and generalization (cross-topology, cross-vendor transfer). The SWE agent community offers a blueprint: SWE-Bench evolved through Lite, Verified, Mobile, and BeyondSWE variants, each addressing a specific evaluation gap.

| Paper | Venue | Relevance |
|-------|-------|-----------|
| [NetArena](https://github.com/Froot-NetSys/NetArena) | ICLR 2026 | Dynamic benchmark generation with correctness + safety + latency metrics |
| [BeyondSWE](https://arxiv.org/abs/2603.03194) | arXiv 2026 | Cross-repo and domain-specific evaluation as a model for network benchmarks |

### 5.2 Long-Horizon Network Tasks

Current network agent benchmarks focus on single-step or short-horizon tasks: fix one routing error, add one node, configure one policy. Real-world network operations involve long-horizon, multi-step reasoning: a capacity upgrade requires topology assessment, device procurement planning, staged migration, traffic rerouting, validation, and rollback preparation. Agents must maintain coherent plans across dozens of interaction steps, recover from intermediate failures, and coordinate across multiple NFs. A central obstacle is context bloat: logs, configs, topology diagrams, and dashboard screenshots accumulate quickly and saturate the context window, forcing agents to drop critical state. Long-horizon task design for network agents remains an open challenge.

| Paper | Venue | Relevance |
|-------|-------|-----------|
| [LMM-Searcher](https://arxiv.org/abs/2604.12890) | arXiv 2026 | File-mapping offloads visual assets to external storage with UID references; sustains 100+ interaction rounds — transferable blueprint for network agents juggling heterogeneous long-horizon evidence |
| [MC-Search](https://arxiv.org/abs/2603.00873) | ICLR 2026 | 3,333-sample benchmark with step-annotated multi-hop chains (avg 3.7 hops, 5 topologies); introduces process-level metrics (step-hit, rolling deviation) to diagnose where long reasoning fails — blueprint for evaluating long-horizon network agents beyond end-answer accuracy |

### 5.3 Computational Efficiency and Real-Time Performance

Network operations often require real-time or near-real-time responses. A fault diagnosis agent that takes 30 seconds to reason is impractical when SLA violations accumulate at millisecond granularity. Current LLM agents rely on frontier models (GPT-4, Claude) with high latency and cost. Deploying efficient, small-footprint models at the network edge is essential for practical adoption. Recent advances in edge-optimized models such as Gemma 4 demonstrate that competitive reasoning can be achieved within tight compute budgets, opening the door for on-device network agents deployed alongside network functions. A critical reframing comes from AgentCPM-Explore: for 4B-scale agents, the bottleneck is **reasoning stability, not capability ceiling** — Pass@64 on GAIA reaches 97.09%, proving the model *can* solve complex tasks but is held back by variance from catastrophic forgetting during SFT, reward-noise sensitivity during RL, and context pollution during inference. For edge network agents, this shifts the research agenda from "shrink the model" to "stabilize the trajectory" (parameter fusion, reward denoising, context refining).

| Paper | Venue | Relevance |
|-------|-------|-----------|
| [AgentCPM-Explore](https://arxiv.org/abs/2602.06485) | arXiv 2026 | 4B edge agent matches 32B baselines on GAIA (63.9%) by targeting reasoning stability instead of capacity — DELLA weight fusion, three-layer reward-signal denoising, and dual-loop context refining; Pass@64=97.09% demonstrates the capability is latent, variance is the true bottleneck |

### 5.4 Omni-Modal Network Engineering Agents

Existing network agents operate primarily on text: CLI output, configuration files, log messages. Real network operations involve diverse modalities: topology diagrams, signal heatmaps, spectrum waterfalls, time-series metrics dashboards, and even physical site photographs. An omni-modal network agent would perceive and reason across all these modalities, combining visual understanding of network dashboards with textual analysis of logs and structured reasoning over topology graphs.

| Paper | Venue | Relevance |
|-------|-------|-----------|
| [OmniGAIA](https://arxiv.org/abs/2602.22897) | arXiv 2026 | Self-evolving omni-modal agent architecture |

### 5.5 World Models for Network Agents

World models enable agents to predict the consequences of actions before executing them, reducing costly trial-and-error in real network environments. A network world model would internalize how configurations propagate through topologies, how traffic patterns respond to policy changes, and how faults cascade across interconnected NFs. Such models could enable agents to simulate "what-if" scenarios before committing changes to production networks. Going further, recent work argues for wiring **reflective planning** *inside* the world model: a PlanAgent decomposes intent into ordered sub-actions, a CriticAgent scores each rollout, and inner/outer loops either locally refine or globally re-plan. For networking, this turns the world model from a passive predictor into an active safety interlock that rejects infeasible action chains before any change touches the live topology.

| Paper | Venue | Relevance |
|-------|-------|-----------|
| [World Model Framework](https://arxiv.org/abs/2602.01630) | arXiv 2026 | Normative framework integrating interaction, perception, reasoning, and spatial representation |
| [SPIRAL](https://arxiv.org/abs/2603.08403) | arXiv 2026 | Closed-loop think–act–reflect world model; PlanAgent decomposes intent, CriticAgent scores rollouts on 5 axes, inner/outer loops locally refine or globally re-plan — reflective-planning blueprint transferable to network agents as a pre-deployment safety interlock |

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
