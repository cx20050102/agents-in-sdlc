---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能获取这些上下文。[指令文件][instruction-files]可提供相关指导，让 Copilot 不仅了解要做什么，也了解应该如何完成。

在本练习中，你将：

- 了解项目上下文、编码指南和文档规范如何通过存储库自定义指令及按路径限定的指令文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级规范。

> [!NOTE]
> 与 VS Code 和 CLI 开发环境不同，本练习不会比较提示执行前后的效果。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示差异。稍后审查云智能体创建的拉取请求时，你会看到指令文件产生的影响。

## 指令文件

### 场景

Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代风格。
- 应通过 TSDoc（TypeScript 文档注释）为代码添加文档。
- 每个文件开头都应添加注释块，说明文件的用途。

通过指令文件提供这些信息，确保 Copilot 按照上述实践完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以指定编码规范、库以及注释风格等偏好，为 Copilot 提供更相关的建议和代码片段。你可以创建适用于整个存储库的指令，也可以针对特定文件类型创建任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md` 是单个指令文件，会随该存储库的**每个**请求发送给 Copilot。它应包含对大多数聊天或 CLI 请求都相关的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 可针对特定任务或文件类型创建，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在集成开发环境中，指令文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他规范的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本工作坊的范围。示例项目展示了一种有代表性的做法，主要原则如下：

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目简介、结构和全局编码规范。
- 使用 `*.instructions.md` 为特定文件类型（如单元测试、Astro 组件、数据层）或任务提供具体指令。
- 使用自然语言，确保指导清晰，并提供代码示例说明正确和错误的写法。

创建指令文件没有唯一方法，使用 AI 也是如此。通过实践找到适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的指令文件。查看本项目中的文件时，你可能会发现有些针对特定任务，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品界面提供的入口各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前开发环境对应的课程会在相关处说明。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义指令文件

花些时间阅读此存储库自带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些指导适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并查看其中的文件，包括针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了相对于存储库根目录的 glob，用于确定指令适用的文件。这里，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些指令。
5. 查看针对本项目单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。注意它指向的其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件。这样可以将较大的指令集拆分成可复用的小文件，并为 Copilot 提供生成代码时可参考的示例。文件中的路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码规范，但尚未要求添加代码文档。接下来要添加 TSDoc 注释和文件级注释头的规则。

## 添加存储库级规范

接下来只需进行一项编辑：添加项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。后续练习中，云智能体处理分配给它的议题时会读取这项规则。

编辑前先创建一个分支。（云智能体会读取议题目标分支中的指令，不过这里仍会单独提交更改。）

1. 在 Codespace 终端中创建并切换到新分支：

    ```bash
    git checkout -b custom-instructions-cloud
    ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码规范，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意该部分记录了项目编码规范，但尚未要求为代码添加文档。
3. 在现有规范下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

    ```markdown
    - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
    - Before imports or any code, add a comment block to the file that explains its purpose.
    ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 正如前一课所介绍的，指令文件可以放在存储库级别（`.github/copilot-instructions.md`）以提供全局指导，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指令。项目级文件适合放置刚添加的文档注释等全局规范。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令。下一练习中将把议题分配给 Copilot，而它会从 `main` 分支创建新分支。因此，必须先将指令更改合并到 `main`，云智能体才能读取。

1. 暂存并提交更改：

    ```bash
    git add .github/copilot-instructions.md
    git commit -m "Add doc comment and file-header standards to copilot instructions"
    ```

2. 推送分支：

    ```bash
    git push -u origin custom-instructions-cloud
    ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。下一练习中云智能体处理议题时，就会读取这些指令。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main` 分支，可以跳过创建分支的步骤，直接提交到 `main`。此处采用分支流程，是为了模拟真实项目中的工作方式。

## 总结与后续步骤

你了解了 Copilot 如何从指令文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级规范。后续练习中，云智能体生成的拉取请求将体现这项规范。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更优 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
