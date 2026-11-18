---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要な準備をします。Tailspin Toys リポジトリのコピーを作成して、作業用の [codespace][codespaces] を起動し、エディターで GitHub Copilot Chat が動作することを確認します。

## ラボ用リポジトリを設定する

コードの作業用にリポジトリのコピーを作成します。[テンプレート][template-repository]からインスタンスを作成すると、ラボに必要なファイルがすべて含まれます。演習ではこのリポジトリを使用します。

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]に移動します。
2. ラボのリポジトリページで **Use this template** ボタンを選択し、**Create a new repository** を選択して、自分用のコピーを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの案内に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成できます。

    ![リポジトリテンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えてください。この後のラボで使用します。

> [!NOTE]
> **バックログの準備**
>
> テンプレートからリポジトリを作成すると、GitHub issue のバックログが自動的に作成されます。ワークショップ全体でこの issue を使って作業するため、自分で issue を登録する必要はありません。

## codespace を作成する

次に、ラボの演習で使用する codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードの作成、実行、デバッグができるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする、すべての機能を備えた IDE を利用できます。

1. 作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して、新しい Codespace を作成します。

    ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、サービスを手作業でインストールするよりもずっと早く完了します。その間に GitHub Copilot の機能を試してみましょう。次のレッスンで詳しく説明します。

> [!CAUTION]
> 今後の演習で codespace に戻ります。今はブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは、codespace またはローカルの [dev container][dev-containers] 内で実行できるように構成されています。どちらも必要な前提条件がインストールされた環境を提供します。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択してください。codespace と同じ開発コンテナーが構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat とエージェントモードを使う

GitHub Copilot Chat のエージェントモードを使用するには、IDE に GitHub Copilot Chat 拡張機能をインストールする必要があります。GitHub Codespace を使用している場合は、すでにインストールされています。

> [!TIP]
> GitHub Copilot Chat 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または、Visual Studio Code の拡張機能ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

拡張機能をインストールしたら、GitHub アカウントで認証して有効化します。

1. codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、codespace の上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに「Hello world」のようなメッセージを入力して Enter キーを押し、Copilot Chat を起動します。
4. 認証されていない場合は、GitHub アカウントへのサインインが求められます。画面の指示に従って認証します。

    ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証後、Copilot Chat ウィンドウが表示されます。

## まとめ

ラボ用リポジトリのコピーを作成できました。コードの作成を始めるときに使用する codespace の作成も開始しました。

## 次のステップ

Copilot を使い始めましょう。[演習 1 - カスタム指示][next-lesson]に進み、Copilot にプロジェクトの規約を伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
