---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を利用するうえで、コンテキストは重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。その中でも重要なのが、コード生成の指針を提供する[指示ファイル][instruction-files]です。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルによって、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- 現在の指示を使って、フィルター機能の最初のデータ部分（出版社ヘルパー）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトをもう一度実行し、生成されたコードが新しい標準に従うかを確認します。
- 次の演習で Copilot が更新されたガイダンスを使えるように、指示の変更とフィルター機能の一部を `main` にコミットします。

> [!CAUTION]
> 生成されたコードが、設定した標準の一部に従わないことがあります。Copilot は確率的に動作します。この演習では、出力の文字列を完全に一致させるのではなく、指示を更新した後の動作の**傾向**を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データレイヤーには、必ずユニットテストを用意します。
- UI はダークモードで、現代的な印象にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使うと、Copilot がこれらの開発手法に沿ってタスクを実行するための情報を提供できます。

### カスタム指示

カスタム指示を使うと、Copilot がコーディングスタイルや要件を理解するためのコンテキストと設定を指定できます。コーディング規約、ライブラリ、記述するコメントの種類などを指定でき、提案やコードスニペットを目的に合わせて調整できます。リポジトリ全体に適用する指示も、ファイルの種類やタスクに応じた指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、構築するものの概要、ベストプラクティスなど、チャットや CLI の多くのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類に使用できます。TypeScript や Astro などの言語、UI コンポーネントやユニットテストの作成などのタスクに合わせてガイドラインを指定できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot クラウドエージェントは、リポジトリレベルの指示ファイルと、`applyTo` フロントマターで対象が指定された `*.instructions.md` ファイルを使用します。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、別の標準形式の指示ファイルにも対応しています。[対応する指示ファイル形式][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することは、このワークショップの範囲外です。サンプルプロジェクトには、一般的な使い方を示す例が含まれています。基本的には、次の点を意識してください。

- `copilot-instructions.md` には、構築するものやプロジェクトの構造、全体的なコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルは、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル形式や、特定のタスクに関する指示に使います。
- 自然な言葉で、明確なガイダンスと例を記載します。良いコード例と避けるべきコード例を示してください。

指示ファイルの作成や AI の使い方に、唯一の正解はありません。プロジェクトで試しながら、最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、指示ファイルを充実させることが重要です。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスクに対応する指示ファイルがあります。
>
> Copilot に指示ファイルを作成してもらうこともできます。方法は利用する環境によって異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など）。利用中の環境に対応するレッスンで詳しく説明します。
>
> テンプレートやひな形を探している場合は、指示ファイル、カスタムエージェント、その他のリソースをまとめたリポジトリ [awesome-copilot][awesome-copilot] を参照してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## プロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを読みます。基本となる `copilot-instructions.md` が 1 つと、さまざまなタスクに対応する `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスにも注目してください。これらの指示は Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用先となるリポジトリルート相対の glob パターンを指定しています。たとえば、`**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. ユニットテストの作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。リンク先のパスは指示ファイルからの相対パスです。このように大きな指示を再利用可能な小さなファイルに分け、コード生成の参考例を指定できます。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc のドキュメントコメントとファイルヘッダーコメントのルールを追加します。

## 変更用ブランチを作成する

ベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

カスタム指示の影響を確認するため、まずは現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新して同じプロンプトをもう一度送信し、違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に伝えます。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択して、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   `src/lib/publishers.ts` を作成または更新し、すべての出版社について名前と ID の一覧を返すデータアクセスヘルパーを追加してください。ファイルの変更を適用してください。
   ```

5. Copilot はプロジェクトを調べてコードを更新します。多くの場合、ヘルパーファイルとテストの両方が変更されます。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取り、出版社の型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードに TSDoc のドキュメントコメントとファイル単位のコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントが追加される場合があります。その場合も問題ありません。指示を更新した後に動作の**一貫性**が高まることが、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。コード提案の品質を高めるため、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールはありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準のすぐ下に次の Markdown 行を追加し、ファイルヘッダーと TSDoc のドキュメントコメントを追加するよう Copilot に指示します。

   ```markdown
   - エクスポートされたすべての関数に、目的、パラメーター、戻り値を説明する TSDoc コメントを記述します。
   - import 文やコードの前に、ファイルの目的を説明するコメントブロックを追加します。
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベルの `.github/copilot-instructions.md` に作成して全体に適用することも、特定の言語、ファイルの種類、タスクに合わせた `*.instructions.md` ファイルとして作成することもできます。今回追加したドキュメントコメントのルールのような、プロジェクト全体に適用する標準にはリポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をリセットします。
2. `src/lib/publishers.ts` を選択して、Copilot の対象ファイルを指定します。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   `src/lib/publishers.ts` を作成または更新し、すべての出版社について名前と ID の一覧を返すデータアクセスヘルパーを追加してください。ファイルの変更を適用してください。
   ```

4. 今回は、提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディングプラットフォーム用の出版社データアクセスヘルパーです。
    * データベースから出版社情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * すべての出版社の ID と名前の一覧を返します。
    *
    * @param db - Drizzle データベースクライアントです。
    * @returns 出版社オブジェクトの配列に解決される Promise です。
    */
   ```

新しいプロジェクト標準に従うよう Copilot をガイドし、次の演習で使うコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルも他のリポジトリ内のファイルと同じように、ソース管理で管理します。変更をコミットしてリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. 次のコマンドを実行して、指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の変更と、Copilot がヘルパーの基盤として変更したすべてのファイルをステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連ファイル（テストなど）が表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の issue に関連付けて pull request を作成します。手動で行う代わりに、すでにプロジェクトに接続されている GitHub ツールを使って Copilot に作成してもらいましょう。issue を見つけ、issue をクローズする pull request を作成し、マージするよう依頼します。

> [!NOTE]
> この演習では、Copilot が初めて GitHub 上の操作を行います。プロジェクトテンプレートでは GitHub のツールがあらかじめワークスペースに接続されているため、設定は不要です。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインが求められる場合があります。画面の指示に従ってください。次の演習では、この接続に使われる Model Context Protocol について説明します。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントの選択で **Agent** を選びます。
3. 指示ファイル更新の issue を探し、現在のブランチから PR を作成して、指示の更新と出版社ヘルパーの基盤の両方を説明するよう Copilot に依頼します。

   ```
   指示ファイルの更新に関する issue を探してください。現在のブランチから PR を作成し、その PR で該当の issue がクローズされることを明記してください。また、今後のフィルタリング作業に向けて出版社ヘルパーの基盤も追加したことを記載してください。
   ```

4. Copilot が issue を探して PR を作成します。
5. Copilot に GitHub 操作を許可するよう求められたら、コマンドを確認して、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトで PR をマージし、ローカルのブランチを `main` に戻すよう Copilot に依頼します。

   ```
   PR を main にマージしてください。その後、ローカルで main に戻り、最新のコードを pull して最新の状態にしてください。
   ```

7. Copilot に GitHub 操作やシェルコマンドの実行を許可するよう求められたら、コマンドを確認して、適切な場合に **Allow** を選択します。

これで、GitHub Copilot を使って pull request を作成し、マージできました。

## まとめと次のステップ

このリポジトリの指示ファイルから Copilot がコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- 既存の指示を使ってコード生成のプロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトをもう一度実行して、提案されたコードが新しい標準に従うことを確認しました。
- 次の演習の土台となる指示の変更とヘルパーを `main` にコミットしました。

次は[エージェントモード][next-lesson]で、Copilot にコードベース全体にわたる新機能を実装してもらい、指示を活用します。

## リソース

- [GitHub Copilot のカスタマイズに使用する指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
