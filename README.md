# AWS Codex Developer Guide

这是一个 Codex Skill，适合完全不熟悉 AWS、服务器和软件开发流程的人。

它会帮助 Codex 引导你完成：

- AWS 账号和 Codex 访问方式配置
- GitHub、分支、提交和验证流程
- 分层 `AGENTS.md` 与项目文档维护
- Codebase-Memory MCP 使用规则
- 必要时规划 EC2、Docker、Caddy、Route 53、S3、Terraform、SSM 等 AWS 资源

## 使用方式

在 Codex 中安装这个 Skill：

https://github.com/fengzee/aws-codex-developer-guide

安装后，新开一个 Codex 对话，复制下面这段：

```text
使用 $aws-codex-developer-guide 帮我从零建立 AWS + Codex 软件开发环境。

我是初学者，不懂 AWS、服务器和软件开发流程。请一步一步带我完成，并且：

1. 不要让我把密码、Access Key、Token 等秘密信息粘贴到聊天里。
2. 涉及花钱、开服务器、改 DNS、创建管理员权限、部署线上服务前，先解释影响并等我确认。
3. 先检查我电脑和项目当前状态，再只问必要问题。
4. 帮我配置 Codex 可用的 AWS 访问方式。
5. 帮我建立 GitHub、分支、自动提交、验证命令等开发流程。
6. 帮我维护分层 AGENTS.md、docs 文档和 Codebase-Memory MCP 规则。
7. 只有确实需要时，再规划 EC2、Docker、Caddy、Route 53、S3、Terraform、SSM 等 AWS 资源。

请先从环境检查和最少必要问题开始。
```

如果已经有项目，就在项目文件夹里打开 Codex；如果还没有项目，就先在一个空文件夹里打开 Codex。
