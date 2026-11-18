---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora o código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho ocorre no computador em tempo real, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo. Você pode revisar cada diff, aceitar ou rejeitar alterações e orientar a conversa com prompts complementares sem sair do editor.

Neste exercício, você vai:

- acompanhar o trabalho do agente de acessibilidade em tempo real.
- revisar e aceitar as edições propostas.
- orientar o agente durante a sessão para adicionar um controle de modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Durante o trabalho, a equipe de design solicitou também um controle de modo claro para pessoas que consideram o tema padrão escuro demais. Em vez de começar outra conversa, você orientará a sessão existente para ampliar o trabalho.

## Acompanhar o progresso do Copilot

A visualização Copilot Chat mostra cada etapa do agente: arquivos que explora, comandos que executa e edições propostas. Cada edição aparece na conversa com um diff e controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa e acompanhe as chamadas de ferramentas, leituras e edições de arquivos e comandos do terminal.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
   - selecione **Keep** para aceitar a alteração.
   - selecione **Undo** para revertê-la e permitir que o agente tente novamente.
4. Abra o indicador **Files changed** na parte superior do chat para conferir todos os arquivos alterados durante a sessão.

> [!NOTE]
> Se uma chamada pedir permissão, por exemplo, para executar um comando de terminal, o agente será pausado e exibirá uma solicitação **Allow**. Aprove apenas os comandos que considerar seguros.

5. Aguarde o agente concluir o modo de alto contraste. Ele exibirá uma mensagem final que resume as alterações.

## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, amplie a mesma conversa para adicionar um controle de modo claro. Como você continuará na mesma sessão, o agente manterá o contexto do trabalho, incluindo os componentes, a abordagem de estilo e o padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt complementar:

    ```plaintext
    Bom trabalho! Agora adicione um controle de modo claro junto ao controle de alto contraste. O modo claro deve seguir o mesmo padrão — persistir no armazenamento local e estar disponível na mesma área da interface. O usuário deve poder ativar o alto contraste e o modo claro de forma independente.
    ```

2. O agente retomará o trabalho, explorará as alterações já feitas e proporá mudanças para o modo claro.
3. Revise cada nova edição e use **Keep** ou **Undo**.
4. Se o agente seguir uma direção indesejada, como refatorar os controles de uma maneira que você não queira, interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma instrução complementar mais clara.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts complementares para pedir ajustes, como “simplifique o CSS e use variáveis CSS em vez de duplicar seletores”; corrigir o rumo, como “não adicione um terceiro controle; integre o modo claro ao painel de configurações existente”; ou ampliar o escopo, como “adicione também atalhos de teclado para os dois controles”.

## Revisar o conjunto de alterações

Antes de fazer commit, revise rapidamente tudo o que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Ela deve incluir componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos e examine os diffs. Confirme se seguem as práticas de acessibilidade do agente personalizado: atributos ARIA, navegação pelo teclado, HTML semântico e persistência com armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts complementares para ampliar o trabalho sem perder o contexto.

Neste exercício, você:

- acompanhou o trabalho do agente em tempo real.
- aceitou ou rejeitou edições individuais.
- orientou a sessão durante o trabalho com prompts complementares.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade como um pull request e revisará as alterações produzidas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
