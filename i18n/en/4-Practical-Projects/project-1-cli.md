# Project 1: CLI Tool

Build a command-line tool using ECC.

## Project Goal

**Estimated Duration**: 2-3 hours

Create an `ecc-gen` tool for generating ECC project components (Agent, Skill, Hook).

## Functional Requirements

### 1. Core Features
- `ecc-gen agent <name>` - Generate Agent file
- `ecc-gen skill <name>` - Generate Skill file
- `ecc-gen hook <name>` - Generate Hook configuration

### 2. Interactive Features
- Interactive Q&A generation
- Template selection
- Output path specification

### 3. Advanced Features
- Batch generation
- Custom templates
- Configuration file support

## Technical Solution

### Directory Structure

```
ecc-gen/
├── src/
│   ├── commands/
│   │   ├── agent.js
│   │   ├── skill.js
│   │   └── hook.js
│   ├── generators/
│   │   └── templates/
│   └── index.js
├── package.json
└── README.md
```

### Libraries Used
- `commander` - CLI argument parsing
- `inquirer` - Interactive Q&A
- `ejs` - Template engine

## Acceptance Criteria

- [ ] All three subcommands work correctly
- [ ] Generated template format is correct
- [ ] Supports `--output` to specify output path
- [ ] Includes unit tests
- [ ] Documentation is complete

## Learning Points

- CLI tool development
- Template system design
- Interactive input handling
- Modular architecture

> Reference commands: [/build-fix](../Reference-Docs/commands/04-构建修复.md) | [/test](../Reference-Docs/commands/02-测试相关.md)

---

[Return to Practical Projects README](./README.md)