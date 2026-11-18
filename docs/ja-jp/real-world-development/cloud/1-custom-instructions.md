---
title: "演習 1 - カスタム指示 (クラウドエージェント)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot クラウドエージェント →][next-lesson]

生成 AI を利用するうえで、コンテキストは重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot は依頼された作業だけでなく、その進め方も理解できます。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルによって、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code および CLI のハーネスとは異なり、この演習ではプロンプトの「実行前と実行後」を比較しません。Copilot クラウドエージェントは GitHub issue を使って非同期に動作するため、リアルタイムで影響を並べて示すのは困難です。このハーネスの後半で、クラウドエージェントが作成した pull request をレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データレイヤーには、必ずユニットテストを用意します。
- UI はダークモードで、現代的な印象にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使うと、Copilot がこれらの開発手法に沿ってタスクを実行するための情報を提供できます。

### カスタム指示

カスタム指示を使うと、Copilot がコーディングスタイルや要件を理解するためのコンテキストと設定を指定できます。コーディング規約、ライブラリ、記述するコメントの種類などを指定でき、提案やコードスニペットを目的に合わせて調整できます。リポジトリ全体に適用する指示も、特定のファイルやタスクに適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。Copilot に送るチャットや CLI の多くのリクエストに関係する、プロジェクト全体の情報を記載します。たとえば、技術スタック、構築するものの概要、ベストプラクティス、全般的なガイダンスなどです。
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
> GitHub Copilot を使うプロジェクトでは、指示ファイルを充実させることが重要です。このプロジェクトの指示ファイルを確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスクに対応するファイルが用意されています。
>
> Copilot に指示ファイルを作成してもらうこともできます。方法は利用する環境によって異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など）。利用中の環境に対応するレッスンで詳しく説明します。
>
> テンプレートやひな形を探している場合は、指示ファイル、カスタムエージェント、その他のリソースをまとめたリポジトリ [awesome-copilot][awesome-copilot] を参照してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## プロジェクトのカスタム指示ファイルを確認する

少し時間を取って、このリポジトリに含まれる指示ファイルを読みます。基本となる `copilot-instructions.md` が 1 つと、さまざまなタスクに対応する `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスにも注目してください。これらの指示は Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、指示の適用先となるリポジトリルート相対の glob パターンを指定しています。ここでは、たとえば `**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトでユニットテストを作成するときに適用される指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` の末尾までスクロールします。`unit-tests.instructions.md` などの別の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。リンク先のパスは指示ファイルからの相対パスです。大きな指示を再利用可能な小さなファイルに分割し、コード生成の参考例を Copilot に示すことができます。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc のドキュメントコメントとファイルヘッダーコメントのルールを追加します。

## リポジトリ全体に適用する標準を追加する

ここで行う編集は、コード内のドキュメントを TSDoc のドキュメントコメントとファイル単位のヘッダーコメントで記述するルールの追加です。クラウドエージェントが、このハーネスの後半で割り当てる issue に取り組むときに、このルールを使います。

編集する前に、作業用のブランチを作成します（クラウドエージェントは issue が対象とするブランチから指示を読み取りますが、変更を整理してコミットするため、ここではブランチを作成します）。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前の説明のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。コード提案の品質を高めるため、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールはありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準のすぐ下に次の Markdown 行を追加し、ファイルヘッダーと TSDoc のドキュメントコメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベルの `.github/copilot-instructions.md` に作成して全体に適用することも、特定の言語、ファイルの種類、タスクに合わせた `*.instructions.md` ファイルとして作成することもできます。今回追加したドキュメントコメントのルールのような、プロジェクト全体に適用する標準にはリポジトリレベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

クラウドエージェントは、issue の対象ブランチにある指示ファイルを読み取ります。次の演習で Copilot に issue を割り当てると、Copilot は `main` からブランチを作成します。指示を適用するには、`main` に変更を反映する必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けた pull request を作成し、マージします。次の演習で issue を割り当てるとき、クラウドエージェントはこの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを作成せず、`main` に直接コミットしてもかまいません。実際のプロジェクトで行う方法に近づけるため、ここではブランチを使っています。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを得る方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。このハーネスの残りでクラウドエージェントが作成する pull request をレビューするときに、この標準が適用されているかを確認できます。

次は [Copilot クラウドエージェントに最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使用する指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
