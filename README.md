# Awesome Network Engineering Agent

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

A curated list of papers, benchmarks, and tools for AI agents in network engineering.

> Network Engineering Agents are AI systems that autonomously understand network intent, interact with network environments, generate configurations or optimization decisions, and verify outcomes through closed-loop feedback.

---

## Table of Contents

- [1. What is a Network Engineering Agent?](#1-what-is-a-network-engineering-agent)
  - [1.1 Definition](#11-definition)
  - [1.2 Application Domains](#12-application-domains)
  - [1.3 From Traditional Automation to Agentic AI](#13-from-traditional-automation-to-agentic-ai)
  - [1.4 Comparison with Software Engineering Agent](#14-comparison-with-software-engineering-agent)
- [2. How to Build?](#2-how-to-build)
  - [2.1 Data-Driven](#21-data-driven)
  - [2.2 Model-Driven](#22-model-driven)
  - [2.3 Environment-Driven](#23-environment-driven)
- [3. How to Scale?](#3-how-to-scale)
  - [3.1 Scaling with RL](#31-scaling-with-rl)
  - [3.2 Scaling with Tools](#32-scaling-with-tools)
  - [3.3 Scaling with Skills](#33-scaling-with-skills)
  - [3.4 Scaling with Memory](#34-scaling-with-memory)
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

### 1.2 Application Domains

| Domain | Tasks | Example |
|--------|-------|---------|
| Routing & Configuration | BGP/OSPF config, static routes, ACL/firewall rules | Configure OSPF so that all subnets can communicate |
| Resource Allocation | Power control, spectrum management, network slicing | Allocate transmit power to maximize total throughput |
| UAV & Trajectory Planning | Coverage optimization, relay path planning | Plan UAV trajectory to cover 20 ground users within energy budget |
| RAN Optimization | Beamforming, handover, scheduling | Design beamforming vectors to maximize SINR |
| Fault Diagnosis | Root cause analysis, troubleshooting | H1 cannot reach H3, diagnose and fix the routing error |
| Cloud Native Networking | K8s NetworkPolicy, VNF placement, service function chaining | Isolate namespace-A from namespace-B in Kubernetes |

### 1.3 From Traditional Automation to Agentic AI

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

### 2.1 Data-Driven

Training or fine-tuning agents on network-domain data.

| Approach | Description | Papers |
|----------|-------------|--------|
| SFT on network data | Fine-tune LLM on network config/troubleshooting data | Mobile-LLaMA |
| Data synthesis | Generate training data from simulators or templates | NetArena dynamic query generation |
| Distillation | Distill large model network knowledge to smaller models | LLM for Telecom survey (IEEE COMST 2025) |

### 2.2 Model-Driven

Leveraging LLM capabilities through prompting and agent framework design.

| Approach | Description | Papers |
|----------|-------------|--------|
| Prompt engineering | Zero-shot, CoT, few-shot for network tasks | Intent-LLM (IEEE TCCN 2025) |
| ReAct / tool-use | Agent reasons and calls network APIs iteratively | MeshAgent (SIGMETRICS 2026) |
| Multi-agent | Multiple specialized agents collaborate on network tasks | Confucius (SIGCOMM 2025), Multi-Agent ns-3 |
| Code generation | LLM generates executable network config/simulation code | SIMCODE, NetConfEval |

### 2.3 Environment-Driven

Building agents that interact with realistic network environments.

| Approach | Description | Papers |
|----------|-------------|--------|
| Simulator-in-the-loop | Agent operates inside Mininet/Containerlab/ns-3 | NetArena, NetLLMBench |
| Digital twin | Agent interacts with a digital replica of production network | (emerging) |
| Closed-loop verification | Agent action is executed and verified before deployment | NetArena (correctness + safety metrics) |

---

## 3. How to Scale?

### 3.1 Scaling with RL

Reinforcement learning in network environments to improve agent policies.

| Approach | Description | Papers |
|----------|-------------|--------|
| Agentic RL | Train agent policy through interaction with network simulator | AReaL (free5GC Tau2-Bench), NetworkGym |
| RLHF for networks | Human feedback on network configuration quality | (unexplored) |
| Reward design | Define rewards for network KPIs (throughput, latency, SLA) | PC-LLM, DRL for spectrum access |

### 3.2 Scaling with Tools

Expanding agent capabilities through external tools and APIs.

| Approach | Description | Papers |
|----------|-------------|--------|
| MCP integration | Standardized tool interface for network operations | (proposed) |
| API-defined action space | Constrain agent to valid network operations via API | Intent-LLM (VipeeGPT API design) |
| Verification tools | Batfish, HeaderSpace analysis for safety checking | NetLLMBench |

### 3.3 Scaling with Skills

Accumulating reusable network operation skills across tasks.

| Approach | Description | Papers |
|----------|-------------|--------|
| Skill library | Agent stores successful operation sequences for reuse | Analogy: Voyager skill library |
| Skill discovery | Automatically extract reusable skills from past trajectories | (unexplored in networking) |
| Skill composition | Combine atomic skills into complex multi-step operations | (unexplored in networking) |
| Skill injection | Provide domain skill documents to boost agent performance | SWE-Skills-Bench, SWE-Bench 5G spec-as-skill |

### 3.4 Scaling with Memory

Enabling agents to accumulate and reuse experience across tasks.

| Approach | Description | Papers |
|----------|-------------|--------|
| Cross-task experience | Remember past diagnoses to speed up future troubleshooting | Analogy: MemoryBank, A-MEM |
| Network state memory | Persistent knowledge of topology, history, SLA | Analogy: LD-Agent dual memory |
| Retrieval-augmented | Retrieve relevant past cases or spec documents at inference | RAG for network operations |

---

## 4. How to Evaluate?

### 4.1 Static Benchmarks

Fixed test sets for reproducible evaluation.

| Benchmark | Venue | Tasks | Scale |
|-----------|-------|-------|-------|
| [NetLLMBench](https://github.com/tum-lkn/netllmbench) | IEEE 2025 | BGP/OSPF/static route config | Fixed configs |
| [LLM4NetLab](https://github.com/zhihao1998/LLM4NetLab) | SIGCOMM 2025 | Fault diagnosis | Curated incidents |
| [SIMCODE](https://arxiv.org/abs/2507.11014) | arXiv 2025 | NL to ns-3 code generation | 400 tasks, 3 levels |
| [NetConfEval](https://dl.acm.org/doi/10.1145/3680121) | CoNEXT 2024 | Network config verification | Fixed test suite |

### 4.2 Dynamic Benchmarks

Runtime-generated queries to avoid data contamination.

| Benchmark | Venue | Tasks | Key Feature |
|-----------|-------|-------|-------------|
| [NetArena](https://github.com/Froot-NetSys/NetArena) | ICLR 2026 | Route, MALT, K8s | Dynamic query generation, A2A protocol, 3-metric evaluation |

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
