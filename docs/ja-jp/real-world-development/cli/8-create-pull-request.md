---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリング実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストが1つのブランチに保存されました。まとめてレビューし、pull request を作成します。星評価の pull request (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスの管理を任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントをいくつかの別々の PR に分けます。ワークショップを円滑に進めるため、フィルタリングと品質のワークフロー全体を1つの会話とブランチにまとめ、すべての作業をこの PR に含めています。

このレッスンでは、次の操作を行います。

- Agent Merge の概要と、マージライフサイクルの自動化方法を学ぶ。
- 機能の完全な変更と検証の証拠を確認する。
- フィルタリングの PR を作成する。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

フィルタリングワークフロー全体で、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージの承認を開発者の管理下に置きながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge の紹介

**Agent Merge** は、pull request を反映するために必要な残りの作業を自動化します。有効にすると、Copilot は PR のブロック要因を処理します。失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じたリベースを行い、リポジトリで許可されている場合は GitHub の自動マージを有効にします。

ここまで、マージは自分で **Merge pull request** を選択して行いました。Agent Merge にその責任を任せることもできます。Agent Merge を有効にする前に、作業をレビューして、準備が整ったと判断してください。

## Agent Merge を使って PR を管理する

すべてのコードが作成されたので、まとめてレビューし、PR を作成し、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントダイアログを開きます。
3. リストから **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーション処理を監視します。すべて成功すると、マージを実行します。
7. Copilot から「PR #14 was squash-merged successfully」のようなメッセージが表示されることを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、不足している権限を回避しません。ブロックされた場合は、報告された理由を読み、リポジトリが許可する場合はレビュー済みのマージを手動で完了してください。

## 概要と次のステップ

コードの生成、テストと検証、pull request プロセスなど、開発プロセスの複数の部分を自動化しました。

- Agent Merge の概要と、マージライフサイクルの自動化方法を学びました。
- 機能の完全な変更と検証の証拠を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウド委任に使える [Copilot CLI のスラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI を使った pull request の管理][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
