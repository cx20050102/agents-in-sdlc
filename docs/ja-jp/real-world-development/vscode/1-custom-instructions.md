---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]を使うと、コード生成に関するガイダンスを Copilot に提供できます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- 現在の指示に従ってフィルター機能の最初のデータスライス (パブリッシャーヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準に従うことを確認します。
- 次の演習で Copilot が更新したガイダンスを使えるように、指示の更新とフィルタリング用スライスを `main` にコミットします。

> [!CAUTION]
> 生成コードは、設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習では、出力を一字一句一致させることではなく、指示更新後の傾向の変化を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発作業に関するガイドラインと要件があります。データレイヤーには必ず単体テストを追加すること、UI をモダンなダークモードにすること、TSDoc のドキュメントコメントを追加すること、各ファイルの先頭に内容を説明するコメントブロックを置くことなどです。指示ファイルを使い、Copilot がこれらの慣例に沿ってタスクを進めるために必要な情報を提供します。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件のコンテキストを Copilot に伝え、より適切な提案やコードを生成できるようにします。コーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定の種類のファイルに指示を適用できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、構築するものの概要、ベストプラクティスなど、プロジェクトレベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、TypeScript や Astro などの言語、UI コンポーネントの作成や単体テストなどのタスクに関するガイドラインを記述します。

> [!NOTE]
> IDE で作業する場合、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と、`applyTo` フロントマターがある `*.instructions.md` ファイルを使ってコードを生成します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。[カスタム指示の対応状況][custom-instructions-support]を参照してください。

カスタム指示ファイルは、明確で自然な言葉で記述します。プロジェクトレベルのガイダンスは `copilot-instructions.md` に、特定のファイルやタスクのガイダンスは `*.instructions.md` に記載します。必要に応じて、プロジェクトの例を参照してください。AI の使い方に唯一の正解はないため、プロジェクトに合わせて試し、調整します。

> [!TIP]
> このプロジェクトには [UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示ファイルがあります。Copilot に指示ファイルを生成してもらうこともできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。テンプレートを探す場合は [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトに含まれる指示ファイルを確認し、変更前の状態を把握します。

1. Codespace に戻ります。
2. エディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明と指示を確認します。これらはリポジトリ内で Copilot と行うすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データレイヤー、テストなどに関するファイルを確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドが `**/*.test.ts` に一致する TypeScript テストファイルなどに指示を適用することを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルやプロジェクト内のファイルへのリンクを確認します。パスは指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトの標準がありますが、コード内ドキュメントを必須にしていません。次の手順で TSDoc コメントとファイルヘッダーのルールを追加します。

## 変更用のブランチを作成する

変更にはブランチを使います。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 次のコマンドでブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

現在の指示ファイルを使って最初にプロンプトを送信し、後から指示を更新して同じプロンプトをもう一度送ることで効果を比較します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを見るようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択し、ファイル変更を適用できるようにします。

   ![Chat ビューのエージェント選択画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、ヘルパーファイルやテストなどのコードを更新する様子を確認します。
6. 提案されたヘルパーが、型付けされた関数として `db` クライアントを最初の引数に受け取り、パブリッシャーの型付き配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` の規約に基づいています。
7. 提案されたコードに TSDoc コメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する可能性があります。その場合も問題ありません。指示の更新後に、より一貫して標準が守られることが要点です。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` にプロジェクト全体の情報を追加し、コード提案を改善します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。現在はコーディング標準が記載されていますが、コード内ドキュメントのルールはありません。
3. 既存の標準の直後に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリレベルの `.github/copilot-instructions.md` は全体的な標準に、`*.instructions.md` は特定の言語、ファイルの種類、タスクに適しています。今回追加したドキュメントコメントのルールは、プロジェクト全体に適用するためリポジトリレベルのファイルに記載します。

## 同じプロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot に対象ファイルを示します。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しい標準を Copilot に伝え、次の演習で使うコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもほかのリポジトリアセットと同様に、ソース管理で管理します。ブランチに指示とヘルパーの変更をコミットしてプッシュします。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. 次のコマンドを実行し、指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基盤として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

プッシュしたブランチからプルリクエスト (PR) を作成し、バックログのドキュメント標準に関する issue と関連付けます。GitHub ツールがプロジェクトに接続されているので、Copilot に issue を探させ、issue をクローズする PR を作成してマージするよう依頼します。

> [!NOTE]
> この演習では初めて Copilot に GitHub 上の操作を依頼します。プロジェクトテンプレートでは GitHub のツールがあらかじめワークスペースに接続されています。Copilot が最初にツールを使うとき、VS Code で GitHub へのサインインを求められることがあります。画面の指示に従ってください。この接続に使われる Model Context Protocol (MCP) については、後の演習で説明します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選択されていることを確認します。
3. 次のプロンプトを使い、指示ファイルの更新に関する issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が GitHub 操作の実行許可を求めたら、コマンドを確認して、適切な操作に対して **Allow** を選択します。
5. PR の作成後、次のプロンプトで PR をマージしてローカルブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Copilot が GitHub 操作とシェルコマンドの実行許可を求めたら、コマンドを確認して、適切な操作に対して **Allow** を選択します。

GitHub Copilot の支援で PR を作成してマージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次の操作を行いました。

- 既存の指示でコード生成のプロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従うことを確認しました。
- 指示の更新とヘルパーの基盤をコミットし、次の演習で使えるように `main` にマージしました。

次は、指示を [agent mode][next-lesson] で活用し、Copilot にコードベース全体で新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
