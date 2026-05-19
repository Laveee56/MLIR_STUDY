# MLIR 学习项目

个人 MLIR（Multi-Level Intermediate Representation）学习与实践仓库。通过阅读官方文档、跟随教程、动手编写 Pass 与小工具，逐步理解 MLIR 的核心概念与工程实践。

## 项目结构

| 目录 | 说明 |
|------|------|
| `llvm-project/` | LLVM / MLIR 上游源码（本地参考与编译） |
| `mlir-toy/` | 基于 MLIR Toy 教程的动手练习 |
| `mlir-tutorial/` | 教程笔记与示例 |

## Git 提交信息规范

提交信息**必须使用英文**，简洁可读，并说明**为什么**做这次改动，而不仅是改了什么。

### 格式

```
<type> short summary in imperative mood (max ~50 chars)

Optional body: motivation, scope, caveats, etc.
```

也支持带方括号的简写形式（与现有历史提交保持一致）：

```
[type] short summary in English
```

### 类型（type）

| 类型 | 用途 |
|------|------|
| `feat` | 新功能、新示例、新 Pass |
| `fix` | 修复 bug 或错误配置 |
| `docs` | 仅文档、笔记、README |
| `refactor` | 重构，不改变外部行为 |
| `test` | 测试相关 |
| `build` | 构建系统、CMake、依赖 |
| `chore` | 杂项（如 `.gitignore`、工具脚本） |

### 示例

```
[feat] add Toy dialect lexer skeleton

[docs] note MLIR dialect registration flow

[fix] ignore build artifacts in .gitignore

[build] add minimal CMake target for mlir-toy
```

### 约定

- 标题与正文均使用**英文**（imperative mood，如 `add` / `fix` / `update`）。
- 一次提交只做一件事；大改动可拆成多个逻辑清晰的 commit。
- 不要提交密钥、本地路径或个人敏感信息。
- 构建产物、IDE 配置、临时 dump 等应被 `.gitignore` 忽略，勿纳入版本库。

## 每日贡献记录

记录每天的学习与代码进展，便于回顾与坚持。

| 日期 | 内容摘要 |
|------|----------|
| 2026-05-19 | 初始化仓库；完善 `.gitignore`；创建 `mlir-toy` 子项目骨架；添加项目 README（提交规范与每日记录）；提交 `mlir-toy` 示例与 `llvm-project` submodule |

<!-- 每日更新：在表格顶部（最新日期在上）追加一行即可 -->
