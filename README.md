# AWS Codex Developer Guide

这是一个 Codex Skill，适合完全不熟悉 AWS、服务器和软件开发流程的人。

它会帮助 Codex 引导你完成：

- AWS 账号和 Codex 访问方式配置
- 购买域名，并配置为由 Route 53 管理 DNS
- 边做边解释 AWS、服务器、Docker、GitHub、测试和部署等概念
- GitHub、分支、提交和验证流程
- 分层 `AGENTS.md` 与项目文档维护
- Codebase-Memory MCP 使用规则
- 必要时规划 EC2、Docker、Caddy、Route 53、S3、Terraform、SSM 等 AWS 资源

## 使用方式：两步，开两个 Codex 对话

### 第一步：安装 Skill

新开一个 Codex 对话，把下面这句话发给 Codex：

```text
请帮我安装这个 Codex Skill：

https://github.com/fengzee/aws-codex-developer-guide
```

等 Codex 告诉你安装完成后，结束这个对话。

### 第二步：开始配置和开发

再新开一个 Codex 对话：

- 如果已经有项目，就在项目文件夹里打开 Codex。
- 如果还没有项目，就先在一个空文件夹里打开 Codex。

然后把下面这段发给 Codex：

```text
使用 $aws-codex-developer-guide 帮我从零建立 AWS + Codex 软件开发环境。

我是初学者，不懂 AWS、服务器和软件开发流程。请一步一步带我完成，并且：

1. 不要让我把密码、Access Key、Token 等秘密信息粘贴到聊天里。
2. 涉及花钱、开服务器、改 DNS、创建管理员权限、部署线上服务前，先解释影响并等我确认。
3. 请先默认我使用 Windows 系统；除非我明确说明不是 Windows，本地命令和说明都按 Windows 来。
4. 先检查我电脑和项目当前状态，再只问必要问题。
5. 每遇到一个重要概念，请用简短中文解释它是什么、为什么现在需要它、我应该如何判断它是否配置成功。
6. 根据我的提问和回答，判断我对 AWS、GitHub、服务器、Docker、测试、部署、文档这些概念的掌握程度，并自动调整讲解深度。
7. 帮我配置 Codex 可用的 AWS 访问方式。
8. 帮我建立 GitHub、分支、自动提交、验证命令等开发流程。
9. 帮我维护分层 AGENTS.md、docs 文档和 Codebase-Memory MCP 规则。
10. 只有确实需要时，再规划 EC2、Docker、Caddy、Route 53、S3、Terraform、SSM 等 AWS 资源。
11. 如果我没有域名，请主动带我选择并购买一个中国境外域名商的域名，再配置给 Route 53 管理；我在中国大陆，除非我明确需要境内合规，请优先建议 AWS 韩国区、新加坡等离中国较近的境外区域。

请先从环境检查和最少必要问题开始。
```
