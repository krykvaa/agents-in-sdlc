---
title: "レッスン 6 - Playwright MCP による機能の検証"
description: "Playwright MCP を設定し、Copilot CLI のブラウザーでフィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードの作成だけでなく、データや外部サービスとの連携、ブラウザー操作などもワークフローに含まれます。Model Context Protocol (MCP) を使うと、Copilot CLI は外部ツールやサービスを利用できます。

このレッスンでは、Playwright MCP server を設定し、エージェントにブラウザーを操作させてフィルター機能を確認します。

## シナリオ

単体テストだけでは UI の使い勝手を十分に検証できません。Copilot に実際のブラウザーで作業中の Web サイトを操作させ、ユーザーと同じ操作で機能を確認します。

## MCP server を設定する

1. `.vscode/mcp.json` を開きます。
2. `github` server に加えて `playwright` server を設定します。

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

3. ファイルを保存します。

VS Code は Copilot が必要になったときに MCP server を起動し、信頼の確認を求める場合があります。構成を確認して、信頼を選択してください。

> [!CAUTION]
> MCP server はプロジェクトの依存関係と同様に慎重に扱ってください。ソース、発行元、セキュリティへの影響を確認してから使用してください。

## フィルター機能をブラウザーで検証する

アプリを起動し、次のプロンプトを Copilot CLI に送ります。

```plaintext
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot はブラウザーを起動して各手順を実行し、観察結果を報告します。Issue の受け入れ基準と照合し、問題があれば修正を依頼します。

## 概要と次のステップ

Playwright MCP を設定し、ブラウザーでフィルター機能を検証しました。次は、[QA エージェント][next-lesson]を使ってテストを体系化します。

## リソース

- [Model Context Protocol (MCP) の概要][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [VS Code の MCP 設定][vscode-mcp-config]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
