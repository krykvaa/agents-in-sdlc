---
title: "レッスン 7 - QA エージェントを作成して使用する"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP を使ってブラウザーでフィルターの動作を確認しました。ここでは、明確に定義された QA プロセスを持つカスタムエージェントにこれらの機能を統合します。

このレッスンでは、次の操作を行います。

- カスタムエージェントが指示、スキル、MCPツールと連携する仕組みを確認する。
- 再利用可能な品質保証プロファイルを作成して確認する。
- QA エージェントを選択し、フィルタリング Issue と照らして結果をレビューする。

## シナリオ

Tailspin Toys は、pull request (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザー動作を一貫してレビューしたいと考えています。カスタムエージェントは、この QA プロセスを調整し、再利用可能なレポートを提供できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門バージョンです。プロファイルには、エージェントの目的、指示、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使用したカスタマイズには、異なる役割があります。リポジトリの指示はチームの基準を示します。`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、これらの機能を使用して要件を評価し、結果を報告する方法を Copilot に指示します。既存の機能を置き換えたり、別の会話を必要としたりしません。

## QA プロファイルを作成する

機能の PR を作成する前に、再利用可能な QA プロファイルを作成します。このプロファイルは、QA が実行するチェックと守るべき境界を定義します。

1. Codespace に戻り、フィルター会話が Interactive モードになっていることを確認します。
2. デフォルトエージェントに新しいカスタムエージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前にプロファイルを確認し、意図した QA ワークフローと境界が Copilot に反映されていることを確認します。これにより、機能の検証だけが目的なのに不完全または権限の広すぎるエージェントが機能を変更することを防げます。

1. `/diff` を入力して `.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は任意ですが、指定するとエージェントに明確な表示名がつきます。
3. プロファイルの指示を読み、QA が要件を起点にし、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使用することを確認します。
4. QA が根拠となる証拠を報告し、実装コードを変更する前に確認し、変更のコミットや pull request の作成を行わないことを確認します。
5. いずれかの責任や境界が欠けている場合は、作業を続ける前にデフォルトエージェントに修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話開始時にプロジェクトエージェントを読み込みます。プロファイルを作成したら同じフィルター会話を再開し、Issue と計画の決定事項をコンテキストとして使える QA を選択します。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成したばかりの場合、利用可能なエージェント一覧に表示されないことがあります。エージェントは作成済みなので、上記のコマンドで有効化できます。

2. 次のプロンプトで、QA エージェントに機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、提供されたレポートを確認します。

## 概要と次のステップ

再利用可能な専門的な役割をワークフローに追加し、その作業をレビューしました。このレッスンでは、次のことを行いました。

- カスタムエージェントが指示、スキル、MCPツールと連携する仕組みを確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルタリング Issue に照らして結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートがレビュー可能な状態になりました。次は、[これらを機能の PR にまとめ、Agent Merge を使用][next-lesson]します。

## リソース

- [Copilot CLI 用カスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
