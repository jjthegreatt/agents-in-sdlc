---
title: "Lição 10 - Encerramento e próximos passos"
description: "Recapitule o fluxo de trabalho do Copilot CLI, as duas solicitações de pull, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI, a interface de linha de comando (CLI) do GitHub Copilot, em um fluxo contínuo de trabalho com o Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtragem inicial.
- adicionou avaliações por estrelas, revisou o resultado em um navegador usando a porta encaminhada e fez merge manual da sua primeira solicitação de pull (PR).
- começou pela issue de filtragem, definiu a abordagem no modo Plan, implementou-a no modo Autopilot e revisou-a no modo Interactive.
- orientou o agente com instruções personalizadas, personalizou a skill existente `quality-checks` e a usou para executar testes unitários, lint e verificações de tipos.
- adicionou o servidor Model Context Protocol (MCP) do Playwright e o usou para explorar a filtragem em um navegador real.
- criou e selecionou um agente personalizado de garantia da qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador.
- revisou a alteração completa da filtragem e autorizou o Agent Merge para a PR da filtragem.
- explorou comandos de barra para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## O que você entregou

O workshop tem dois marcos de PR, cada um em sua própria branch criada a partir da `main` atualizada:

1. **Avaliações por estrelas:** exibir o `starRating` existente e indicar explicitamente quando não houver avaliação nos cartões dos jogos.
2. **Filtragem e fluxo de qualidade:** implementar a filtragem, atualizar as instruções e aplicá-las à funcionalidade, personalizar o relatório `quality-checks`, criar um perfil de QA e incluir os testes correspondentes.

Do planejamento da filtragem à abertura da PR, você usou a mesma conversa e branch. Reunimos esse trabalho em uma PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias maneiras: testes automatizados, inspeção do site no navegador e exploração do navegador pelo Copilot usando MCP. A skill `quality-checks` executou testes unitários, lint e verificações de tipos, apresentando os resultados no novo formato. O QA reuniu esses resultados com a revisão dos requisitos e da cobertura de testes antes da PR.

Os testes adicionados devem resolver lacunas reais; uma execução de QA que não precise adicionar testes também pode estar correta. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas depois de qualquer alteração.

## Práticas recomendadas

O contexto e as ferramentas disponibilizados ao Copilot influenciam o trabalho dele. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações em outras conversas e ajuste-as conforme as necessidades da equipe. As instruções definem padrões, as skills descrevem tarefas repetíveis, os agentes personalizados definem funções especializadas e os servidores MCP conectam ferramentas externas. Revise as alterações e os resultados reais das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** de acordo com a tarefa. Use **Plan** para definir uma abordagem antes da implementação, **Interactive** para acompanhar alterações focadas e **Autopilot** para tarefas bem delimitadas. Escolha um modelo mais rápido para alterações rotineiras e um modelo mais avançado para tarefas complexas.

O contexto continua sendo tão importante quanto a infraestrutura. Descrever com clareza *o que* você quer criar, *por que* e *como* influencia significativamente o resultado.

## Mais recursos para explorar

Você percorreu o fluxo de trabalho principal. Veja outros recursos do CLI:

- `/review` para pedir ao agente de revisão de código que analise as alterações.
- `/rubber-duck` para conversar sobre um problema e obter outra perspectiva.
- `/fleet` para coordenar subtarefas independentes em paralelo.
- `/worktree` para isolar uma tarefa separada.
- `/delegate` para enviar uma tarefa ao agente do Copilot na nuvem.

## Próximos passos

A melhor maneira de melhorar com qualquer ferramenta é continuar usando-a. Use-a em código de produção, em projetos pessoais e naquele pequeno aplicativo que você pensa em criar há anos. Compartilhe o que aprendeu com sua equipe e aprenda com os colegas. E, como sempre, explore a documentação.

Se quiser ampliar o Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente do Copilot na nuvem][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar solicitações de pull com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
