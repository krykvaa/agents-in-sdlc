---
title: "Exercício 5: Iterar no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em tarefas para melhorar a experiência e adicionar funcionalidades. Você pediu que ele documentasse o código, criasse um recurso de jogos relacionados para a equipe de design e implementasse recursos de acessibilidade, incluindo controles de alto contraste e modo claro. Vamos revisar as alterações e, se necessário, orientar o Copilot para aprimorá-las.

### Cenário

Os fundamentos do design de software e do DevOps não mudam com a IA generativa. Sempre precisamos revisar o código gerado e seguir o processo de DevOps habitual. Vamos revisar as sugestões do GitHub Copilot para a documentação, os jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão pela equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem trabalha de forma assíncrona e sem supervisão contínua, existem restrições de segurança:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente de nuvem trabalha no GitHub Actions, em um ambiente separado e temporário.
- Workflows do GitHub Actions precisam de aprovação humana antes de serem executados.
- O [acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores MCP.

## Revisar a documentação gerada

Vamos começar pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: o PR que adiciona documentação ao código.

> [!NOTE]
> O PR pode mostrar um aviso de que o Copilot está bloqueado por um firewall. Isso é **esperado**, pois o acesso a recursos externos, incluindo servidores MCP, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Abra seu repositório em uma nova guia do navegador.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o PR com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde a conclusão do trabalho; pode levar alguns minutos.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui comentários TSDoc e outra documentação. As alterações exatas podem variar. Procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentários, orientações dos arquivos de instruções personalizados vistos no início do Exercício 2.
6. Quando terminar, volte à guia **Conversation** e role para baixo.
7. Confira se há um indicador de workflows aguardando aprovação.
8. Se houver, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows devem entrar na fila de verificações do PR. Confirme se as verificações do projeto passaram para o aplicativo Astro. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Você também pode mencionar o Copilot em comentários do PR ou comentários em linha do código, como faria com outra pessoa da equipe. O Copilot poderá iniciar outra sessão para atender à solicitação. Como os resultados são não determinísticos, não há um prompt único. Você pode pedir, por exemplo:

- adicionar um cabeçalho com uma breve descrição no início de cada arquivo de código.
- adicionar comentários TSDoc a arquivos TypeScript e Astro.
- criar um README que descreva a estrutura da aplicação Astro.

1. Comente solicitando uma alteração na documentação gerada e mencione **@copilot**. Use uma das ideias acima ou outra orientação de documentação para o código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot, que iniciará uma nova sessão.
3. Selecione **Back to pull request** para voltar ao PR.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, deverá aparecer um novo commit no PR.
5. Selecione **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, marque o PR como pronto para revisão e faça o merge.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Agora, volte ao PR criado pelo Copilot para resolver a issue de exibir jogos relacionados na página de detalhes.

1. Acesse seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione **Files changed** para revisar o código gerado.
5. Quando terminar, volte à guia **Conversation** e role para baixo.
6. Confira se há workflows aguardando aprovação.
7. Se houver, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Confirme se as verificações do projeto passaram para o aplicativo Astro. Isso pode levar alguns minutos.
9. **Opcional:** teste o recurso no Codespace. Abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, `copilot/fix-8`:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados. Você pode pedir alterações pelo comentário na guia **Conversation**, como ajustar o número de jogos exibidos ou adicionar cabeçalhos de comentários e comentários TSDoc. Observe que a issue foi atribuída **antes** da atualização das instruções. O Copilot poderá iniciar outra sessão para atender a novos pedidos.

## Revisar os recursos de acessibilidade

Por fim, revise os recursos de acessibilidade implementados pelo agente personalizado. O PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no painel de controle do Exercício 4.

1. Acesse seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde a conclusão do trabalho; isso pode levar alguns minutos.

4. Selecione **Files changed** para revisar o código gerado.
5. Examine especialmente:
   - os componentes de interface para alternar os modos.
   - o uso de armazenamento local para persistir as preferências.
   - as alterações de CSS para os modos de alto contraste e claro.
   - os atributos de acessibilidade, como rótulos ARIA e navegação pelo teclado.
   - o código JavaScript/TypeScript que gerencia a alternância.
6. Quando terminar, volte à guia **Conversation** e role para baixo.
7. Confira se há workflows aguardando aprovação.
8. Se houver, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Confirme se as verificações do projeto passaram para o aplicativo Astro.
10. **Opcional:** teste os recursos no Codespace. Abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Inicie a aplicação e teste os controles de alto contraste e modo claro no navegador. Confirme se funcionam e se as preferências persistem após recarregar a página.

O agente personalizado de acessibilidade orientou a implementação de acordo com as práticas recomendadas. Se encontrar problemas ou melhorias, mencione **@copilot** em um comentário para solicitar alterações.

## Exercício opcional: continuar delegando

O agente de nuvem é útil para tarefas reais do backlog que você pode delegar enquanto se concentra em outras atividades. Para praticar, crie mais algumas issues e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na lista de jogos.
- adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo das lições, você:

- **inspecionou os arquivos de instruções personalizados** do repositório para observar o efeito deles no trabalho do agente.
- **atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **criou e usou um agente personalizado** de acessibilidade para implementar os modos de alto contraste e claro.
- **usou a página de agentes do Copilot como painel de controle** para acompanhar e orientar a sessão de acessibilidade.
- **revisou e aprimorou pull requests do agente de nuvem**, mencionando `@copilot` para solicitar mudanças e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar explorando, veja outros percursos:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas no terminal com os modos Plan e Autopilot, skills de agente, agente personalizado de QA, MCP do Playwright, Agent Merge e a integração opcional `/delegate` com o agente de nuvem.

No seu repositório, experimente:

- atribuir uma issue de refatoração ou cobertura de testes ao agente de nuvem.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
