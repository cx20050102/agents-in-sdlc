---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证（QA）配置文件和测试都已保存在同一分支上。现在一起审查这些更改并创建拉取请求。星级评分 PR 是你手动合并的；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会把功能、指令更新、技能更新和 QA 智能体拆分到几个 PR 中。为简化本工作坊，我们将完整的筛选和质量工作流保留在同一个对话和分支中，并集中提交到这个 PR。

在本课中，你将：

- 了解 Agent Merge 及其如何自动化 PR 合并流程。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能工作流中，你已使用 Copilot 规划、实现并验证了一项功能。Tailspin Toys 现在希望自动化 PR 的后续工作，同时由开发者掌控合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的后续工作。启用后，Copilot 会解决阻止 PR 合并的问题，例如修复持续集成（CI）检查失败、回复审查意见，以及在需要时进行变基；如果存储库允许，之后会启用 GitHub 自动合并。

之前都是你亲自选择 **Merge pull request**。Agent Merge 可以代为处理，但启用之前仍应先审查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码已完成。接下来一起审查代码、创建 PR，并让 Agent Merge 管理剩余流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建新的 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它将执行合并。
7. 确认收到 Copilot 的消息，内容类似于 “PR #14 was squash-merged successfully.”

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限要求。如果流程受阻，请查看报告的原因，并在存储库允许时手动完成已审查的合并。

## 总结与后续步骤

你已自动化开发流程中的多个环节，包括生成代码、测试和验证代码，以及管理拉取请求。本课中，你：

- 了解了 Agent Merge 及其如何自动化 PR 合并流程。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，[探索 GitHub Copilot CLI 中更多实用的斜杠命令][next-lesson]，管理上下文、模型、共享和可选的云委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
