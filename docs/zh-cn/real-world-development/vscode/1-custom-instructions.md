---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能获取这些上下文。可以通过多种方式向 Copilot 提供具体上下文，其中重要的一种方式是使用[指令文件][instruction-files]，指导代码生成。

在本练习中，你将：

- 了解项目上下文、编码指南和文档规范如何通过存储库自定义指令及按路径限定的指令文件传递给 Copilot。
- 在现有指令下生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级规范。
- 再次运行相同提示，观察生成的代码如何采用新规范。
- 将指令更新和筛选功能切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合所有规范。Copilot 的行为具有非确定性。本练习旨在观察更新指令前后行为的*趋势*，而不是要求逐字匹配输出。

## 指令文件

### 场景

Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面（UI）应采用深色模式并具有现代风格。
- 应通过 TSDoc（TypeScript 文档注释）为代码添加文档。
- 每个文件开头都应添加注释块，说明文件的用途。

通过指令文件提供这些信息，确保 Copilot 按照上述实践完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以指定编码规范、库以及注释风格等偏好，为 Copilot 提供更相关的建议和代码片段。你可以创建适用于整个存储库的指令，也可以针对特定文件类型创建任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md` 是单个指令文件，会随该存储库的**每个**请求发送给 Copilot。它应包含对大多数请求都相关的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 可针对特定任务或文件类型创建，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在集成开发环境（IDE）中，指令文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
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

先查看项目附带的一组指令文件：一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。这些指令适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并查看其中的文件，包括针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个 glob，用于确定指令适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向的其他指令文件和项目文件。这种做法可将较大的指令集拆分成可复用的小文件，并为 Copilot 提供生成代码时可参考的示例。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码规范，但尚未要求添加代码文档。接下来要添加 TSDoc（TypeScript 文档注释）和文件级注释头的规则。

## 为更改创建分支

按照最佳实践，为本次更改创建分支。

1. 返回上一练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

    ```bash
    git checkout -b custom-instructions
    ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新的对话。模式和模型的选择因练习而异；每个步骤会在相关处说明。

要查看自定义指令的影响，先在现有指令下发送提示。稍后更新指令后，再次发送相同提示并比较结果。

1. 关闭之前练习中打开的编辑器选项卡，避免 Copilot 获取不必要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 修改文件。

    ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

    ```plaintext
    Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
    ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意生成的辅助函数是一个带类型的函数，它将 `db` 客户端作为第一个参数，并返回带类型的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层规范，该文件适用于 `src/lib/*.ts`。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此即使没有明确要求，也可能添加文档注释。如果发生这种情况也没关系，本练习重点在于更新指令后行为的一致性提升。

## 添加存储库级规范

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

## 重新发送提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除上下文。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前**完全相同的提示**：

    ```plaintext
    Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
    ```

4. 注意生成文件现在以类似下面的注释块开头：

    ```typescript
    /**
     * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
     * 提供从数据库检索发行商信息的函数。
     */
    ```

5. 注意生成的函数现在包含类似下面的 TSDoc 注释：

    ```typescript
    /**
     * 返回所有发行商的 ID 和名称列表。
     *
     * @param db - Drizzle 数据库客户端。
     * @returns 一个 Promise，解析为发行商对象数组。
     */
    ```

现在，你已引导 Copilot 遵循新的项目规范，并将其应用到真实代码中，供下一练习继续使用。

## 提交指令并推送分支

指令文件和存储库中的其他资源一样，也需要通过源代码管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改均已存在：

    ```bash
    git status
    ```

3. 暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有更改：

    ```bash
    git add .github/copilot-instructions.md src/lib/publishers.ts
    # 如果 git status 显示还有其他相关更改（例如测试），也要添加这些文件。
    git commit -m "Add doc comment standards and publishers helper foundation"
    ```

4. 将分支推送到存储库：

    ```bash
    git push -u origin custom-instructions
    ```

## 创建并合并拉取请求

分支推送完成后，创建拉取请求，并关联待办事项中的文档规范议题。Copilot 已连接项目的 GitHub 工具，可以代你查找议题、创建关联该议题的 PR 并合并。

> [!NOTE]
> 这是第一个让 Copilot 代表你操作 GitHub 的练习。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用其中一个工具时，VS Code 可能会提示你登录 GitHub。按提示授权即可。后续练习会介绍这种连接所使用的模型上下文协议（MCP）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保智能体选择器中选中 **Agent**。
3. 让 Copilot 查找更新指令文件的议题，并从当前分支创建 PR，说明指令更新和新的发行商辅助函数基础：

    ```plaintext
    Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
    ```

4. Copilot 会查找议题并创建 PR。
5. 如果 Copilot 提示 **Allow** 其代表你执行 GitHub 操作，请审查操作并酌情选择 **Allow**。
6. PR 创建后，使用以下提示要求 Copilot 将 PR 合并到 `main`，并将本地分支切回 `main`：

    ```plaintext
    Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
    ```

7. 如果 Copilot 提示 **Allow** 其代表你执行 GitHub 操作和运行 shell 命令，请先审查操作，再酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并拉取请求。

## 总结与后续步骤

你了解了 Copilot 如何从指令文件获取上下文，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，观察 Copilot 如何根据*现有*指令生成内容。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级规范。
- 再次发送相同提示，观察生成的代码如何采用新规范。
- 将指令更新和辅助函数基础提交到 `main`，供下一练习使用。

接下来，在[智能体模式][next-lesson]中运用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更优 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
