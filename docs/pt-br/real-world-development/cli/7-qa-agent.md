---
title: "Lição 7 - Criar e usar um agente de garantia da qualidade (QA)"
description: "Crie um agente personalizado de garantia da qualidade (QA) que reúna os requisitos da issue, a skill quality-checks e o Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o servidor Playwright, baseado no Model Context Protocol (MCP), para observar a experiência de filtragem no navegador. Agora, você reunirá esses recursos em um agente personalizado com um processo de QA bem definido.

Nesta lição, você vai:

- explorar como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criar e inspecionar um perfil reutilizável de garantia da qualidade.
- selecionar o agente de QA e revisar as descobertas com base na issue de filtragem.

## Cenário

A Tailspin Toys quer revisar de forma consistente os requisitos, a qualidade do código, as verificações automatizadas, a cobertura de testes e o comportamento no navegador antes de abrir uma solicitação de pull (PR). Um agente personalizado pode coordenar esse processo de garantia da qualidade (QA) e gerar um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot, definida em um perfil Markdown. O perfil descreve a finalidade, as instruções e as ferramentas disponíveis para o agente. Neste workshop, você definirá a função de garantia da qualidade (QA) em `.github/agents/qa.agent.md` e a selecionará no Copilot CLI, a interface de linha de comando (CLI) do GitHub Copilot.

As personalizações usadas até agora têm funções diferentes. As instruções do repositório descrevem os padrões da equipe. A skill `quality-checks` reúne verificações repetíveis. O Playwright MCP fornece ferramentas para o navegador. O perfil de QA instrui o Copilot a usar esses recursos para avaliar os requisitos e apresentar as descobertas. Ele não substitui os outros recursos nem exige outra conversa.

## Criar o perfil de QA

Antes de abrir a PR da funcionalidade, peça ao Copilot que crie um perfil reutilizável de QA. O perfil definirá as verificações e os limites que o agente deve respeitar.

1. Volte ao seu Codespace e confirme que a conversa sobre filtragem está no modo Interactive.
2. Peça ao agente padrão que crie o novo agente personalizado:

   ```plaintext
   Crie um agente personalizado chamado QA em .github/agents/qa.agent.md. Ele deve verificar as funcionalidades com base nas issues e nos requisitos acordados, seguir as instruções do repositório, executar a skill quality-checks, usar o Playwright MCP para verificar o comportamento e adicionar testes quando houver lacunas de cobertura.

   Peça que informe cada requisito como aprovado, reprovado ou bloqueado, com evidências. O agente deve pedir autorização antes de alterar o código de implementação e não pode fazer commits nem abrir solicitações de pull.

   Por enquanto, apenas crie o perfil para que eu possa revisá-lo.
   ```

## Inspecionar o perfil

Antes de usar o novo agente, revise o perfil para confirmar que o Copilot registrou o fluxo de QA e os limites pretendidos. Assim, você evita que um perfil incompleto ou amplo demais altere a funcionalidade quando você queria apenas verificá-la.

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. `description` é obrigatório; `name` é opcional, mas dá ao agente um nome de exibição claro.
3. Leia as instruções do perfil e confirme que o agente começa pelos requisitos, segue as instruções do repositório, executa a skill `quality-checks` e usa o Playwright MCP.
4. Confirme que o agente apresenta evidências, pede autorização antes de alterar o código de implementação e não faz commits nem abre solicitações de pull.
5. Se o perfil gerado não incluir alguma dessas responsabilidades ou limites, peça ao agente padrão que o revise antes de continuar.

## Executar o QA com base na issue

O Copilot CLI carrega os agentes do projeto quando uma conversa começa. Depois de criar o perfil, retome a mesma conversa sobre filtragem e selecione o agente de QA para que ele possa usar a issue e as decisões de planejamento já disponíveis no contexto.

1. Ative o agente com este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como você acabou de criar o agente, talvez ele ainda não apareça na lista de agentes disponíveis. Mesmo assim, o comando acima o ativará.

2. Use este prompt para pedir ao agente de QA que revise a funcionalidade:

   ```plaintext
   Revise a funcionalidade de filtragem com base na issue e nas decisões do nosso plano. Ela está pronta para uma PR?
   ```

3. Aguarde enquanto o agente de QA trabalha.
4. Quando terminar, leia o relatório.

## Resumo e próximos passos

Você adicionou uma função especializada reutilizável ao fluxo de trabalho e revisou o resultado. Nesta lição, você:

- explorou como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criou e inspecionou um perfil reutilizável de garantia da qualidade.
- selecionou o agente de QA e revisou as descobertas com base na issue de filtragem.

Agora, a implementação, a atualização da skill, o perfil de QA, os testes e o relatório de verificação estão prontos para revisão. Em seguida, você vai [reunir esses itens em uma PR de funcionalidade e usar o Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
