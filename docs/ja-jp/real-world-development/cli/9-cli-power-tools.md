---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト、モデル、セッション共有、任意のクラウド委任を管理するスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

優れた CLIツールと同様に、GitHub Copilot CLIには操作に使うスラッシュコマンドが多数用意されています。これらのコマンドを使うと、高度な機能、内部情報、追加の構成オプションを利用できます。すでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` を使用しました。ここでは、さらに役立つコマンドを確認します。

このレッスンでは、次の操作を行います。

- `/context` と `/compact` を使って、Copilot が会話コンテキストを管理する方法を確認する。
- `/model` を使って利用可能なモデルを確認する。
- `/share` を使ったセッションのエクスポートまたは共有方法を学ぶ。
- 並列作業、worktree、クラウドエージェントへの委任の任意コマンドを確認する。

## シナリオ

CLI の中心的なワークフローを完了しました。ここでは、コンテキストの管理、モデルの切り替え、セッションの共有、作業の [Copilot cloud agent][about-cloud-agent] への任意の委任という追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキストウィンドウに達する場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮し、スラッシュコマンドで確認または手動圧縮できます。

1. Codespace に戻り、Copilot CLI をリポジトリのルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域へのコンテキストの配分を確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. 再び `/context` を入力し、結果を比較します。会話が小規模な場合は大きな変化はありません。

> [!NOTE]
> コンテキストウィンドウが満杯になると、Copilot CLI は自動的にコンテキストを圧縮します。タイミングを選ぶ場合は `/compact` を使用します。無関係なタスクに切り替えて新しい会話を開始する場合は、`/clear` または `/new` を使用します。

## モデルを選択する

モデルごとに得意分野が異なり、開発者の好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示して選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学びを共有することで、AIツールの利用方法を改善できます。`/share` コマンドは、セッションを Markdown または HTML ファイルにエクスポートするか、共有可能なリンクや GitHub gist を作成できます。

1. `/help` を入力し、インストールされたバージョンで利用できる `/share` オプションを確認します。
2. このセッションを共有する場合は、ニーズに合う保存先を選択します。たとえば、ローカルの Markdown エクスポートには `/share file` を使用します。
3. 誰かに送信または公開する前に、エクスポートされた内容を確認します。セッションのエクスポートには、プロンプト、回答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 任意: スケールアウトまたは委任

ワークショップの中心的な内容は完了しました。Copilot CLI には、より大規模なタスク向けのコマンドもあります。

- `/fleet`: 独立したサブタスクをサブエージェントに分割し、並列実行できます。
- `/worktree`: 別タスク用に分離された Git worktree を作成できます。
- `/delegate`: タスクを Copilot cloud agent に送信できます。cloud agent は非同期で作業し、pull request を作成する場合があります。

これらのコマンドは、追加の worktreeやリモート作業を作成する可能性があるため任意です。試す前に、新しく対象を絞ったタスクを開始し、通常のワークフローで結果をレビューしてください。非同期エージェント作業について詳しく知る場合は、[cloud agent ワークショップ][cloud-workshop]を続けてください。

## 概要と次のステップ

Copilot CLI のスラッシュコマンドを使うと、構成、セッション共有、内部で進行している処理を確認できます。このレッスンでは、次の操作を行いました。

- `/context` と `/compact` を使って、Copilot が会話コンテキストを管理する方法を確認しました。
- `/model` を使って利用可能なモデルを確認しました。
- `/share` を使ったセッションのエクスポートまたは共有方法を学びました。
- 並列作業、worktree、クラウドエージェントへの委任の任意コマンドを確認しました。

Copilot CLI にはさらに多くのスラッシュコマンドがあります。[学習内容を振り返り][next-lesson]、継続して学ぶための次のステップを確認しましょう。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agentについて][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
