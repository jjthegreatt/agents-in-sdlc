---
title: "Lição 12 - Implantar o site com o GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um fluxo de trabalho do Pages, revisar a solicitação de pull e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dê ao Space Quiz um endereço público implantando-o com o GitHub Pages. O Copilot pode preparar o fluxo de trabalho e orientar a implantação, mas cabe a você revisar as alterações e confirmar que o site está no ar.

Nesta lição, você vai:

- pedir ao Copilot para configurar a implantação do GitHub Pages.
- revisar o fluxo de trabalho e o pull request antes de mesclar.
- acompanhar a implantação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma maneira simples de experimentá-lo. Publicar o site de arquivo único com o GitHub Pages o disponibiliza em um endereço da Web público (URL), sem adicionar um servidor ou uma dependência de tempo de execução.

## Pedir ao Copilot que implante o site

Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt para configurar e implantar o site:

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama em três etapas: solicitação de pull do space-quiz integrada; execução bem-sucedida de Deploy website no GitHub Actions, com as etapas Build website, Upload artifact e Deploy to Pages; e Space Quiz publicado em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pede ao agente que crie e mescle um pull request e altere as configurações do Pages no repositório. Revise o diff, as permissões de implantação e todas as solicitações de aprovação do GitHub antes de prosseguir.

## Revisar o fluxo de trabalho e verificar a implantação

Revise as alterações antes de autorizar o merge e acompanhe a execução até confirmar que o site está publicado e acessível.

1. Confirme que o README foi adicionado somente se o repositório ainda não tinha um.
2. Revise as alterações da solicitação de pull, incluindo as etapas de compilação e implantação do fluxo de trabalho.
3. Faça merge somente depois que as verificações obrigatórias da solicitação de pull forem aprovadas.
4. Acompanhe a execução da implantação no GitHub Actions até que ela seja concluída com êxito.
5. Abra a URL informada e confirme que o Space Quiz carrega.

Se o agente não conseguir habilitar o Pages ou fazer merge da solicitação de pull, siga as instruções específicas que ele fornecer. Não considere a tarefa concluída até que a implantação seja bem-sucedida e o site publicado carregue.

## Resumo e próximos passos

Você configurou a implantação do Space Quiz, revisou o fluxo de trabalho e a solicitação de pull, acompanhou a implantação e confirmou que o site publicado carrega. Continue para [revisar e conhecer os próximos passos][next-lesson].

[next-lesson]: ../13-review/
