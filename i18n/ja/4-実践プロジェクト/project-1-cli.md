# Project 1: CLI Tool

ECC を使用してコマンドラインツールを構築します。

## Project Goal

**Estimated time**: 2-3 hours

Create an `ecc-gen` tool for generating ECC project components (Agent, Skill, Hook).

## Feature Requirements

### 1. Core Features
- `ecc-gen agent <name>` - Generate Agent files
- `ecc-gen skill <name>` - Generate Skill files
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
- `commander` - CLI parameter parsing
- `inquirer` - Interactive Q&A
- `ejs` - Template engine

## Acceptance Criteria

- [ ] All three subcommands work properly
- [ ] Generated templates have correct format
- [ ] Support `--output` to specify output path
- [ ] Include unit tests
- [ ] Complete documentation

## Learning Points

- CLI tool development
- Template system design
- Interactive input processing
- Modular architecture

> Reference commands: [/build-fix](../参考ドキュメント/commands/04-构建修复.md) | [/test](../参考ドキュメント/commands/02-测试相关.md)

---

[Return to practical projects directory](./README.md)
