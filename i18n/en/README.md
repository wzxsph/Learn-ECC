# Learn-ECC

> A structured learning path for mastering ECC (Everything Claude Code)

Welcome to Learn-ECC! This is a systematic learning path for mastering Claude Code, guiding you from beginner to proficient user.

---

## 🌐 Language / 语言 / 語言 / Dil / Язык / Ngôn ngữ

English | Português (Brasil) | 简体中文 | 繁體中文 | 日本語 | 한국어 | Türkçe | Русский | Tiếng Việt | ไทย

| [🇺🇸 English](./README.md) | [🇯🇵 日本語](./ja/README.md) | [🇰🇷 한국어](./ko/README.md) | [🇩🇪 Deutsch](./de/README.md) |
|:---:|:---:|:---:|:---:|
| [🇷🇺 Русский](./ru/README.md) | [🇹🇷 Türkçe](./tr/README.md) | [🇧🇷 Português](./pt-BR/README.md) | [🇻🇳 Tiếng Việt](./vi/README.md) |
| [🇹🇭 ไทย](./th/README.md) | [🇨🇳 简体中文](../README.md) | | | |

---

## Background

### Why This Project?

I found reading ECC's English documentation challenging due to language barriers. So I translated the documentation for Commands, Agents, Skills, and other features into Chinese, organizing them into this learning course.

The reference documentation came from this translation effort -- taking content from the ECC project and explaining what each command and Agent does.

### How Was This Course Created?

Honestly, this course was AI-generated. AI created the basic content based on ECC's file structure, but it lacked detailed thinking and practical evaluation. If you find any issues or unclearness in the content, please feel free to suggest improvements.

---

## Core Concepts

### What is ECC?

**ECC** (Everything Claude Code) is a Claude Code plugin project ([original repository](https://github.com/affaan-m/ECC)) that provides:
- **75 commands** - Slash commands (like `/plan`, `/code-review`) and script commands
- **60 professional Agents** - Code review, build fixes, architecture design, etc.
- **232+ Skills** - Reusable workflow templates
- **Hooks system** - Event-driven automation mechanism
- **Rules** - Enforced development standards

### What is Learn-ECC?

**Learn-ECC** is a systematic learning path I organized for learning ECC, not ECC itself.

It follows a "learning by doing" design philosophy, with each stage containing both theory and practical exercises. Through completing real projects, you will master Claude Code's core capabilities.

### What is the Reference Documentation?

**Reference documentation** (./Reference-Docs/) is the Chinese translation of the ECC project. I translated ECC's English documentation into Chinese for easier reference.

Content includes:
- Complete command documentation (what each command does)
- Detailed introductions for all Agents
- Skills directory structure and authoring standards
- Complete Hooks system documentation
- Best practices and design patterns

---

## Learning Path Overview

| Stage | Content | Duration | Difficulty |
|-------|---------|----------|------------|
| [1-入门](./1-Getting-Started/README.md) | ECC commands, Agents, Skills intro + installation | 2-3 hours | Beginner |
| [2-核心能力](./2-Core-Capabilities/README.md) | Command system, Agent collaboration, Hooks customization, Skills development, Rules authoring | 8-12 hours | Intermediate |
| [3-专家之路](./3-Expert-Path/README.md) | MCP development, autonomous agents, advanced patterns | 6-8 hours | Advanced |
| [4-实战项目](./4-Practical-Projects/README.md) | CLI tools, code review, automation, Agent systems | 10-15 hours | Practical |
| [5-快速参考](./5-Quick-Reference/README.md) | Commands cheatsheet, Agents cheatsheet, workflows cheatsheet | Reference | Reference |

---

## Quick Navigation

### Stage 1: Getting Started
- [Course Introduction](./1-Getting-Started/01-ECC-Introduction.md) - What is ECC, what can Learn-ECC do
- [Installation & Configuration](./1-Getting-Started/02-Installation.md) - Quick setup guide
- [Basic Commands](./1-Getting-Started/03-Basic-Commands.md) - Overview of common commands
- [Your First Task](./1-Getting-Started/04-First-Task.md) - Complete your first task
- [Getting Started Exercises](./1-Getting-Started/exercises/Getting-Started-Exercises.md) - Reinforce basics

### Stage 2: Core Capabilities
- [Mastering the Command System](./2-Core-Capabilities/01-Command-System/README.md) -掌握 all command types
- [Agent Collaboration](./2-Core-Capabilities/02-Agent-Collaboration/README.md) - Multi-agent system design
- [Hooks Customization](./2-Core-Capabilities/03-Hooks-Customization/README.md) - Automated workflows
- [Skills Development](./2-Core-Capabilities/04-Skills-Development/README.md) - Build reusable skills
- [Rules Authoring](./2-Core-Capabilities/05-Rules-Authoring/README.md) - Customize development standards

### Stage 3: The Expert Path
- [MCP Development](./3-Expert-Path/01-MCP-Development.md) - Model Context Protocol
- [Autonomous Agents](./3-Expert-Path/02-Autonomous-Agents.md) - Build autonomous Agents
- [Advanced Patterns](./3-Expert-Path/03-Advanced-Patterns.md) - Design patterns and best practices

### Stage 4: Practical Projects
- [Project 1: CLI Tool](./4-Practical-Projects/project-1-cli.md) - Build a command-line tool
- [Project 2: Code Review](./4-Practical-Projects/project-2-review.md) - Automated review pipeline
- [Project 3: Workflow Automation](./4-Practical-Projects/project-3-auto.md) - End-to-end automation
- [Project 4: Agent System](./4-Practical-Projects/project-4-agent.md) - Multi-agent collaboration platform

### Stage 5: Quick Reference
- [Commands Cheatsheet](./5-Quick-Reference/cheatsheet-commands.md)
- [Agents Cheatsheet](./5-Quick-Reference/cheatsheet-agents.md)
- [Workflows Cheatsheet](./5-Quick-Reference/cheatsheet-workflows.md)

---

## How to Use This Course

### Recommended Learning Sequence

1. **Learn stage by stage in order** - Each stage builds on the previous one
2. **Combine theory with practice** - Each chapter has exercises, understand first then practice
3. **Complete all exercises** - Exercises are the core of the learning path

### Learning Suggestions

- **Getting Started**: 2-3 hours recommended, focus on familiarizing with basic concepts and operations
- **Core Capabilities**: 8-12 hours recommended, practice亲手 every module
- **Expert Path**: 6-8 hours recommended, requires some project experience
- **Practical Projects**: 10-15 hours recommended, choose projects you're interested in

### Learning Checkpoints

After completing each stage, confirm you have mastered:
- Ability to independently complete all operations in that stage
- Ability to explain core concepts to others
- Ability to apply knowledge to other scenarios

---

## Prerequisites

### Basic Knowledge
- Command line basics (understand basic commands)
- JavaScript/Node.js basics (needed for some advanced features)
- Git basics (version control concepts)

### Environment Requirements
- Node.js >= 18
- npm/pnpm/yarn/bun (one of these)
- Text editor (VS Code recommended)

### Prerequisites
None. Getting Started is designed for beginners.

---

## Learning Path Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                        Learn-ECC Learning Path                   │
└─────────────────────────────────────────────────────────────────┘

    Stage 1: Getting Started
        │
        ↓
    ┌───────────────────────┐
    │   Stage 2: Core       │←───────────────────┐
    │  (Commands/Agent/     │                    │
    │   Hooks/Skills/Rules) │                    │
    └───────────┬───────────┘                    │
                │                                │
                ↓                                │
    ┌───────────────────────┐                    │
    │   Stage 3: Expert      │                    │
    │  (MCP/Autonomous/     │                    │
    │   Advanced)           │                    │
    └───────────┬───────────┘                    │
                │                                │
                ↓                                │
    ┌───────────────────────┐                    │
    │   Stage 4: Projects   │────────────────────┘
    │  (CLI/Review/Auto/    │
    │   Agent)              │
    └───────────┬───────────┘
                │
                ↓
    ┌───────────────────────┐
    │   Stage 5: Quick Ref  │ ← Always accessible
    │  (Command/Agent      │
    │   Cheatsheets)        │
    └───────────────────────┘
```

**Estimated total learning time**: 26-38 hours (~2-3 weeks)

---

## Directory Structure

```
Learn-ECC/
├── 1-入门/                         # Stage 1: Getting Started (2-3 hours)
│   ├── 01-ECC-Introduction.md               # ECC overview, core value, architecture
│   ├── 02-Installation.md              # Environment setup, installation, verification
│   ├── 03-Basic-Commands.md              # /plan, /code-review, /build-fix
│   ├── 04-First-Task.md           # Complete development workflow example
│   └── exercises/
│       └── Getting-Started-Exercises.md             # 4 getting started exercises
│
├── 2-核心能力/                     # Stage 2: Core Capabilities (8-12 hours)
│   ├── 01-Command-System/
│   ├── 02-Agent-Collaboration/
│   ├── 03-Hooks-Customization/
│   ├── 04-Skills-Development/
│   └── 05-Rules-Authoring/
│
├── 3-专家之路/                     # Stage 3: Expert Path (6-8 hours)
│   ├── 01-MCP-Development.md
│   ├── 02-Autonomous-Agents.md
│   └── 03-Advanced-Patterns.md
│
├── 4-实战项目/                     # Stage 4: Practical Projects (10-15 hours)
│   ├── project-1-cli.md           # CLI tool development
│   ├── project-2-review.md        # Automated code review pipeline
│   ├── project-3-auto.md           # Workflow automation
│   └── project-4-agent.md         # Custom Agent development
│
├── 5-快速参考/                     # Stage 5: Quick Reference
│   ├── cheatsheet-commands.md     # Commands cheatsheet
│   ├── cheatsheet-agents.md       # Agents cheatsheet
│   └── cheatsheet-workflows.md    # Workflows cheatsheet
│
├── assets/                         # Image resources
├── progress-tracker.md            # Learning progress tracker
├── contributing.md                 # Contribution guide
└── README.md                       # This file
```

---

## Success Tips

1. **Progress gradually**: Don't skip stages, each Stage is the foundation for the next
2. **Hands-on practice**: Practice immediately after learning each concept, don't just watch
3. **Complete exercises**: Each Stage has exercises, complete them seriously to reinforce knowledge
4. **Use cheatsheets**: Make good use of cheatsheets in daily use, don't rote memorize
5. **Project-driven**: Stage 4 connects all knowledge through real projects
6. **Take notes**: Record learning insights and encountered problems
7. **Teach others**: You only truly understand when you can teach others

---

## Frequently Asked Questions (FAQ)

### Q: Do I need programming basics?

A: Basic command line and programming knowledge is helpful (JavaScript/Node.js basics help understand some features), but you don't need to be proficient in any specific language. Getting Started is designed for beginners.

### Q: Can I skip certain stages?

A: Stages 1-3 are recommended in order, Stage 4 can be selected based on interest, Stage 5 is always accessible.

### Q: What can I do after completing this course?

A: Master production-level Claude Code workflows, efficiently perform code development, review, testing and automation; design and implement custom Agent systems; customize Hooks and Skills.

### Q: What if I encounter problems while learning?

A: 1) Check relevant sections in [Reference Documentation](./Reference-Docs/); 2) Check [Commands Cheatsheet](./5-Quick-Reference/cheatsheet-commands.md); 3) Submit Issue for help.

### Q: How do I verify learning outcomes?

A: Use [progress-tracker.md](./progress-tracker.md) to track progress, each stage has clear learning objectives, completing all exercises indicates mastery of that stage's content.

### Q: Will course content be updated?

A: Yes, continuous updates welcome. Submit PR to contribute. See [contributing.md](./contributing.md) for detailed guide.

---

## Contributing and Feedback

If you find issues or have suggestions during learning, welcome to submit Issue or Pull Request.

---

*Last updated: 2026-05-26*