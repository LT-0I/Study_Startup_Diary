# Belief - 学习日志

> **说明：** 本日志用于记录学习过程、遇到的问题、AI 助手使用情况等。请每周至少更新一次。

---

## Date/Week: YYYY-MM-DD / Week X

### Learning Content
*本周/本次学习的主要内容是什么？学习了哪些知识点？*

- 学习内容 1
- 学习内容 2
- ...

### Key Takeaways
*本周/本次学习的关键收获和理解*

- 收获 1
- 收获 2
- ...

### Practice & Code (Link to Commit)
*本周/本次完成的实践任务和代码*

- 完成的任务：xxx
- 代码提交链接：
  - [Commit 1](link-to-commit)
  - [Commit 2](link-to-commit)

### Problems Encountered & Solutions
*遇到的问题及解决方案*

**问题 1：**
- 描述：xxx
- 解决方案：xxx
- 参考资料：xxx

**问题 2：**
- 描述：xxx
- 解决方案：xxx

### AI Assistant Usage
*记录 AI 编程助手的使用情况（重要！）*

**使用场景 1：**
- **Tool Used:** (e.g., Cursor AI, GitHub Copilot, ChatGPT)
- **Problem Addressed:** (遇到了什么问题？为什么使用 AI？)
- **Key Prompt:** (使用的关键提示词/问题)
  ```
  [粘贴你的提示词]
  ```
- **Effectiveness & Reflection:** (AI 的帮助效果如何？你从中学到了什么？有哪些需要改进的地方？)

**使用场景 2：**
- **Tool Used:** 
- **Problem Addressed:** 
- **Key Prompt:** 
- **Effectiveness & Reflection:** 

### Reflections & Next Steps
*本周学习的总体反思和下一步计划*

**反思：**
- 这周学得怎么样？
- 哪些地方理解得不够深入？
- 学习方法是否需要调整？

**下一步计划：**
- [ ] 下周任务 1
- [ ] 下周任务 2
- [ ] 需要深入学习的知识点

---

## 日志模板使用说明

1. **每周至少更新一次**，可以在周末总结本周学习
2. **每个部分都要填写**，特别是 AI 助手使用情况
3. **代码提交链接**要具体到某个 commit，方便回顾
4. **问题与解决方案**要详细记录，方便以后查阅
5. **AI 使用记录**要如实填写，反思 AI 的帮助和局限

---

## 示例条目（供参考）

## Date/Week: 2025-10-21 / Week 1

### Learning Content
- 学习了 Linux 基本命令（cd, ls, mkdir, cp, mv, rm）
- 配置了 Ubuntu 国内镜像源（使用清华源）
- 学习了环境变量的概念和使用方法
- 开始学习 Git 基础（git init, add, commit）

### Key Takeaways
- Linux 命令行比想象中强大，很多操作比图形界面更高效
- 理解了相对路径和绝对路径的区别
- 明白了为什么需要配置国内镜像源（下载速度提升明显）
- Git 的版本控制思想很重要，每次提交都是一个快照

### Practice & Code (Link to Commit)
- 完成任务 1.1：Linux 命令行练习
- 完成任务 1.2：配置镜像源并添加用户到 dialout 组
- 开始任务 1.3：编写 Shell 脚本（进行中）
- 代码提交：
  - [Initial commit: setup project structure](https://github.com/username/repo/commit/abc123)

### Problems Encountered & Solutions

**问题 1：配置镜像源后 apt update 报错**
- 描述：修改 `/etc/apt/sources.list` 后运行 `sudo apt update` 报 404 错误
- 解决方案：发现是我的 Ubuntu 版本是 20.04 (focal)，但配置文件中写的是 jammy。修改为正确的版本代号后问题解决
- 参考资料：[Ubuntu 镜像源配置教程](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)

**问题 2：git push 时需要输入密码**
- 描述：每次 push 都要输入 GitHub 密码，很麻烦
- 解决方案：配置了 SSH 密钥认证，现在不需要输入密码了
- 参考资料：[GitHub SSH 配置文档](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

### AI Assistant Usage

**使用场景 1：Shell 脚本语法问题**
- **Tool Used:** Cursor AI
- **Problem Addressed:** 不清楚如何在 Shell 脚本中判断目录是否存在
- **Key Prompt:** 
  ```
  如何在 Bash 脚本中判断一个目录是否存在？如果不存在则创建它。
  ```
- **Effectiveness & Reflection:** 
  - AI 提供了 `[ -d "dirname" ]` 的语法，并给出了完整示例
  - 很有帮助，但我发现 AI 没有解释 `-d` 参数的含义
  - 后来我自己查阅了 `man test` 文档，理解了 `-d`, `-f`, `-e` 等参数的区别
  - **反思：** AI 可以快速给出答案，但要主动查阅文档理解原理

**使用场景 2：Git 分支操作**
- **Tool Used:** ChatGPT
- **Problem Addressed:** 需要创建新分支进行实验，但不确定具体操作流程
- **Key Prompt:** 
  ```
  如何在 Git 中创建新分支并切换到该分支？完成工作后如何合并回 main 分支？
  ```
- **Effectiveness & Reflection:** 
  - AI 给出了 `git checkout -b branch_name` 和合并流程
  - 非常清晰，但我实际操作时遇到了合并冲突
  - **反思：** AI 教了基本操作，但真实场景更复杂。需要多实践。

### Reflections & Next Steps

**反思：**
- 这周学习进度比预期快，Linux 命令和 Git 基础已经基本掌握
- 使用 AI 助手确实提高了效率，但要注意不能完全依赖，需要理解原理
- 实践很重要，光看教程不够，必须自己动手操作

**下一步计划：**
- [ ] 完成 Shell 脚本任务（setup_project.sh）
- [ ] 创建 GitHub 仓库并练习分支、合并操作
- [ ] 开始学习 C++ 基础（LearnCpp.com）
- [ ] 深入学习 Git 分支管理和冲突解决

---

*（在上方添加新的日志条目）*

