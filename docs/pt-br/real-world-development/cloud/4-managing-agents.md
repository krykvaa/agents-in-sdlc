---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você atribuiu três tarefas diferentes ao agente de nuvem do Copilot para melhorar a experiência e adicionar funcionalidades. Embora ele trabalhe de forma assíncrona e autônoma, é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel, você pode ver as tarefas de agentes com pull requests abertos, explorar as operações realizadas e orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para pedir outra funcionalidade.

## Cenário

Depois de atribuir a implementação do modo de alto contraste, a equipe percebeu que seria útil adicionar também um modo claro. Como o agente já estava atualizando o estilo do site e adicionando controles de alternância, vamos orientá-lo para incluir também o modo claro.

## Revisar as tarefas do agente de nuvem do Copilot

Vamos conferir o estado atual das tarefas atribuídas ao agente de nuvem.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe as tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação do código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações do Copilot. Elas podem incluir:
   - obter o código do repositório.
   - configurar o ambiente de trabalho.
   - configurar servidores MCP.
   - executar etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel de pull request à direita, que mostra o PR e os arquivos alterados.

## Orientar o agente de nuvem

Agora que você conhece as tarefas ativas, peça ao Copilot que inclua um controle de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada ao modo de alto contraste. O título exato depende do nome escolhido pelo Copilot e do estado do trabalho.

    ![Sessão de acessibilidade no painel de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que a configuração seja concluída e o agente comece a trabalhar. Você verá mensagens semelhantes às exibidas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione este prompt:

    ```plaintext
    Enquanto trabalhamos no modo de alto contraste, vamos adicionar também um modo claro. Deve haver um controle para esse modo que permita aos usuários selecionar a aparência desejada.
    ```

    ![Captura de tela da tarefa do agente de nuvem na página de agentes com a caixa de diálogo para orientar a sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe o Copilot reconhecer o prompt e incluí-lo no fluxo de trabalho.

## Deixar o Copilot trabalhar

O Copilot incorporará a nova solicitação ao fluxo de trabalho depois de concluir a etapa que estava executando quando você enviou a mensagem.

Isso pode levar alguns minutos. Aproveite para fazer uma pausa e refletir sobre o que aprendeu até aqui.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, seu painel central para acompanhar e orientar tarefas do agente de nuvem. Você pode acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar sessões em andamento para ajustar o escopo ou oferecer orientações.

Você explorou estes conceitos:

- usar o painel de controle e a página de agentes para acompanhar tarefas.
- redirecionar uma sessão em andamento para pedir outra funcionalidade.

Enquanto o Copilot conclui os recursos de acessibilidade, vamos [iterar nos pull requests que ele criou][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
