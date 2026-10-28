---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は Copilot CLI を基盤とするデスクトップアプリケーションで、エージェント主導の開発を 1 つの集中できるワークスペースで実現します。並行するエージェントセッション、切り替え可能なセッションモード、共有キャンバス、GitHub issue とプルリクエストのネイティブな管理機能を備えています。また、リベース、レビューのフィードバック、継続的インテグレーション (CI) の修正、マージまでプルリクエストを進める **Agent Merge** も利用できます。

このワークショップでは、Tailspin Toys の一連のワークフローを進めます。

1. プロジェクトを準備し、アプリをインストールしてリポジトリに接続し、ワークスペースと作成済みのバックログを確認します。
2. 星評価の変更を加えてブラウザーでレビューし、最初のプルリクエスト (PR) を手動でマージします。
3. フィルター機能の issue から始め、**Plan** モードで方針を定め、**Autopilot** モードで構築して、**Interactive** モードでレビューします。
4. リポジトリの指示を更新し、フィルター機能の作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルター機能を確認します。
7. 品質保証 (QA) カスタムエージェントを作成し、要件、カバレッジ、検証の根拠をレビューします。
8. フィルター機能全体をレビューし、2 つ目の PR に Agent Merge を使用します。
9. キャンバスが利用できるか確認してから、Database Explorer を使い、リポジトリに保存するトリアージキャンバスを作成してテストします。必要な機能を利用できない場合は、ブロッカーを記録し、読み取り専用の代替手段を使うか、まとめに進みます。

ワークショップの焦点を絞るため、PR は 2 つ作成します。星評価の変更と、指示の更新、スキルの更新、QA プロファイル、テストを含むフィルター機能です。どちらも更新済みの `main` から始めます。フィルター機能と品質ワークフローでは、1 つのセッション、worktree、ブランチを共有し、各ツールを学びながら既存の作業を引き継ぎます。最後のキャンバス演習では同じセッションを使い、PR ワークフローを繰り返さずに共有スペースの作成とテストに集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトの自分用コピーを作成します |
| [1. Copilot app のインストール][ex1] | セットアップ | アプリをインストールしてプロジェクトに接続し、ワークスペースを確認します |
| [2. 星評価を追加する: 小さな改善][ex2] | 最初の変更 | 既存の評価と未評価の場合の表示を追加し、PR 1 をマージします |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | issue から機能を計画し、Autopilot で構築して、Interactive モードでレビューします |
| [4. カスタム指示で Copilot をガイドする][ex4] | コンテキスト | 指示を確認して更新し、フィルター機能に適用します |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し実行できるチェック | 既存のスキルを確認し、レポート形式を変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | Customize から MCP を構成し、フィルターの動作を確認します |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門的なプロファイルを作成して選択し、最終検証の根拠を集めます |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | フィルター機能、指示、スキル、QA プロファイル、テストをレビューし、2 つ目の PR に Agent Merge を使います |
| [9. キャンバスを確認して作成する][ex9] | コラボレーション | Database Explorer を使い、リポジトリに保存するトリアージキャンバスを作成してテストします |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、成果物、追加のリソースを確認します |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを利用できる GitHub アカウント
- [ ] **macOS、Linux、Windows** のいずれかを実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランを利用していない場合、認定学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI が含まれているため、すべてのハーネスを完了できます。

> [!NOTE]
> Copilot app は Codespace ではなく自分のコンピューターで動作します。[前提条件のレッスン][ex0]では、アプリをインストールする前に Node.js をインストールし、プロジェクトの自分用コピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。これは Copilot CLI のポリシーとは別です。

## はじめに

**[前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
