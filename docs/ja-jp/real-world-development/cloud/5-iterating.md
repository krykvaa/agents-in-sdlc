---
title: "演習 5: GitHub Copilot の作業を反復改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスの改善や機能追加を目的とする複数のタスクで GitHub Copilot と作業してきました。コードへのドキュメント追加、デザインチームが試せる関連ゲーム機能の作成、ハイコントラストとライトモードを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善しましょう。

### シナリオ

何度も説明してきたように、生成 AI が加わっても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードは必ずレビューし、通常の DevOps プロセスに沿って作業します。チーム全体にレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認しましょう。

## GitHub Copilot クラウドエージェントのセキュリティ

Copilot クラウドエージェントは、監視されていない状態で非同期にタスクを実行します。安全性を確保するため、次のような制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みできるのはコード作業用のブランチに限られます。
- クラウドエージェントは GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions のワークフローを実行するには、人間による承認が必要です。
- [外部リソースへのアクセスは、デフォルトで制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

GitHub Copilot クラウドエージェントが作成した最初の pull request (PR)、つまりコードへのドキュメント追加から確認しましょう。GitHub.com の標準的な PR インターフェイスを使ってレビューします。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールでブロックされているという警告が表示されることがあります。Copilot は、外部 MCP サーバーへの呼び出しなど、外部リソースへのアクセスがデフォルトで制限されているため、これは**想定どおり**です。必要であれば、[Copilot クラウドエージェントのファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、pull request の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いた pull request を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、pull request のタイトルに **[WIP]** が含まれます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. pull request の準備ができたら、**Files changed** タブを選択して変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメントコメントやその他のドキュメントを確認します。実際の変更内容は状況によって異なります。

    変更を確認するときは、TSDoc のコメント、TypeScript の規約、ファイルヘッダーに注目してください。これらは演習 2 の最初に確認したカスタム指示ファイルで指定した内容です。

6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. pull request のチェックセクションにワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

pull request で Copilot と作業することは、一方通行ではありません。チームの他のメンバーと同じように、pull request のコメントやコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確定的ではないため、必ず依頼する文面を指定することはできません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、役割を簡単に説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 生成されたドキュメントへの変更を依頼するコメントを追加します。他のユーザーと同じように **@copilot** をメンションします。上記の例を使うか、コードベースに必要なドキュメントについて別の変更を提案します。
2. **View Session** を選択して、Copilot が作業する様子を確認します。更新を行うため、新しいセッションが開始されます。
3. **Back to pull request** を選択すると pull request に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、pull request に新しいコミットが追加されます。
5. **Files changed** タブを選択して変更をレビューします。

満足するまで改善を繰り返してください。問題がなければ、ドラフト PR をレビュー可能な状態に変更して `main` ブランチにマージできます。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲームの詳細ページに関連ゲームを表示する issue に対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容を確認して問題がなければ、**Conversation** タブに戻って下にスクロールします。
6. ワークフローが承認待ちになっていることを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. pull request のチェックセクションにワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **オプション:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。codespace に移動し、ターミナルを開いて、次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名に置き換えます。例: **copilot/fix-8**）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

関連ゲーム機能が完成しました。前の PR と同様に、Copilot クラウドエージェントに変更を依頼して反復的に改善できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーや TSDoc のコメントを追加したりできます（このタスクはカスタム指示を追加する前に割り当てたことを思い出してください）。前と同じように **Conversation** タブにコメントを追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントを使って実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 の mission control から追加を依頼したライトモードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、pull request のタイトルに **[WIP]** が含まれます。Copilot が作業を完了するまで待ってください。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保存するためのローカルストレージの使用
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. pull request のチェックセクションにワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **オプション:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。codespace に移動し、ターミナルを開いて、次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードの切り替えをテストします。ページを再読み込みしても設定が保持されることも確認してください。

アクセシビリティカスタムエージェントが、アクセシビリティのベストプラクティスに従って Copilot の実装をガイドしたことがわかります。アクセシビリティ上の問題や改善点があれば、前の PR と同じようにコメントで **@copilot** に更新を依頼できます。

## オプションの演習 — 作業を続けて委任する

クラウドエージェントに実際のバックログ項目を任せると、その間に別の作業に取り組めます。この方法を身につけるため、リポジトリにいくつか issue を追加して Copilot に割り当ててみましょう。次のような作業が考えられます。

- ゲームの詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

クラウドエージェントのハーネスを完了しました。これらのレッスンでは、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認**し、クラウドエージェントの出力に反映されることを確認しました。
- **issue を Copilot クラウドエージェントに割り当て**、環境の準備、計画、非同期の作業を確認しました。
- **カスタムエージェントを作成して使用**し、ハイコントラストモードとライトモードを追加しました。
- **Copilot エージェントページをコントロールセンターとして使用**し、作業中のアクセシビリティセッションを監視して誘導しました。
- **クラウドエージェントの pull request をレビューして改善**し、`@copilot` に変更を依頼してワークフローを承認しました。

## レビューと次のステップ

クラウドエージェントのハーネスを完了しました。さらに学びたい場合は、次のハーネスで今回の内容を発展させることができます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat のエージェントモードと MCP の統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェントスキル、カスタム QA エージェント、Playwright MCP、Agent Merge、オプションの `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つ届けます。`/delegate` はクラウドエージェントへの連携機能です。

自分のリポジトリで、次のようなアイデアを試してみましょう。

- リファクタリングやテストカバレッジの issue をクラウドエージェントに割り当てる。
- 別の分野に特化したカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot クラウドエージェントのファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
