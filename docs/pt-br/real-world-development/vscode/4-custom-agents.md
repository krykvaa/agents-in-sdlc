---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados em tarefas ou domínios específicos do fluxo de desenvolvimento. Ao defini-los em arquivos Markdown na pasta `.github/agents` do repositório, você oferece instruções focadas, práticas recomendadas, padrões de código e conhecimento do domínio. Assim, equipes podem transformar sua experiência em agentes reutilizáveis: um agente de acessibilidade que aplica as diretrizes [WCAG][wcag], um agente de segurança que segue práticas de código seguro ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a experiência e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos são definidos principalmente em arquivos Markdown e ensinam a IA a executar tarefas. A distinção mais simples é: o **agente personalizado** é quem trabalha, e as **skills** são ferramentas.

Os agentes personalizados têm uma janela de contexto própria e podem orquestrar skills e até outros agentes. Neste laboratório, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade. Ele também poderá usar skills, como uma skill para o fluxo de pull requests ou para executar e gerenciar testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Como em qualquer uso de IA, teste e itere para encontrar o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer garantir que todas as pessoas possam usar sua plataforma de financiamento coletivo, independentemente da visão ou das preferências visuais. Comentários recentes indicaram dificuldade para ler o tema escuro devido ao contraste insuficiente entre o texto e o fundo. Para resolver isso, a equipe de design solicitou um modo de alto contraste que as pessoas possam ativar e desativar.

Como a acessibilidade é importante, vamos usar um agente personalizado para implementar a funcionalidade rapidamente.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar o modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Já existe um agente personalizado de acessibilidade. Vamos revisar seu conteúdo para entender como ele orientará o Copilot.

Volte ao codespace, abra um terminal e crie uma nova branch a partir de `main` para o trabalho de acessibilidade. Assim, o PR de filtragem do Exercício 3 continuará separado:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código para HTML, CSS e JavaScript.
   - armadilhas e erros comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code mostra todos os agentes personalizados definidos em `.github/agents` no menu suspenso de agentes, na parte inferior da visualização Copilot Chat. Você pode selecionar um agente para limitar a sessão às instruções e ferramentas dele.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa indica as opções relevantes.

1. Na visualização Chat, selecione **Agent** no menu suspenso de agentes, se ainda não estiver selecionado.

   ![Captura de tela do seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione o menu suspenso de agentes na parte inferior da visualização Chat. Ele mostra o agente ativo, que por padrão é **default**.
3. Selecione **Accessibility agent** na lista de agentes.
4. Envie este prompt ao agente de acessibilidade:

    ```plaintext
    Adicione um modo de alto contraste ao site. Deve haver um controle que o usuário possa ativar para usar o alto contraste, e a configuração deve persistir após recarregar a página por meio do armazenamento local do navegador.
    ```

5. O Copilot Chat explorará o código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para revisão.
6. O indicador **Files changed** será atualizado à medida que o agente modificar arquivos.

> [!NOTE]
> Esse processo pode levar alguns minutos. O Copilot fará alterações reais no repositório, editando componentes Astro, CSS e testes relacionados.

Você acompanhará e orientará o trabalho na próxima lição.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes especializados em tarefas e domínios. Eles permitem transformar a experiência e os padrões da equipe em agentes reutilizáveis.

Você explorou estes conceitos:

- como agentes personalizados são definidos.
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [acompanhará e orientará o trabalho do agente][next-lesson], revisando as alterações à medida que aparecem e adicionando um controle de modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar o recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
