---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot アプリを使って Pages ワークフローを構成し、pull request をレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages でデプロイして、Space Quiz を一般公開しましょう。Copilot はワークフローの準備とデプロイの案内を行いますが、変更をレビューし、サイトが公開されたことを確認するのはあなたの役割です。

このレッスンでは、次の作業を行います。

- Copilot に GitHub Pages のデプロイを構成するよう依頼します。
- マージする前に、ワークフローと pull request をレビューします。
- デプロイの進行を確認し、公開されたサイトを検証します。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、まだ簡単に試せる場所がありません。単一ファイルのサイトを GitHub Pages で公開すれば、サーバーやランタイムの依存関係を追加せずに、公開 URL から利用できるようになります。

## Copilot にデプロイを準備してもらう

> [!WARNING]
> pull request の作成によって、コードがコミットおよびプッシュされる場合があります。このプロンプトを送る前に、承認設定と送信先のリポジトリを確認してください。**すべて承認**に設定されている場合、個別の確認は表示されません。

`space-quiz` の Copilot アプリセッションで、次のプロンプトを送信します。

```plaintext
このリポジトリで GitHub Pages を使ったデプロイを準備してください。必要に応じて Web サイトをビルドし、変更がデフォルトブランチに反映された後に GitHub Pages へデプロイする GitHub Actions ワークフローを作成してください。README が存在しない場合は、アプリの説明とローカルでの実行方法を記載した README を追加してください。これらの変更で pull request を作成し、そのリンクと必要な Pages の設定や権限を報告してください。マージ、自動マージの有効化、Pages 設定の変更、デプロイは行わないでください。レビューを待って停止してください。
```

## ワークフローと pull request をレビューする

1. README は、リポジトリにまだ存在しない場合に限って追加されていることを確認します。
2. ビルドとデプロイの手順、トリガー、権限を含め、pull request の変更をレビューします。
3. ワークフローが、未マージの pull request ではなくデフォルトブランチからデプロイすることを確認します。
4. 修正が必要な場合は依頼し、更新された差分をレビューします。マージを承認する前に、必要な pull request のチェックが成功するまで待ちます。

## マージとデプロイを承認する

> [!WARNING]
> 次のプロンプトを送ると、レビュー済み pull request のマージ、リポジトリの Pages 設定の変更、サイトの公開をエージェントに許可します。レビューを完了し、リポジトリと pull request を確認してから送信してください。承認設定によっては、これらの操作に追加の確認が求められない場合があります。

`<PR_NUMBER>` をレビューした pull request の番号に置き換えてから、別のプロンプトとして送信します。

```plaintext
このリポジトリの pull request <PR_NUMBER> をレビューし、マージとデプロイを承認しました。GitHub Actions を使って GitHub Pages を有効にし、必要なチェックが成功してから該当の pull request のみをマージしてください。デプロイが成功するまで状況を確認し、公開された Web サイトの最終 URL を報告してください。Pages の有効化やマージに必要な権限がない場合は、承認または実行が必要な操作を具体的に教えてください。サイトが公開されるまでは成功と報告しないでください。
```

![3 段階の図: マージ済みの space-quiz pull request、Build website、Upload artifact、Deploy to Pages の各ステップが成功した GitHub Actions の Deploy website 実行、yourname.github.io/space-quiz/ で公開された Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを確認する

1. GitHub Actions のデプロイ実行が成功するまで確認します。
2. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントが Pages の有効化や pull request のマージを実行できない場合は、案内された手順に従ってください。デプロイが成功し、公開サイトが読み込まれるまでは完了と見なさないでください。

## まとめと次のステップ

Space Quiz が公開され、エージェントの報告を信頼するだけでなく、自分でデプロイを確認できました。続けて[レッスン 13: レビューと次のステップ][next-lesson]に進みましょう。

[next-lesson]: ../13-review/
