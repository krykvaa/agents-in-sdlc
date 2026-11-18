---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Poucas organizações estão livres de dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que ficaram paradas no backlog por falta de tempo. O agente de nuvem do GitHub Copilot pode atualizar o código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria um pull request (PR) em rascunho para revisão por uma pessoa desenvolvedora. Assim, você pode delegar tarefas repetitivas e se concentrar em questões mais abrangentes.

Neste exercício, você vai explorar:

- a personalização do ambiente de geração de código.
- como garantir que as operações sejam realizadas com segurança.
- a importância de issues com escopo claro.
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer resolver algumas dívidas técnicas. A documentação deixada por quem criou a primeira versão do site está em estado inadequado — na verdade, ela está completamente ausente. Como primeira etapa, a equipe quer comentários TSDoc em todas as funções exportadas da aplicação.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Ela quer exibir jogos relacionados — outros títulos da mesma categoria — na página de detalhes de cada jogo para que as pessoas continuem navegando. Por enquanto, não é necessária uma implementação refinada; basta algo que permita testar os critérios de aceitação da experiência do usuário. Essa tarefa está bloqueada, mas há issues de prioridade maior.

Essas tarefas podem acabar adiadas e são boas candidatas para atribuição ao agente de nuvem do Copilot. Ele trabalha de forma assíncrona enquanto você cuida de outras tarefas e, depois, pode revisar o trabalho para confirmar se está correto.

## Conhecer o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de forma semelhante a uma pessoa desenvolvedora. Há várias maneiras de trabalhar com ele, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request em rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão durante a execução ou depois que ela terminar. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo bem definido

O GitHub Copilot não faz mágica: você ainda precisa pensar. Até operações aparentemente simples ficam complexas quando examinadas de perto. Por isso, [defina com cuidado o escopo das tarefas atribuídas ao agente de nuvem][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Se você já concluiu o exercício prático sobre instruções, viu `.github/copilot-instructions.md` em ação. Caso contrário, confira o arquivo antes de atribuir trabalho ao agente de nuvem. Assim, você poderá observar o efeito das instruções posteriormente.

Abra os arquivos a seguir na interface Web do GitHub ou em um codespace:

- `.github/copilot-instructions.md`: examine a seção **Code standards**, especialmente as expectativas sobre convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente de nuvem, observe se o pull request inclui comentários TSDoc, segue as convenções de TypeScript e tem cabeçalhos de comentários. Essas orientações vêm dos arquivos de instruções e serão mencionadas novamente em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem

Criar código costuma exigir um ambiente específico e scripts de configuração para garantir que tudo esteja pronto. O mesmo se aplica ao atribuir tarefas ao Copilot, que trabalha de maneira semelhante a uma pessoa engenheira de software.

O agente de nuvem usa o [GitHub Actions][github-actions] para trabalhar. Você pode personalizar o ambiente com um [workflow de configuração][setup-workflow] em `.github/workflows/copilot-setup-steps.yml`, executado antes do início do trabalho. Isso disponibiliza as ferramentas e dependências necessárias. O workflow foi configurado previamente para este laboratório: ele instala o Node.js e as dependências do projeto, os navegadores para testes de ponta a ponta e configura o banco de dados SQLite da aplicação Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina somente as permissões mínimas necessárias às suas etapas. O Copilot terá seu próprio token para as operações dele.
    permissions:
      # Para clonar o repositório durante as etapas de configuração, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se o repositório não for clonado nessas etapas, o Copilot fará isso automaticamente depois que elas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo: Node.js (o aplicativo agora é todo baseado em Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local para que os builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer outro workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job, `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar o workflow sem esperar o Copilot executá-lo.

## Adicionar documentação

Embora todo mundo reconheça a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica reduz a produtividade e dificulta a manutenção do código e a integração de novas pessoas à equipe. O Copilot pode ajudar a criar documentação, por isso esta é uma boa tarefa para atribuir ao agente de nuvem. Ele trabalhará em segundo plano; você revisará o resultado em um exercício futuro.

1. Abra uma nova guia do navegador e acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. Defina o **Title** como `Code lacks documentation`.
6. Defina a **Description** como:

    ```plaintext
    Nossa organização exige que funções e métodos incluam comentários TSDoc quando forem úteis. Infelizmente, as atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para incluir comentários que esclareçam o comportamento.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

    ![Detalhes da atribuição ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está configurando o ambiente com o workflow mostrado acima, analisando o projeto e definindo uma abordagem.

13. Revise a lista e as tarefas que o Copilot pretende concluir.
14. Role a linha do tempo do PR para baixo e confira se há uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

    ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Percorra a sessão em tempo real e observe como o Copilot resolve a tarefa: ele explora o código, entende o estado do projeto, considera a abordagem e cria o código.

Isso pode levar alguns minutos. Um dos objetivos do agente de nuvem é executar tarefas de forma assíncrona e liberar você para outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente e, em seguida, voltando nossa atenção a outras atividades.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma grande vantagem do agente de nuvem é dividir o trabalho: enquanto você se concentra em uma tarefa, ele trabalha em outra. Mesmo que a seção de jogos relacionados talvez não leve muito tempo para ser criada, você pode usar esse tempo em outras atividades. Vamos atribuir a tarefa ao agente de nuvem.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. Defina o **Title** como `Show related games on the game details page`.
6. Defina a **Description** como:

    ```markdown
    Queremos ajudar as pessoas a descobrir mais jogos exibindo jogos relacionados na página de detalhes de cada jogo. A equipe de design quer explorar a experiência e fazer alguns testes de aceitação. Estes são os requisitos:

   - Adicione em `src/lib/` um helper de acesso a dados que retorne outros jogos da mesma categoria de um determinado jogo, excluindo esse jogo.
   - Exiba na página de detalhes uma seção "Related games" que use esse helper.
   - Trate o caso em que um jogo não tenha jogos relacionados.
   - Crie testes de unidade para o novo helper.
   - Antes de criar o PR, garanta que todos os testes sejam aprovados.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot**.

    ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Em breve, você deverá ver 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando na segunda solicitação. O agente de nuvem funciona de forma semelhante a uma pessoa engenheira de software; não é necessário acompanhá-lo o tempo todo, mas revise o resultado quando estiver pronto. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para execução assíncrona e resolver dívida técnica, criar recursos ou ajudar a migrar código entre frameworks.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, vamos criar e usar agentes personalizados. O [agente de nuvem também pode usar servidores MCP][cloud-agent-mcp] e instruções personalizadas, apresentadas nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
