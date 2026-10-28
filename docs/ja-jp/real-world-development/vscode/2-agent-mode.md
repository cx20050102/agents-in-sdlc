---
title: "演習 2 - Copilot agent mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、複数のファイル変更やテスト実行などが必要になることがあります。開発者は必要なファイルを探し、変更を加え、テストを実行し、問題を修正して、変更漏れがあればさらに修正します。作業は次々と増えていきます。

ここで Copilot Agent Mode が役立ちます。

Copilot Agent Mode は、統合開発環境 (IDE) 内でより自律的に作業します。開発者のようにプロジェクト構造を調べ、必要なファイルを変更し、テストなどのタスクを実行して、発見した問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けです。Agent mode は IDE 内で作業し、すばやいフィードバックとやりとりができます。Cloud agent は issue とプルリクエストを通じて非同期に作業する、チームのペアプログラマーとして設計されています。

この演習では、次の操作を行います。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数の変更を調整する方法を確認します。
- GitHub Copilot Agent Mode を使い、ユーザーインターフェイス (UI) とデータレイヤーにまたがる新機能を実装します。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューします。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリとパブリッシャーで絞り込めるようにします。前の演習ではパブリッシャーヘルパーを追加しました。Copilot Agent Mode を使って、残りのデータレイヤー、UI、テストを完成させます。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys Web サイトの現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲーム一覧と詳細を表示します。単一の Astro アプリで、ビルド時に静的 HTML としてページをレンダリングします。ページは frontmatter 内で Drizzle オブジェクトリレーショナルマッピング (ORM) を通じてローカル SQLite データベースを直接参照します。別個のバックエンドのアプリケーションプログラミングインターフェイス (API) やクライアントサイドのユーザーインターフェイス (UI) フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな動作には標準のドキュメントオブジェクトモデル (DOM) API を使う小さな Astro `<script>` を使います。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクを使います。

1. 前の演習の後に `main` に戻っていることを確認し、Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択して、サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは、Web サーバーをホストする Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、現在の機能を確認します。

- **Home Page**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **Game Details Page**: ゲームを選択すると、そのタイトル、説明、パブリッシャー、カテゴリなどを表示する詳細ページが開きます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

最初の Web サイトは動作しますが、さらに機能を追加します。テンプレートからプロジェクトを作成したときに、issue のバックログも自動的に作られています。GitHub Copilot に項目を表示してもらいます。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。Agent は複数のファイルにまたがる変更を計画・実装し、ターミナルコマンドやツールを実行します。

   ![Chat ビューのエージェント選択画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップは、特定のモデルを推奨するものではありません。作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案を比較的安定させることを意図していますが、LLM は確率的に動作するため、説明と異なる提案が表示される場合があります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な動作により、Copilot は別の MCP コマンドを使う場合がありますが、タスクを完了できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログの issue を確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーに最も有用だと考える項目を優先順位付けしたことを確認できます。

## 指示ファイルを確認する

Copilot にコードを生成させる前に、コンテキストを与える指示ファイルを確認します。Web サイトの機能追加方法に関するコンテキストを含む [ユーザーインターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使用します。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関するガイダンスを確認します。アーキテクチャの概要、コンポーネントの設計、テスト容易性とアクセシビリティの原則、Astro、Tailwind CSS、Drizzle などのファイル種類ごとの指示ファイルへのリンクが含まれています。

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。複雑な指示を再利用しやすい小さな単位に分け、例やテンプレートを示すために使えます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも 3 つの更新が必要です。

- データレイヤー (`src/lib/`) に残りのフィルターロジックを追加または調整します。
- フィルター動作のテストを追加または更新します。
- ゲーム一覧ページを更新し、フィルター UI を追加します。

さらに、コードベースにマージする前にテストを実行して成功を確認します。Copilot Agent Mode にこれらの作業を依頼します。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルの選択例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** であることを確認します。
5. バックログの関連 issue に基づいて実装するよう、次のプロンプトを入力します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、関連するファイルを特定し、データレイヤー、UI、テストを更新してテストを実行する様子を確認します。

   ![Copilot がプロジェクトのファイルを調べる画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行うため、複雑さによっては数分かかります。作業中、エディターに **Keep** と **Undo** が表示されることがあります。完了後はすべての変更に **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot はエラーを検出しなくなるまでコード生成とテストを繰り返します。

   ![Copilot Agent Mode の完了したチャットセッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を試します。
11. 動作を確認し、コードをレビューしたら Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys Web サイトに機能を追加しました。次のことを確認しました。

- GitHub Copilot Agent Mode が UI とデータレイヤーにまたがる機能を実装する方法。
- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数の変更を調整する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナスの演習 — ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。この演習で学んだ方法を使い、Copilot にページネーションの実装を依頼します。既存の指示ファイルなどのベストプラクティスに従い、ページサイズをユーザーが選べるようにするか固定値にするかを検討します。希望する実装に必要なガイダンスをプロンプトに含め、必要に応じて Copilot に変更やコンテキストを伝えて反復します。

## リソース

- [Copilot の ask、edit、agent mode: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
