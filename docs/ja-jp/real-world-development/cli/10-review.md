---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI ワークフロー、2つの pull request、再利用可能なカスタマイズ、その他のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使用しました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを調べ、用意されたフィルタリング Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初の pull request (PR) を手動でマージしました。
- フィルタリング Issue から開始し、Plan モードでアプローチを定義し、Autopilot モードで構築し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーの証拠を評価しました。
- 完全なフィルタリングの変更をレビューし、フィルタリング PR に対して Agent Merge を承認しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュコマンドを確認しました。

## 作成した成果

ワークショップには、更新した `main` からそれぞれ別のブランチを作成する2つの PR マイルストーンがあります。

1. **星評価:** 既存の `starRating` と、未評価であることを示す明示的な表示をゲームカードに追加します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルを作成し、関連テストを含めます。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使用しました。ワークショップを円滑に進めるため、この作業を1つの PR にまとめました。

## 異なる種類の検証

コードを複数の方法で確認しました。自動テスト、自分で行うブラウザー確認、MCP経由で Copilot が行うブラウザー確認です。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で報告しました。QA は、PR 作成前にそれらの結果を要件とテストカバレッジのレビューと統合しました。

追加するテストは実際の不足を埋めるものである必要があります。新しいテストを必要としない QA 実行も正しい場合があります。マージを承認する前にコードと証拠をレビューし、変更後は影響を受ける証拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが作業を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは会話間で再利用し、チームのニーズに応じて調整してください。指示は基準を設定し、スキルは繰り返し可能なタスクを記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールを接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った **モードとモデル** を選びます。構築前にアプローチを検討する場合は **Plan**、対象を絞った変更を常に確認する場合は **Interactive**、範囲が明確なタスクには **Autopilot** を使用します。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

インフラと同じくらいコンテキストが重要です。構築する内容、理由、方法を明確に記述することで、出力は大きく変わります。

## その他の機能

ワークショップの中心的なワークフローは完了しました。さらに確認できる CLI 機能があります。

- `/review`: コードレビューエージェントに変更の分析を依頼します。
- `/rubber-duck`: 問題を言語化し、別の視点を得ます。
- `/fleet`: 独立したサブタスクを並列に調整します。
- `/worktree`: 別タスクを分離します。
- `/delegate`: タスクを Copilot cloud agent に送信します。

## 次のステップ

ツールを使いこなす最良の方法は、継続して使用することです。本番コード、趣味のコード、長年構想していた小さなアプリに利用してください。学びをチームと共有し、チームの学びから学びましょう。また、ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI を使った pull request の管理][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
