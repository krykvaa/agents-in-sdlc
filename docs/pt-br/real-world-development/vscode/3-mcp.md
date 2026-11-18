---
title: "Exercício 3 - Testar o recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de desenvolver a filtragem com o modo de agente. Antes de abrir um pull request, confirme se ela funciona no navegador. Em vez de testar o aplicativo manualmente, conecte o **servidor MCP do Playwright** e peça ao Copilot para controlar um navegador real. Depois, publique sua branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionar o servidor MCP do Playwright ao espaço de trabalho.
- pedir ao Copilot para testar manualmente a filtragem em um navegador.
- publicar a branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica mais poderoso quando pode acessar ferramentas fora do editor. O Model Context Protocol (MCP) permite que o Copilot se comunique com ferramentas e serviços externos.

![Diagrama do funcionamento interno do modo de agente e da interação com contexto, LLM e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real, acessem informações atualizadas por meio de recursos e executem ações em seu nome por meio de ferramentas.

Um servidor MCP funciona como ponte entre o agente de IA e as ferramentas e serviços externos, gerenciando a comunicação. Cada servidor oferece ao agente um conjunto diferente de ferramentas e recursos.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: oferece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: permite automatizar o navegador com o Playwright, incluindo navegar por páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP. O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta e as contribuições para esse ecossistema.

> [!CAUTION]
> Por segurança, trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use apenas servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Observe que já há um servidor `github` configurado:

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

Essa entrada vem do modelo do projeto e permitiu que o Copilot lesse o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é preciso instalar nada localmente: o VS Code se conecta por HTTP e você autentica na primeira vez que o Copilot usa uma ferramenta.

## Adicionar o servidor MCP do Playwright

Adicione um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador controlável para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`:

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

2. Salve o arquivo.

Diferentemente do servidor remoto `github`, `playwright` é local: o VS Code inicia-o no computador executando `npx @playwright/mcp@latest`. O sinalizador `--headless` executa o navegador sem uma janela visível, necessário em um codespace sem área de trabalho gráfica.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright nos testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP **sob demanda**: quando o Copilot precisa das ferramentas de um servidor pela primeira vez, o VS Code inicia esse servidor e pede que você confirme se confia nele.

1. Em `.vscode/mcp.json`, aparece uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor e confirmar se a configuração está correta.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e confirme para iniciá-lo.

Iniciá-lo manualmente é opcional. Se você ignorar essas etapas, o VS Code iniciará o servidor e solicitará sua aprovação quando o Copilot precisar das ferramentas.

## Testar o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] dá ao Copilot acesso a um navegador real. O agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou.

Internamente, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server], não capturas de tela. Assim, o agente raciocina sobre elementos estruturados e rotulados — botões, links e itens de lista — como uma tecnologia assistiva, e um teste funcional também serve como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot para testar a filtragem:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, seguirá as etapas e apresentará os resultados. Compare o relatório com os critérios de aceitação da issue. Se algo parecer errado, faça perguntas ou peça que o código seja corrigido antes de abrir o pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se você tiver interrompido o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usa o MCP do Playwright, talvez seja necessário baixar um navegador. Se o Copilot informar que ele está ausente, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

Agora que confirmou o funcionamento do recurso, publique a branch `filtering-vscode` para que a equipe possa revisar o pull request.

1. Abra o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control com as alterações realizadas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Há várias maneiras de criar um pull request, inclusive pelo github.com e pela interface de linha de comando do GitHub (CLI). Como você já está usando o GitHub Copilot, peça que ele crie o PR e associe-o à issue correta.

1. Abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma sessão nova.
2. Confirme se **Agent** está selecionado para permitir o uso das ferramentas do GitHub.
3. Peça ao Copilot que crie o PR:

   ```plaintext
   Encontre no repositório a issue relacionada à filtragem por categoria e editora. Crie um novo pull request para a branch atual e associe-o à issue correta.
   ```

4. Selecione **Continue** quando necessário para permitir que o Copilot reúna informações e execute operações. Na primeira vez que usar uma ferramenta do GitHub, talvez seja necessário autenticar. Siga as instruções.
5. Observe o Copilot procurar a issue correta e criar o PR.
6. Selecione o link exibido pelo Copilot para revisar o pull request. **Não faça o merge ainda.**

## Resumo e próximos passos

Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionou o servidor MCP do Playwright ao espaço de trabalho.
- usou o servidor para testar manualmente o recurso de filtragem em um navegador antes da entrega.
- publicou a branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso, mas o MCP é apenas uma das maneiras de ampliar o Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para agilizar tarefas focadas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
