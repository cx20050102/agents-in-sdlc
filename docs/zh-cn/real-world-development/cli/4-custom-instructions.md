---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "查看存储库指令、添加文档规范，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，就要让 Copilot 能够获取相关指导。[指令文件][instruction-files]不仅说明希望编写什么代码，也说明代码应如何组织。现在筛选功能已经完成，你将查看 Copilot 使用的指令、添加文档规范，并将其应用于代码。

在本课中，你将：

- 了解存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码规范。
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc（TypeScript 文档注释）或 JSDoc（JavaScript 文档注释）说明用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持更新。
- 保留现有格式和 lint 指南。

通过指令文件提供这些信息，确保 Copilot 按照上述实践完成任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以指定编码规范、库以及注释风格等偏好，为 Copilot 提供更相关的建议和代码片段。指令既可以适用于整个存储库，也可以针对特定文件类型提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md` 是单个指令文件，会随该存储库的**每个**请求发送给 Copilot。它应包含对大多数请求都相关的项目级信息。
- `.github/instructions/*.instructions.md` 可针对特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他指令格式及其支持情况因开发环境而异。依赖特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 查看项目中的自定义指令文件

初始项目已包含一组指令文件。先查看现有内容，再进行更改，了解其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看文件，了解项目简介和编码指导。这些指令适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并查看其中的文件，包括针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个 glob，用于确定指令适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向的其他指令文件和项目文件。这种做法可将较大的指令集拆分成可复用的小文件，并为 Copilot 提供生成代码时可参考的示例。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting guidance** 部分记录了项目编码规范，但尚未要求添加代码文档。接下来要添加 TSDoc 注释和文件级注释头的规则。

## 更新指令文件以符合团队规范

现有文件是不错的起点，但仍缺少一项要求。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，位置大约在第 35 行。
2. 添加以下要点，作为该部分的最后一项：

    ```markdown
    - All new TypeScript should contain TSDocs comments for documentation purposes.
    ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。保存更新后的指令后，退出并恢复筛选功能对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选功能对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中恢复已保存的对话：

    ```bash
    copilot --resume
    ```

    如果出现提示，请选择筛选功能对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

    ```plaintext
    We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
    ```

4. 输入 `/diff`，检查更改的 TypeScript 文件。查看新添加的 TSDoc 注释，并确认其准确说明了代码。

## 总结与后续步骤

你了解了 Copilot CLI 如何从指令文件获取上下文，并将新的规范应用于功能。本课中，你：

- 了解存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码规范。
- 观察了指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
