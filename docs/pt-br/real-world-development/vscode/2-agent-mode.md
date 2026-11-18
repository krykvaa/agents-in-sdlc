---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até a alteração mais simples em uma aplicação pode exigir mudanças em vários arquivos e operações como executar testes. Em geral, uma pessoa desenvolvedora precisa localizar os arquivos, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou faltando e repetir o processo. É aí que entra o modo de agente do Copilot.

O modo de agente trabalha com mais autonomia no IDE. Ele explora a estrutura do projeto, faz as alterações necessárias, executa tarefas como testes e corrige automaticamente os erros encontrados. Vamos usar esse modo para adicionar funcionalidades ao site.

> [!NOTE]
> Embora tenham nomes semelhantes, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE, com ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, usando issues e pull requests para interagir.

Neste exercício, você vai aprender como:

- o modo de agente pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- o modo de agente pode implementar recursos na interface e na camada de dados.
- revisar alterações e testes gerados pelo modo de agente antes do merge.

## Cenário

À medida que a lista de jogos cresce, você quer permitir a filtragem por categoria e editora. No exercício anterior, você adicionou um auxiliar para editoras. Agora, concluirá o trabalho restante na camada de dados, na interface e nos testes usando o modo de agente.

## Executar o site Tailspin Toys

Antes de fazer alterações, explore o site para conhecer o comportamento atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele permite listar jogos e exibir seus detalhes. A aplicação é um único projeto Astro que gera páginas HTML estáticas durante o build. As páginas consultam diretamente um banco de dados SQLite local por meio do Drizzle ORM no frontmatter; não há uma API de back-end separada nem framework de interface no cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade usa um pequeno `<script>` do Astro com APIs padrão do DOM.

### Iniciar o site

O comando `npm run dev` inicia o servidor de desenvolvimento Astro. Execute-o no GitHub Codespace:

1. Volte ao codespace. Você deve estar na branch `main` depois do exercício anterior.
2. Abra um terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, será exibido um aviso semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo pedir para abrir `http://localhost:4321` em um navegador, feche-a selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar o link `localhost` no terminal redireciona para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o servidor do codespace.

### Explorar o site

Os principais recursos do site incluem:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você vê uma página com mais informações, como título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa indica as opções relevantes.

O site funciona, mas queremos adicionar recursos. O modelo criou um backlog de issues automaticamente. Peça ao GitHub Copilot para exibi-lo.

1. Na visualização Chat, selecione **Agent** no menu suspenso de agentes. O agente planeja e implementa alterações em vários arquivos, executa comandos de terminal e usa ferramentas.

   ![Captura de tela do seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo em particular. O Claude Sonnet 4.5 foi usado para criar estas instruções para que as sugestões fossem relativamente consistentes. Como os modelos de linguagem são probabilísticos, suas sugestões podem ser diferentes; isso é normal.

> [!NOTE]
> Devido à natureza probabilística dos modelos de linguagem, o Copilot pode usar um comando MCP diferente e ainda concluir a tarefa.

3. Envie este prompt:

   ```plaintext
   Mostre o backlog de itens do meu repositório do GitHub. Ajude-me a priorizá-los com base no que será mais útil para o usuário.
   ```

4. Selecione **Continue** para executar o comando que lista as issues.
5. Revise a lista gerada.

Observe como o Copilot priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere código, examine o arquivo de instruções que fornecerá contexto. O arquivo [de interface do usuário][ui-instructions] contém orientações para adicionar recursos ao site.

1. No codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações sobre como adicionar funcionalidades:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para instruções de tipos de arquivo, incluindo Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Isso facilita a reutilização, a divisão de instruções complexas em partes menores e o uso de exemplos e modelos.

## Implementar a filtragem

Para concluir a filtragem, você precisará fazer pelo menos três alterações:

- adicionar ou aprimorar a lógica de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Os testes precisam ser executados e aprovados antes do merge. O modo de agente pode fazer esse trabalho.

1. Continue a conversa atual ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela da seleção do arquivo de instruções da interface do usuário](../../../_images/ex3-select-instructions-file.png)

3. Confirme se **Agent** continua selecionado.

   ![Captura de tela do seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme se **Claude Sonnet 4.5** continua selecionado.
5. Peça ao Copilot para implementar o recurso com base na issue relacionada do backlog:

   ```plaintext
   Atualize o site para incluir a filtragem por editora e categoria de acordo com os requisitos da issue relacionada no backlog do GitHub. Já existe um helper de editoras do exercício anterior; preserve e refine esse trabalho conforme necessário ao concluir as alterações restantes na camada de dados, na interface e nos testes. Garanta que todos os testes sejam aprovados antes de concluir. O servidor já está em execução, então não é necessário iniciá-lo.
   ```

6. Observe o Copilot explorar o projeto, localizar auxiliares de dados, a interface e os testes, alterar os arquivos e executar os testes.

   ![Captura de tela do Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot pode levar alguns minutos para explorar o projeto, modificar arquivos e executar testes. Durante o trabalho, os botões **Keep** e **Undo** podem aparecer no editor. Quando terminar, haverá uma opção **Keep** ou **Undo** para cada alteração; não é necessário selecioná-las enquanto o agente ainda trabalha.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo no painel do Copilot Chat pedindo confirmação para executar os testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e testes com falha durante o processo. O Copilot alterna entre geração de código e testes até concluir a tarefa e não detectar erros.

   ![Captura de tela de uma sessão completa do Copilot Chat no modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examine o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por outras ferramentas de IA.

10. Volte ao navegador com o site em execução e explore o novo recurso.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Você usou o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você aprendeu como:

- implementar recursos na interface e na camada de dados.
- explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- revisar alterações e testes antes do merge.

Agora, vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra: implementar paginação

À medida que a lista de jogos cresce, será necessário oferecer paginação. Use o que aprendeu neste exercício e peça ao Copilot para implementá-la. Considere:

- seguir as práticas recomendadas existentes, inclusive usar os arquivos de instruções.
- decidir se a pessoa usuária poderá escolher o tamanho da página ou se ele será fixo.
- incluir no prompt as orientações necessárias para obter a implementação desejada.
- iterar com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse processo é normal ao trabalhar com o Copilot.

## Recursos

- [Modos Ask, Edit e Agent do Copilot: o que fazem e quando usar][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
