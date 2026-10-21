---
title: "Lição 8 - Criar e fazer merge da solicitação de pull da funcionalidade"
description: "Revise a filtragem, as instruções, a atualização da skill, o perfil de garantia da qualidade e os testes; depois, crie uma solicitação de pull e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia da qualidade (QA) e os testes estão salvos em uma única branch. Agora, revise-os em conjunto e abra uma solicitação de pull (PR). Você mesmo fez merge da PR de avaliação por estrelas; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos a funcionalidade, as atualizações das instruções, a atualização da skill e o agente de QA em algumas PRs separadas. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma conversa e branch, e incluiremos todo esse trabalho nesta PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionar a alteração completa da funcionalidade e as evidências de verificação.
- criar a PR de filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar que o merge da PR foi feito.

## Cenário

Durante o fluxo de trabalho da filtragem, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora, a Tailspin Toys quer automatizar o trabalho restante da PR, mantendo o controle da autorização de merge nas mãos do desenvolvedor.

## Introdução ao Agent Merge

O **Agent Merge** automatiza o trabalho restante para concluir uma solicitação de pull. Quando você o habilita, o Copilot resolve os itens que estão bloqueando a PR — corrige falhas nas verificações de integração contínua (CI), responde a comentários de revisão e faz rebase quando necessário — e, em seguida, habilita o merge automático do GitHub se as configurações do repositório permitirem.

Até agora, você mesmo selecionou **Merge pull request**. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que está pronto antes de habilitar o Agent Merge.

## Usar o Agent Merge para gerenciar a PR

Com todo o código pronto, vamos revisar as alterações em conjunto, criar a PR e permitir que o Agent Merge gerencie o restante do processo.

1. Volte ao seu Codespace.
2. Abra a caixa de diálogo do agente digitando `/agent`.
3. Selecione **Default** na lista de opções e pressione <kbd>Enter</kbd>.
4. Crie uma nova PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua da PR. Quando tudo for aprovado, ele fará o merge.
7. Confirme que aparece uma mensagem do Copilot semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de merge, configurações do repositório nem permissões ausentes. Se houver um bloqueio, leia o motivo informado e faça manualmente o merge revisado quando o repositório permitir.

## Resumo e próximos passos

Você automatizou várias etapas do processo de desenvolvimento, incluindo a geração, os testes e a validação do código e, agora, o processo da solicitação de pull. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionou a alteração completa da funcionalidade e as evidências de verificação.
- criou a PR de filtragem.
- habilitou o Agent Merge somente depois da revisão e confirmou que o merge da PR foi feito.

Em seguida, você vai [explorar outros comandos de barra úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional ao agente na nuvem.

## Recursos

- [Gerenciar solicitações de pull com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
