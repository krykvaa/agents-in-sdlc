---
title: "レッスン 2 - 星評価の追加"
description: "Copilot CLI を使ってゲームカードに星評価を表示し、変更をレビューして最初の pull request をマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したら、次はプロジェクトを小さく変更します。ゲームにはすでに星評価のデータがありますが、ホームページのゲームカードには表示されていません。エージェントに表示を依頼し、変更をレビューして、最初の pull request としてマージします。

このレッスンでは、次の操作を行います。

- 機能ブランチで対象を絞った Copilot の会話を開始する。
- エージェントにプロジェクトの小さな変更を依頼する。
- `/diff` を使って変更を確認する。
- 転送されたブラウザーでアプリを実行し、変更を確認する。
- 最初の pull request を作成してマージする。

## シナリオ

各ゲームには星評価があり、ゲーム詳細ページでは表示されています。ホームページのゲームカードにはタイトル、カテゴリー、パブリッシャー、説明のみが表示されています。この準備として、既存の評価を各カードに表示します。

## 会話を開始して変更を依頼する

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. 次のコマンドで Copilot を開始します。

   ```bash
   copilot --yolo
   ```

4. `/new` を入力し、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトを入力します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲームカードと詳細を表示するファイルを特定して変更を作成します。

## 差分をレビューする

小さな変更でも、AI が生成した変更はマージ前にレビューする必要があります。

1. `/diff` を入力し、変更された各ファイルを確認します。
2. 評価がある場合は数値が表示され、`starRating` が `null` の場合は `No rating yet` が表示されることを確認します。
3. 両方の状態をテストで確認します。
4. チェックの失敗があれば Copilot に修正を依頼します。
5. レビューが終わったら、<kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> Copilot を含む生成 AI は確率的なため、生成されるコードは完全には一致しないことがあります。単一の実装ではなく、期待する動作を確認してください。

## ブラウザーで変更を確認する

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. ポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. カードに評価が表示されることを確認します。
4. Copilot にサーバーの停止を依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## pull request を作成してマージする

1. 次のプロンプトで変更をコミットします。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は既存のコミットを push して PR を作成します。
3. 表示された PR の URL を確認して開きます。
4. **Merge pull request** を選択し、マージを確定します。
5. `/exit` を入力して Copilot CLI を終了します。
6. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## 概要と次のステップ

このレッスンでは、会話の開始、差分レビュー、ブラウザー確認、pull request のマージを行いました。次は、[フィルター Issue を使い、Plan モードと Autopilot モードでより大きな機能を構築][next-lesson]します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
