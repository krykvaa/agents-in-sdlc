---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードを使って、フィルタリング機能を作成しました。pull request を開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の作業を行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーでフィルタリング機能を手動テストするよう依頼します。
- ブランチを公開して、フィルタリング作業の pull request を作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にアクセスできると、さらに高機能になります。Copilot がそのために使用するのが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスとやりとりするための標準的な方法です。

![エージェントモードの内部構造と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)を使うと、AI エージェントが外部のツールやサービスと通信できます。MCP によって、エージェントはリソースを使って最新情報にアクセスしたり、ツールを使って代理で操作したりできます。

これらのツールとリソースには、AI エージェントと外部のツールやサービスの橋渡しをする MCP サーバーを通じてアクセスします。MCP サーバーは、既存の API や npm パッケージのようなローカルツールなど、外部ツールとの通信を管理します。各 MCP サーバーによって、AI エージェントが利用できるツールとリソースが異なります。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。新しいリポジトリの作成、既存リポジトリの更新、issue や pull request の管理などを AI エージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などを AI エージェントが実行できます。

他にも、さまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけたり、提供に参加したりできる [MCP レジストリ](https://github.com/mcp)を公開しています。

> [!CAUTION]
> セキュリティの面では、MCP サーバーも他の依存関係と同様に扱ってください。使用前にソースコードを確認し、提供元を検証して、セキュリティ上の影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可するときは注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. codespace で `.vscode/mcp.json` を開きます。
2. 次のような `github` サーバーがすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトのテンプレートに含まれているため、前の演習では Copilot がバックログの issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うので、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供するため、機能のテストに適しています。

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートサーバーの `github` と異なり、`playwright` は**ローカル**サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、Playwright がブラウザーを表示せずに実行されます。codespace には表示用のデスクトップ環境がないため、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンドテストに Playwright をすでに使用しているため、通常は Playwright 用のブラウザーもインストール済みです。後で Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot がサーバーのツールを初めて必要としたときに VS Code が起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. サーバーを信頼するか確認された場合は、構成を確認して、サーバーを起動できるように信頼することを選択します。

手動で起動する必要はありません。ここで起動しない場合は、次の手順で Copilot がツールを初めて使うときに、VS Code がサーバーを起動して信頼の確認を表示します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作しなくても、エージェントがページを開き、移動し、フィルターを適用して、その結果を確認して要約します。会話を続けながら機能が期待どおりに動作するかを確かめる、すばやい方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使って動作します。つまり、エージェントは支援技術と同じように、ボタン、リンク、リスト項目などのラベル付きの構造化された要素を調べます。そのため、機能を簡単に確認すると同時に、アクセシビリティも簡易的に確認できます。

サーバーを接続してアプリを実行した状態で、作成したフィルタリング機能をテストするよう Copilot に依頼します。

```text
Playwright MCP サーバーを使い、実行中のアプリ (http://localhost:4321) をブラウザーで開いて、新しいゲームのフィルタリング機能を確認してください。

1. ゲームのページを開き、一覧に表示されるゲームの数を記録します。
2. カテゴリフィルターを適用し、一覧がそのカテゴリのゲームだけに更新されることを確認します。
3. カテゴリフィルターを解除してから、出版社フィルターを適用し、その出版社のゲームだけに更新されることを確認します。
4. カテゴリと出版社の両方のフィルターを適用し、結果が両方の条件を満たすことを確認します。

各手順で確認したことを報告し、期待どおりに動作しなかった箇所があれば指摘してください。
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、手順を実行して結果を報告します。issue の受け入れ基準と照らし合わせて報告内容を確認してください。問題がある場合は、追加の質問を送るか、pull request を作成する前にコードを修正するよう依頼します。

> [!NOTE]
> このテストには、`http://localhost:4321` でアプリが実行中である必要があります。開発サーバーを停止した場合は、プロンプトを送る前にもう一度起動してください。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーをダウンロードする場合があります。ブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開して pull request を作成する

機能が動作することを確認できたら、チームでレビューするために pull request (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルに移動し、Copilot が加えた変更をレビューします。
2. **+** アイコンを選択して変更をステージングします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![Copilot が加えた変更を表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## pull request を作成する

github.com や GitHub コマンドラインインターフェイス (CLI) など、pull request を作成する方法はいくつかあります。GitHub Copilot を使っているため、Copilot に作成してもらいましょう。関連する issue を見つけて、issue と関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを始めます。
2. GitHub ツールを使えるように、エージェントのドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトを送って、PR の作成を Copilot に依頼します。

   ```plaintext
   リポジトリでカテゴリと出版社によるフィルタリングに関係する issue を探してください。現在のブランチから新しい pull request を作成し、該当する issue に関連付けてください。
   ```

4. 必要に応じて **Continue** を選択し、情報の収集や操作の実行を Copilot に許可します。GitHub ツールを初めて使用するときは、GitHub での認証が求められる場合があります。画面の指示に従ってください。
5. Copilot が issue を検索して該当するものを見つけ、PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択して pull request をレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次の作業を行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルタリング機能を手動テストするために使用しました。
- ブランチを公開し、フィルタリング作業の pull request を作成しました。

MCP サーバーを使って機能を**テスト**しましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は[カスタムエージェントを作成][next-lesson]して、アクセシビリティレビューなど、対象を絞ったタスクを効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP レジストリ][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
