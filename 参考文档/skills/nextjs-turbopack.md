---
name: nextjs-turbopack
description: Next.js 16+ 和 Turbopack — 增量打包、文件系统缓存、开发速度，以及何时使用 Turbopack 与 webpack。
origin: ECC
---

# Next.js 与 Turbopack

Next.js 16+ 默认使用 Turbopack 进行本地开发：一个用 Rust 编写的增量打包工具，显著加快开发启动速度和热更新。

## 何时使用

- **Turbopack（默认开发模式）**：用于日常开发。更快的冷启动和 HMR，尤其适用于大型应用。
- **Webpack（传统开发模式）**：仅在你遇到 Turbopack bug 或依赖 webpack only 插件时使用。用 `--webpack` 禁用（或者根据你的 Next.js 版本使用 `--no-turbopack`，请查看对应版本的文档）。
- **生产环境**：`next build` 的生产构建行为可能使用 Turbopack 或 webpack，取决于 Next.js 版本；请查看你使用版本的官方 Next.js 文档。

使用场景：开发或调试 Next.js 16+ 应用、诊断慢启动或 HMR 问题、优化生产包体积。

## 工作原理

- **Turbopack**：Next.js 开发的增量打包工具。使用文件系统缓存，重启快得多（例如大型项目快 5–14 倍）。
- **开发默认启用**：从 Next.js 16 起，`next dev` 默认使用 Turbopack，除非被禁用。
- **文件系统缓存**：重启时重用之前的工作；缓存通常位于 `.next` 下；基本使用无需额外配置。
- **Bundle Analyzer（Next.js 16.1+）**：实验性 Bundle Analyzer 用于检查输出和发现重型依赖；通过配置或实验性标志启用（请查看你版本的 Next.js 文档）。

## 示例

### 命令

```bash
next dev
next build
next start
```

### 使用方法

运行 `next dev` 使用 Turbopack 进行本地开发。使用 Bundle Analyzer（见 Next.js 文档）优化代码分割和精简大型依赖。优先使用 App Router 和服务端组件。

## 最佳实践

- 保持在最新的 Next.js 16.x 以获得稳定的 Turbopack 和缓存行为。
- 如果开发慢，确保你使用的是 Turbopack（默认），且缓存没有被不必要地清除。
- 对于生产包体积问题，使用你版本对应的官方 Next.js bundle 分析工具。

## 相关

- 技能: `frontend-patterns` — 前端模式参考
- 技能: `vite-patterns` — Vite 和构建工具模式