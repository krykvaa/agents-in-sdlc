---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加してフィルターコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot に特定の方法で作業させる必要がある場合は、指示を利用できます。Instruction ファイルは、コードの内容だけでなく構造や品質基準も Copilot に伝えます。

このレッスンでは、リポジトリの指示とパススコープ指示がエージェントにどのように渡されるかを確認し、ドキュメント標準を追加します。

## Instruction ファイルの種類

- `.github/copilot-instructions.md`: リポジトリ全体のリクエストに毎回適用される指示です。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイル形式、タスクに適用される指示です。

> [!NOTE]
> Instruction の形式やサポート範囲は環境によって異なります。利用前に [カスタム指示のサポートリファレンス][custom-instructions-support] を確認してください。

## プロジェクトのカスタム指示を確認する

1. Codespace のエディターで `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要とコーディングガイドを確認します。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` の `applyTo` フィールドで、適用対象を確認します。

## チームのガイドラインに合わせる

新しい TypeScript に TSDoc コメントを追加するルールを追加します。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクションを開きます。
2. 次の箇条書きを最後に追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動保存されます。

## 更新した指示を使用する

Copilot CLI は会話開始時にリポジトリの指示を読み込みます。会話を終了せずに保存されたフィルター会話を再開します。

1. `/exit` を入力して会話を終了します。会話は自動保存されます。
2. リポジトリのルートから、`game-filters-cli` ブランチで次を実行します。

   ```bash
   copilot --resume
   ```

3. プロンプトで、更新した指示にコードを合わせるよう依頼します。
4. `/diff` を入力し、TypeScript の TSDoc コメントを確認します。

## 概要と次のステップ

リポジトリの指示を確認し、新しいドキュメント基準をフィルターコードに適用しました。次は、[quality-checks スキル][next-lesson]をカスタマイズして、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI 用カスタム指示の追加][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示作成のベストプラクティス][instructions-best-practices]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
