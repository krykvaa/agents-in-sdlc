---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan でアプローチに合意し、Autopilot で Issue からフィルター機能を構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

小さな機能を追加したところから、より大きな変更では堅牢なプロセスが必要です。GitHub Copilot CLI を使い、Issue に基づいて計画、実装、検証、PR の作成を進めます。

> [!NOTE]
> このレッスンでは、同じ会話とブランチを使って機能ワークフローを進めます。通常はファイルの種類ごとに別のブランチや PR を使いますが、ここでは中心となる概念に集中します。

このレッスンでは、次の操作を行います。

- Issue を使って新しい Copilot の会話を開始する。
- **Plan** モードで要件を定義する。
- **Autopilot** モードで新機能を実装する。
- コードをレビューする。
- 転送されたブラウザーで機能を手動検証する。

## シナリオ

Tailspin Toys のカタログには、カテゴリーとパブリッシャーでゲームを絞り込む機能が必要です。バックログの Issue に機能は記載されていますが、フィルターの組み合わせ方などの決定事項はコーディング前に合意します。

## 会話モード

**Interactive** モードでは、エージェントが提案し、続行前に入力を求めます。**Plan** モードでは計画を作成してファイルは編集しません。**Autopilot** モードでは、コードの作成、テスト、反復をエージェントに任せます。

## Issue から開始する

1. Codespace に戻り、リポジトリとターミナルを確認します。
2. 星評価の PR がマージされていることを確認します。
3. `main` を更新し、フィルター用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

4. Copilot CLI を開始します。

   ```bash
   copilot --yolo
   ```

5. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
6. <kbd>A</kbd> を押してすべての Issue を表示します。
7. 矢印キーで **Allow users to filter games by category and publisher** を選択します。
8. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

Issue の参照には `#7` のような番号が付きます。`#` を使うと、GitHub の Issue または pull request を会話のコンテキストに追加できます。

## フィルター機能を計画する

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。
2. 次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

3. Copilot の質問には、フィルターの動作について自分たちの決定を回答します。
4. 計画が完了したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開し、内容を確認します。
5. 決定事項と一致しない部分は変更を依頼します。

## Autopilot を承認する

計画を確認したら、**Accept plan and build on autopilot (recommended)** を選択します。モード表示が **Autopilot** になり、Copilot が計画に沿ってコードを生成し、テストを実行します。

> [!NOTE]
> 承認すると実装を開始する場合があります。依存関係の不足やポート競合が報告されたら、チェックを完了として扱う前に解決してください。

## 実装をレビューして検証する

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに戻ります。
2. `/diff` を入力し、フィルター実装とテストを確認します。
3. Issue と計画の決定事項と照合します。
4. <kbd>Esc</kbd> を押して差分表示を終了します。
5. チェックの出力を確認し、失敗があれば Copilot に修正を依頼します。

## 新しい機能を確認する

次のプロンプトでアプリを起動します。

```plaintext
Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
```

ポート `4321` が利用可能になったら、**Open in Browser** を選択します。カテゴリー、パブリッシャー、組み合わせフィルターとリセット動作を確認します。

## 概要と次のステップ

このレッスンでは、会話モードを使って機能を構築し、レビューしました。次は、[カスタム指示][next-lesson]を使ってコード生成のルールを定義します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
