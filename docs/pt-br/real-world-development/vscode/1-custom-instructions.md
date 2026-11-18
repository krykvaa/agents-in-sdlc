---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica, disponibilize essa orientação ao Copilot. Os [arquivos de instruções][instruction-files] fornecem orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos aplicados a caminhos específicos.
- gerar a primeira parte dos dados para a filtragem — um auxiliar de editoras — usando as instruções atuais.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.
- executar novamente o mesmo prompt e observar o código gerado adotar o novo padrão.
- fazer commit das instruções e da parte de filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir alguns dos padrões definidos. O Copilot é probabilístico. O objetivo deste exercício é observar a tendência de mudança após a atualização das instruções, não obter um resultado idêntico em cada caractere.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento:

- A camada de dados sempre precisa de testes de unidade.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Os arquivos de instruções ajudam a garantir que o Copilot receba o contexto adequado para realizar tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de código e seus requisitos. Você pode especificar convenções de código, bibliotecas e os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações, como a pilha de tecnologias, uma visão geral do projeto, práticas recomendadas e orientações gerais.
- Arquivos `.github/instructions/*.instructions.md`, que podem oferecer diretrizes para linguagens como TypeScript ou Astro ou para tarefas específicas, como criar um componente de interface ou testes de unidade.

> [!NOTE]
> Em um IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para complementações de código ou sugestões de próximas edições.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam arquivos de instruções do repositório e arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre a criação desses arquivos está fora do escopo do workshop. Os exemplos do projeto mostram uma abordagem representativa:

- Mantenha `copilot-instructions.md` focado em orientações para o projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões gerais de código.
- Use arquivos `*.instructions.md` para instruções específicas para tipos de arquivo, como testes de unidade, componentes Astro e a camada de dados, ou para tarefas específicas.
- Use linguagem natural e orientações claras. Inclua exemplos de como o código deve ou não deve ser escrito.

Não há uma única maneira de criar arquivos de instruções nem de usar IA. Experimente para descobrir o que funciona melhor no projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Neste projeto, há instruções para várias tarefas, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada ambiente oferece essa opção de forma diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição do ambiente usado explicará quando isso for relevante.
>
> Procura modelos ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas do projeto

Leia os arquivos de instruções incluídos no repositório: o arquivo principal `copilot-instructions.md` e os arquivos `*.instructions.md` para várias tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Observe a descrição breve do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e examine os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início, que define um glob relativo à raiz do repositório. Neste caso, ele corresponde a arquivos de teste TypeScript, como `**/*.test.ts`.
5. Observe as instruções para criar testes de unidade no projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos do projeto. Assim, você pode dividir instruções maiores em arquivos menores e reutilizáveis e indicar exemplos que o Copilot pode seguir. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Criar uma branch para as alterações

Vamos seguir algumas práticas recomendadas e criar uma branch para as alterações.

1. Volte ao codespace do exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione uma nova branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa indica as opções relevantes.

Para observar o impacto das instruções personalizadas, envie primeiro um prompt com o arquivo de instruções atual. Depois, atualize-o e envie o mesmo prompt para comparar.

1. Feche as guias do editor abertas nos exercícios anteriores para limitar o contexto do Copilot.
2. Abra `src/lib/publishers.ts` para indicar onde o auxiliar deve ficar.
3. Na visualização Chat, selecione **Agent** no menu suspenso de agentes para permitir que o Copilot altere arquivos.

   ![Captura de tela do seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um helper de acesso a dados que retorne uma lista de todas as editoras, com o nome e o ID de cada uma. Aplique as alterações no arquivo.
   ```

5. O Copilot explora o projeto e aplica as alterações, geralmente no arquivo auxiliar e nos testes.
6. Observe que a função proposta tem tipos, recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras. Essas convenções vêm de `.github/instructions/drizzle.instructions.md`, aplicado a `src/lib/*.ts`.
7. Observe que o código proposto **não tem** comentários TSDoc nem cabeçalho de comentários no arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários mesmo sem essa orientação. Tudo bem: o objetivo é observar a consistência depois da atualização das instruções.

## Adicionar um novo padrão ao repositório

`.github/copilot-instructions.md` fornece orientações sobre o projeto. Vamos documentar os padrões de código do repositório para melhorar as sugestões.

1. Abra `.github/copilot-instructions.md` novamente.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Ela ainda não inclui uma regra de documentação no código.
3. Adicione estas linhas Markdown abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, as instruções podem ser criadas no nível do repositório (`.github/copilot-instructions.md`) para orientações gerais ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar certo para padrões gerais, como a regra de comentários de documentação que você acabou de adicionar.

## Executar novamente o prompt e observar a diferença

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Selecione novamente `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie **o mesmo prompt**:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um helper de acesso a dados que retorne uma lista de todas as editoras, com o nome e o ID de cada uma. Aplique as alterações no arquivo.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a:

   ```typescript
   /**
    * Helpers de acesso a dados de editoras para a plataforma de financiamento coletivo Tailspin Toys.
    * Fornece funções para recuperar informações de editoras do banco de dados.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Retorna uma lista de todas as editoras com seus IDs e nomes.
    *
    * @param db - O cliente de banco de dados do Drizzle.
    * @returns Uma promessa que é resolvida com um array de objetos de editoras.
    */
   ```

Você orientou o Copilot a seguir um novo padrão e aplicá-lo ao código real que será usado no próximo exercício.

## Fazer commit das instruções e enviar a branch

Os arquivos de instruções são recursos do repositório e devem ser controlados pelo Git. Vamos fazer commit e enviar a branch.

1. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Confirme se as instruções e as alterações do auxiliar estão presentes:

   ```bash
   git status
   ```

3. Prepare e faça commit da atualização das instruções e de todos os arquivos alterados pelo Copilot para o auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras alterações relacionadas, como testes, adicione também esses arquivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e fazer merge de um pull request

Com a branch enviada, crie um pull request associado à issue sobre o padrão de documentação. O Copilot pode localizar a issue, criar um PR que a encerra e fazer o merge.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot age no GitHub por você. O modelo do projeto já conecta as ferramentas do GitHub ao espaço de trabalho, então não é preciso configurar nada. Na primeira vez que usar uma ferramenta do GitHub, talvez o VS Code peça que você entre na sua conta. Siga as instruções. Você conhecerá essa conexão (Model Context Protocol) em um exercício posterior.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização do Copilot Chat e confirme se **Agent** está selecionado.
3. Peça ao Copilot para encontrar a issue sobre as instruções e criar um PR a partir da branch atual, descrevendo as instruções e a base do auxiliar de editoras:

   ```
   Encontre a issue relacionada à atualização do arquivo de instruções. Crie um novo PR a partir da branch atual, destaque que ele fecha essa issue e mencione também que adicionamos a estrutura básica do helper de editoras para o próximo trabalho de filtragem.
   ```

4. O Copilot começará a localizar a issue e criar o PR.
5. Se o Copilot pedir permissão para executar ações do GitHub, revise o comando e selecione **Allow** quando apropriado.
6. Depois que o PR for criado, peça ao Copilot para fazer o merge e voltar à branch main:

   ```
   Faça o merge do PR em main. Em seguida, volte localmente para main e baixe o código mais recente para manter tudo atualizado.
   ```

7. Se o Copilot pedir permissão para executar ações do GitHub ou comandos de shell, revise o comando e selecione **Allow** quando apropriado.

Você criou e integrou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções existentes.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.
- executar novamente o prompt e observar o código proposto adotar o padrão.
- fazer commit das instruções e da base do auxiliar em `main` para que o próximo exercício possa usá-las.

Em seguida, você aplicará essas instruções no [modo de agente][next-lesson] enquanto o Copilot adiciona um recurso ao projeto.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot/tree/main
