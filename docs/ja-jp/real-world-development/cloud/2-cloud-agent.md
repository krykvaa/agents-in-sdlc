---
title: "演習 2 - GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがその例です。GitHub Copilot のクラウドエージェントは、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト pull request (PR) を作成します。面倒な作業を任せて開発を加速し、開発者がより大きな課題に集中できるようになります。

このレッスンでは、Copilot クラウドエージェントを使って次のことを学びます。

- コード生成の環境をカスタマイズする方法
- 操作を安全に行う方法
- issue のスコープを明確にする重要性
- Copilot に issue を割り当てる方法

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初のサイトを作成した委託先は、ドキュメントを十分に整備していませんでした。まず、アプリケーションのすべてのエクスポート関数に TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリの他のゲームを表示して、利用者がゲームを探し続けられるようにします。まだ仕上げる必要はありませんが、ユーザー受け入れテストで UX を試せる状態にしたいと考えています。優先度の高い他の issue があるため、この作業は後回しになっています。

どちらも、優先度が下がりやすいタスクです。Copilot クラウドエージェントに割り当てれば、非同期に作業を進めてもらい、開発者は別のタスクに取り組めます。後から Copilot の作業をレビューし、期待どおりか確認できます。

## GitHub Copilot クラウドエージェントについて

[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)は、人間の開発者と同じようにバックグラウンドで作業できます。[GitHub issue を Copilot に割り当てる][assign-issue]など、いくつかの方法で作業を開始できます。割り当てると Copilot は進捗を追跡するドラフト pull request を作成し、環境を準備してタスクに取りかかります。作業中も完了後もセッションを確認できます。提案された解決策のレビュー準備ができると、pull request で通知されます。

## スコープを適切に定めた指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな操作でも、詳しく調べると複雑なことがあります。[Copilot クラウドエージェントのタスクのスコープ][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階的に進めながら学び、試し、必要に応じて調整しましょう。生成 AI を利用しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示とその役割を紹介しました。カスタム指示の演習をすでに完了していれば、`.github/copilot-instructions.md` が機能することを確認しています。まだの場合は、少し時間を取って確認しましょう。Copilot クラウドエージェントに作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認しておくと、後で指示の影響を見つけやすくなります。

リポジトリの GitHub Web UI、または起動済みの codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターで、指示の適用先が `**/*.test.ts` に指定されていることを確認します。

次のセクションでクラウドエージェントに `Code lacks documentation` issue を割り当てるとき、作成された pull request に TSDoc のコメント、TypeScript の規約、ファイルヘッダーが反映されているかを確認してください。後のレッスンでも、これらの指示を振り返ります。

## Copilot クラウドエージェントの開発環境を設定する

誰がコードを書く場合でも、通常は環境の準備とセットアップスクリプトの実行が必要です。タスクを Copilot に割り当てる場合も同様です。クラウドエージェントは SWE と同じように作業するためです。

クラウドエージェントは、作業時の環境として [GitHub Actions][github-actions] を使用します。作業を開始する前に実行する[セットアップワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成すると、環境をカスタマイズできます。必要な開発ツールや依存関係を使用できるようにするための仕組みです。このラボの進行を簡単にし、学習に集中できるよう、あらかじめ設定されています。単一の Astro アプリのために、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションおよびシード済みの SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` 権限が必要です。手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを利用できるように、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じですが、次の点に注目してください。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは、Copilot が pull request の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使用します。これにより、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するのを待たずに、正常に実行されるかテストできます。

## ドキュメントを追加する

ドキュメントが大切だと誰もが理解していても、多くのプロジェクトでは情報が古かったり、まったく存在しなかったりします。こうした技術的負債が放置されると、生産性が下がり、コードベースの保守や新しいメンバーの受け入れが難しくなります。Copilot はドキュメント作成が得意なため、これはクラウドエージェントに任せるのに適した issue です。バックグラウンドで必要なドキュメントを作成してもらい、後の演習でその成果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue ダイアログを開きます。
4. **Blank issue** を選択して issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    関数やメソッドの動作を説明するうえで有用な場合は、TSDoc のドキュメントコメントを含めることが組織の要件です。しかし、最近の更新はこの標準に従っていません。動作を明確にする箇所にドキュメントコメントが含まれるよう、既存のコードを更新する必要があります。
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成された pull request (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。リストに表示されない場合は、少し待ってからブラウザーのウィンドウを更新してください。
12. 数分後、Copilot が todo リストを作成します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで、数分かかることがあります。Copilot は環境を準備し（前述のワークフローを実行します）、プロジェクトを分析して、問題に取り組む方法を検討しています。

13. リストを確認し、実行予定のタスクを確認します。
14. pull request のタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するには、ウィンドウを更新する必要がある場合があります。

16. ライブセッションをスクロールして、Copilot が問題を解決する様子を確認します。コードを調べて状態を理解し、考えながら適切な計画を決め、コードを作成します。

作業には数分かかることがあります。Copilot クラウドエージェントの主な目的は、作業を非同期に進め、開発者が他のタスクに集中できるようにすることです。別の作業に取り組みながらこの利点を活用するため、もう 1 つのタスクをクラウドエージェントに割り当て、その後、アプリケーションに機能を追加します。

## ゲームの詳細ページに関連ゲームを追加する

GitHub Copilot クラウドエージェントの利点の 1 つは、作業を分けられることです。デザインチーム向けの関連ゲームの表示が短時間でできるとしても、その時間を別の作業に使えます。クラウドエージェントに割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    ゲームの詳細ページに関連ゲームを表示して、利用者がほかのゲームを見つけられるようにします。デザインチームは UX を検討し、受け入れテストを行いたいと考えています。要件は次のとおりです。

   - `src/lib/` にデータアクセスヘルパーを追加し、指定したゲームを除外して、同じカテゴリに属する他のゲームを返すようにします。
   - ヘルパーを使って、ゲームの詳細ページに「関連ゲーム」セクションを表示します。
   - 関連ゲームがない場合にも対応します。
   - 新しいヘルパーのユニットテストを作成します。
   - PR を作成する前に、すべてのテストが成功することを確認します。
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を開始したことを示す目の絵文字 👀 が issue の最初のコメントに表示されます。

![issue で Copilot が目の絵文字を使って作業中であることを示す](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot クラウドエージェントは SWE と同様に作業するため、常に監視する必要はありません。完了後にレビューできます。次はカスタムエージェントを作成して使用します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot クラウドエージェント][copilot-agents]について学びました。クラウドエージェントに issue を割り当て、非同期に作業を進めてもらえます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などに利用できます。

次の内容を学びました。

- コード生成の環境をカスタマイズする方法
- 操作を安全に行う方法
- issue のスコープを明確にする重要性
- Copilot に issue を割り当てる方法

クラウドエージェントがバックグラウンドで作業する間、次はカスタムエージェントの作成と使用に取り組みます。[クラウドエージェントは MCP サーバー][cloud-agent-mcp]とカスタム指示も利用できます。これらは前の演習で扱いました。

## リソース

- [Copilot クラウドエージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
