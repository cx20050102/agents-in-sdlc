---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを利用できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何を実行するかだけでなく、どのように実行するかも伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code および CLI のハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub issue を使って非同期に動作するため、リアルタイムで並べて効果を示すのが難しいためです。後続の演習で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発作業に関するガイドラインと要件があります。具体的には、次のとおりです。

- データレイヤーには必ず単体テストを追加します。
- UI はダークモードで、モダンなデザインにします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの内容を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの慣例に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件のコンテキストを Copilot に伝え、より適切な提案やコードを生成できるようにします。コーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定の種類のファイルに指示を適用できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、構築するものの概要、ベストプラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクトレベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語 (TypeScript など) や UI コンポーネント、単体テストの作成など、特定のタスクやファイルの種類向けの指示を記述します。

> [!NOTE]
> 統合開発環境 (IDE) で作業する場合、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と、`applyTo` フロントマターがある `*.instructions.md` ファイルを使ってコードを生成します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。[カスタム指示の対応状況][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することは、このワークショップの範囲外です。サンプルプロジェクトの例から、一般的な方法を確認できます。

- `copilot-instructions.md` には、構築するもの、プロジェクトの構造、全体的なコーディング標準など、プロジェクトレベルのガイダンスを記述します。
- `*.instructions.md` ファイルには、単体テスト、Astro コンポーネント、データレイヤーなど、特定のファイル種類やタスクに関する指示を記述します。
- 自然な言葉を使い、明確なガイダンスと、コードの適切な例・不適切な例を示します。

指示ファイルの作り方や AI の使い方に、唯一の正解はありません。プロジェクトで試しながら最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトには [UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを生成してもらうこともできます。各環境で方法は異なります (VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など)。関連するレッスンで手順を説明します。
>
> テンプレートやひな形を探す場合は、指示ファイル、カスタムエージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。共通の `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** の指示にも注目してください。これらは Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドには、指示を適用するファイルをリポジトリのルートから指定する glob が設定されています。ここでは、`**/*.test.ts` に一致する TypeScript テストファイルなどが対象です。
5. このプロジェクトで単体テストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイル (例: `unit-tests.instructions.md`) や既存ファイルへのリンクを確認します。こうすることで、大きな指示セットを小さな再利用可能なファイルに分け、コード生成時の参考例を Copilot に示せます。ここに記載されたパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準がありますが、コード内のドキュメント化はまだ必須ではありません。次の手順で、TSDoc コメントとファイルヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

ここで行う編集は、TSDoc コメントとファイルレベルのコメントヘッダーをコードに追加するルールです。後続の演習で cloud agent が issue に取り組むときに、この標準を参照します。

編集する前に作業ブランチを作成します。cloud agent は issue の対象ブランチから指示を読み込みますが、変更を整理してコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` はプロジェクトレベルの情報を Copilot に提供します。コードの提案を改善するため、コーディング標準に必要なルールを追加します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。現在はコーディング標準が記載されていますが、コード内ドキュメントのルールがありません。
3. 既存の標準の直後に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体に適用する `.github/copilot-instructions.md` と、特定の言語、ファイル種類、タスクに使う `*.instructions.md` のどちらにも指示を記述できます。追加したドキュメントコメントのルールのように、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## 指示の更新をコミット、プッシュしてマージする

cloud agent は issue の対象ブランチにある指示ファイルを読み込みます。次の演習で issue を Copilot に割り当てると、`main` からブランチが作成されます。cloud agent が新しい指示を読み込めるよう、変更を `main` にマージします。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で issue を割り当てると、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチ手順を省略して `main` に直接コミットできます。ブランチを使う手順は、実際のプロジェクトでの作業方法を再現しています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。cloud agent がこのハーネスの後続の作業で作成するプルリクエストに、その標準が適用されます。

次は、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot をカスタマイズする指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
