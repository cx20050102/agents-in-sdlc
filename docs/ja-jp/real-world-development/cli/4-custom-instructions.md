---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]には、求めるコードの内容だけでなく、コードの構成方法も記述できます。フィルター機能を構築したので、Copilot が参照した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される方法を確認します。
- コーディング標準が守られるよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発作業に関するガイドラインと要件があります。具体的には、次のとおりです。

- コメントでは、コードをそのまま説明するのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポートされた関数では、`db` 引数を注入できる場合はそれも含め、目的、パラメーター、戻り値を TSDoc/JSDoc で記述します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連コードの変更に合わせてコメントを更新します。
- 既存の書式設定と lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの開発慣例に沿ってタスクを進めるために必要な情報を提供します。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件のコンテキストを Copilot に伝え、より適切な提案やコードを生成できるようにします。好みのコーディング規約、ライブラリ、コメントの書き方を指定できます。リポジトリ全体、または特定のファイルやタスクに指示を適用できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送信される単一の指示ファイルです。ほとんどのチャットや CLI リクエストに関係するプロジェクトレベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスク向けのガイドラインを記述します。

> [!NOTE]
> ほかの指示形式への対応状況は環境によって異なります。特定の形式を使用する前に、[カスタム指示の対応状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、すでに指示ファイルが含まれています。変更を加える前に、現在の内容を確認して効果を比較できるようにしましょう。

1. Codespace に戻ります。
2. Codespaces エディターで、ターミナルではなく `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングガイダンスを確認します。この指示は、リポジトリ内で Copilot と行うすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドに、指示が適用されるファイルを指定する glob が設定されています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルやプロジェクトファイルへのリンクを確認します。こうすることで、大きな指示セットを小さな再利用可能なファイルに分け、Copilot に参考例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは有用ですが、まだ改善できます。`copilot-instructions.md` を編集し、新しく生成される TypeScript に TSDoc コメントを追加する標準を設定します。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** というセクションを探します。35 行目付近にあります。
2. セクションの最後に、次の箇条書きを追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を適用し、機能のコンテキストを維持するには、フィルター機能の会話を終了して再開します。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合は、フィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確かめます。

## まとめと次のステップ

Copilot CLI が指示ファイルのコンテキストをどのように取り込むか確認し、新しい標準を機能に適用しました。具体的には、次の操作を行いました。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される方法を確認しました。
- コーディング標準が守られるよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な [quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示の対応状況][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
