---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローを進めました。次の操作を行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、登録済みのフィルター機能の issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の issue から始め、Plan モードで方針を定め、Autopilot モードで構築し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 要件、テストカバレッジ、スキルの結果、ブラウザーの根拠を評価する品質保証 (QA) カスタムエージェントを作成して選択しました。
- フィルター機能全体をレビューし、Agent Merge を使ってフィルター機能の PR をマージしました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュコマンドを確認しました。

## リリースした内容

このワークショップには、更新した `main` から別々に作成する 2 つのブランチと PR があります。

1. **星評価**: ゲームカードに既存の `starRating` と未評価であることを明示する表示を追加します。
2. **フィルター機能と品質ワークフロー**: フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズして QA プロファイルを作成し、関連するテストを追加します。

フィルター機能の計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡略化するため、これらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザーの確認、MCP を使った Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、更新した形式で結果を報告しました。QA はこれらの結果に要件とテストカバレッジのレビューをまとめてから、PR を作成できるか判断しました。

テストは実際に不足している箇所を補うものでなければなりません。追加のテストが不要な QA の実行結果も正しい場合があります。マージを承認する前に、コードと検証の根拠を確認し、変更後は影響を受ける根拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールは、作業結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、スキルは繰り返し行うタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて **モードとモデル** を選択します。構築前に方針を検討するには **Plan**、焦点を絞った変更を進めながら状況を確認するには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を、なぜ、どのように構築するのかを明確に説明すると、結果が大きく変わります。

## さらに確認する

主要なワークフローは完了しました。Copilot CLI の機能として、次のコマンドも確認できます。

- `/review` を使って、コードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` を使って問題を相談し、別の視点を得ます。
- `/fleet` を使って、独立したサブタスクを並行して実行します。
- `/worktree` を使って、別のタスクを分離します。
- `/delegate` を使って、Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールでも、上達するには使い続けることが大切です。本番コード、趣味のコード、何年も作りたいと思っていた小さなアプリなどで活用してください。チームと知見を共有し、互いに学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認します。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
