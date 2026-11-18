---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados em tarefas ou domínios específicos do fluxo de desenvolvimento. Ao defini-los em arquivos Markdown na pasta `.github/agents` do repositório, você fornece instruções focadas, práticas recomendadas, padrões de código e conhecimento do domínio. Assim, as equipes podem transformar sua experiência em agentes reutilizáveis: um agente de acessibilidade que aplica as diretrizes [WCAG][wcag], um agente de segurança que segue práticas de código seguro ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a experiência e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos são definidos principalmente em arquivos Markdown e ensinam a IA a executar tarefas. A distinção mais simples é: o **agente personalizado** é quem trabalha, e as **skills** são ferramentas.

Os agentes personalizados têm uma janela de contexto própria e podem orquestrar skills e até outros agentes. Neste laboratório, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade e poderá usar skills, como uma skill para o fluxo de pull requests ou para executar e gerenciar testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Como em qualquer uso de IA, teste e itere para encontrar o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Neste exercício, você vai explorar:

- como agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys quer garantir que todas as pessoas possam usar sua plataforma de financiamento coletivo, independentemente da visão ou das preferências visuais. Comentários recentes indicaram dificuldade para ler o tema escuro devido ao contraste insuficiente entre o texto e o fundo. Para resolver isso, a equipe de design solicitou um modo de alto contraste que as pessoas possam ativar e desativar.

Como a acessibilidade é importante, queremos implementar esse recurso rapidamente. Vamos usar um agente personalizado para gerar a funcionalidade.

## Revisar o agente personalizado de acessibilidade

Já existe um agente personalizado de acessibilidade. Revise seu conteúdo para entender como ele orientará o Copilot.

Volte ao codespace e examine o arquivo do agente:

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código para HTML, CSS e JavaScript.
   - armadilhas e erros comuns.

## Criar e atribuir uma issue

O painel de agentes do GitHub é o local central para trabalhar com agentes. Você pode atribuir tarefas ao agente de nuvem do Copilot, acompanhar as tarefas e fornecer orientações adicionais. Vamos atribuir uma tarefa para criar o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia de issues.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. Defina o **Title** como `Add high contrast mode to website`.
6. Defina a **Description** como:

    ```plaintext
    Precisamos de um modo de alto contraste para o site. Deve haver um controle que o usuário possa ativar para usar o alto contraste. A configuração deve ser armazenada no armazenamento local do navegador.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.
9. Selecione **Accessibility agent** na lista de agentes personalizados.

    ![Captura de tela da atribuição ao agente de nuvem com o agente personalizado de acessibilidade selecionado](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar em segundo plano.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados em tarefas e domínios específicos. Com eles, é possível transformar a experiência e os padrões da equipe em agentes reutilizáveis.

Você explorou estes conceitos:

- como agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, vamos [acompanhar e orientar a sessão do agente][next-lesson] pelo painel de controle.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar o uso de agentes personalizados na organização][org-custom-agents]
- [Preparar o uso de agentes personalizados na empresa][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
