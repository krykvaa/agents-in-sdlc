---
title: "Lição 12 - Publicar o site com o GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um workflow do Pages, revisar o pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dê ao Space Quiz um endereço público publicando-o com o GitHub Pages. O Copilot pode preparar o workflow e orientar a publicação, mas você continua responsável por revisar as mudanças e confirmar que o site está no ar.

Nesta lição, você vai:

- pedir ao Copilot que configure a publicação no GitHub Pages.
- revisar o workflow e o pull request antes de fazer o merge.
- acompanhar a publicação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma forma simples de experimentá-lo. Publicar o site de um único arquivo com o GitHub Pages o disponibiliza em uma URL pública, sem adicionar um servidor ou dependência de tempo de execução.

## Peça ao Copilot para preparar a publicação

> [!WARNING]
> Criar um pull request pode fazer commit e enviar código. Revise as configurações de aprovação e o repositório de destino antes de enviar este prompt. Com **Approve all**, não espere um prompt de confirmação separado.

Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## Revise o workflow e o pull request

1. Confirme se o README foi adicionado somente quando o repositório ainda não tinha um.
2. Revise as mudanças do pull request, incluindo as etapas de build e publicação do workflow, os gatilhos e as permissões.
3. Confirme se o workflow publica a partir da branch padrão, e não de um pull request ainda não mesclado.
4. Peça as correções necessárias e revise o diff atualizado. Aguarde a aprovação nas verificações obrigatórias do pull request antes de autorizar o merge.

## Autorize o merge e a publicação

> [!WARNING]
> O prompt a seguir autoriza o agente a fazer merge do pull request revisado, alterar as configurações de Pages do repositório e publicar o site. Envie-o somente depois de concluir a revisão e confirmar o repositório e o pull request. As configurações de aprovação podem permitir essas ações sem outro pedido de confirmação.

Substitua `<PR_NUMBER>` pelo número do pull request que você revisou e envie este prompt separado:

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama em três etapas: um pull request de space-quiz mesclado; uma execução bem-sucedida de Deploy website no GitHub Actions com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz publicado em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Verifique a publicação

1. Acompanhe a execução de publicação do GitHub Actions até que ela seja concluída com sucesso.
2. Abra a URL informada e confirme se o Space Quiz carrega.

Se o agente não puder habilitar o Pages nem fazer merge do pull request, siga as instruções específicas que ele fornecer. Não considere a tarefa concluída até que a publicação tenha sucesso e o site esteja no ar.

## Resumo e próximos passos

Seu Space Quiz agora está disponível como um site publicado, e você verificou a publicação em vez de confiar apenas no relato do agente. Continue com a [Lição 13: Revisão e próximos passos][next-lesson].

[next-lesson]: ../13-review/
