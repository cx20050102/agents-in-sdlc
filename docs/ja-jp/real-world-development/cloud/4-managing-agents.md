---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習で、Copilot cloud agent にユーザー体験の改善と機能追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

GitHub.com の[エージェントページ][agents-page]など、cloud agent に割り当てたタスクを管理する方法はいくつかあります。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べ、進行中のセッションを案内できます。

このレッスンでは、次の操作を行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに機能追加を依頼して、作業を調整します。

## シナリオ

チームは、エージェントにハイコントラストモードの作成を依頼した後、ライトモードも追加することにしました。サイトのスタイルと切り替え機能を更新しているため、同じ作業に含めるのが合理的です。エージェントの作業を案内し、ハイコントラストに加えてライトモードも追加させます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot エージェントページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインのタスクリストを確認します。コードベースのドキュメント更新、ゲームを変更する API の生成、Web サイトへのハイコントラストモードの追加など、Copilot に割り当てたタスクが表示されます。
3. 実行中のタスクを 1 つ選択し、Copilot が行った操作を確認します。リポジトリのチェックアウト、作業環境の作成、MCP サーバーの設定、タスクを完了するための各種手順などが含まれます。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここでは、PR と変更されたファイルを確認して、作業を監視できます。

## cloud agent の作業を調整する

実行中のタスクを確認したので、ハイコントラストモードの作業中にライトモードの切り替えも追加するよう Copilot に依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。Copilot が付けた名前や作業状態によって、タイトルは異なります。

    ![mission control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことが表示されるまで数分待ちます。次のようなメッセージが表示されたら作業中です。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページで、作業中の cloud agent に指示するダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に追加したことを確認します。

## Copilot の作業を続ける

更新したタスクに Copilot が取り組みます。メッセージを送信した時点で実行中の手順が終わると、新しい依頼が作業に反映されます。

この作業にも数分かかります。その間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

GitHub Copilot cloud agent のタスクを監視して案内する中心的な場所、Copilot エージェントページを確認しました。ここでは、実行中や完了済みのタスクを追跡し、作業をレビューして、進行中のタスクに指示を追加できます。

次の概念を確認しました。

- mission control とエージェントページで cloud agent のタスクを監視しました。
- 進行中のセッションに機能追加を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、Copilot が作成した[プルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot エージェントページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
