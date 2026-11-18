---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせて、専用の AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、エージェントが効率よく作業できるように、対象を絞った指示、ベストプラクティス、コーディングパターン、分野固有の知識を指定します。チームの専門知識を再利用できるエージェントにまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディング手法に従うセキュリティエージェント、一貫したテストパターンを適用するテストエージェントなどです。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割が重なる部分があります。どちらも主に Markdown ファイルで定義され、AI が作業を行う方法を指定します。違いを簡潔に表すと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルや他のエージェントを使って作業を調整します。このラボのアクセシビリティカスタムエージェントは、アクセシビリティガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、pull request ワークフローのスキルや、テストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI の他の機能と同様に、利用する環境やシナリオに合わせてテストし、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

このレッスンでは、カスタムエージェントを使って次のことを学びます。

- カスタムエージェントの定義方法
- カスタムエージェントへのタスクの割り当て方

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての人がクラウドファンディングプラットフォームを使えるように取り組んでいます。最近のユーザーフィードバックでは、現在のダークテーマはテキストと背景色のコントラストが不十分で、読みづらいと指摘されました。このアクセシビリティの問題に対応するため、デザインチームは利用者が切り替えられるハイコントラストモードを求めています。

アクセシビリティは重要なため、できるだけ早く実装します。カスタムエージェントを使って機能を生成します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ向けのカスタムエージェントはあらかじめ作成されています。内容を確認して、Copilot をどのようにガイドするかを理解しましょう。

codespace に戻り、アクセシビリティカスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` を含むフロントマターは、カスタムエージェントに必須です。

3. 続くセクションを確認します。次の内容が記載されています。
   - アクセシブルな Web サイトのコード生成に関する主な責任
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題と避けるべきミス

## issue を作成して割り当てる

Mission control は、環境内のすべてのエージェントを管理する場所です。Copilot クラウドエージェントにタスクを割り当てたり、進捗を監視したり、作業中のセッションに追加の指示を送ったりできます。まず、ハイコントラストモードの作成を Copilot に割り当てましょう。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Web サイトにハイコントラストモードが必要です。利用者が設定できる切り替え機能を用意し、設定をブラウザーのローカルストレージに保存してください。
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![クラウドエージェントの割り当て画面で、カスタムエージェントと Accessibility が選択されている](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた AI アシスタントである [GitHub Copilot のカスタムエージェント][custom-agents]について学びました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot が適切な方法で作業できるようにガイドできます。

次の内容を学びました。

- カスタムエージェントの定義方法
- カスタムエージェントへのタスクの割り当て方

Copilot がハイコントラストモードの実装に取り組んでいる間に、[mission control からエージェントセッションを監視して誘導][next-lesson]しましょう。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを利用する準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを利用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot クラウドエージェント][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
