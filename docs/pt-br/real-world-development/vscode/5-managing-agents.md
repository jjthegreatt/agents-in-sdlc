---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você coloca o GitHub Copilot no modo de agente, ele trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real na sua máquina, o Copilot Chat no VS Code mostra as chamadas de ferramentas e as edições nos arquivos à medida que ocorrem. Você pode revisar cada diff, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts adicionais para aperfeiçoar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- monitorar o trabalho do agente de acessibilidade em tempo real.
- revisar e aceitar as edições propostas nos arquivos.
- orientar o agente durante a sessão para adicionar uma alternância do modo claro junto com o modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho continua, a equipe de design faz outra solicitação: também quer uma opção de modo claro para os usuários que consideram o tema padrão escuro demais. Em vez de iniciar uma conversa nova, você orientará a sessão atual para ampliar o trabalho.

## Monitorar o progresso do Copilot

A visualização Copilot Chat mostra cada etapa executada pelo agente: os arquivos explorados, os comandos executados e as edições propostas. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa para acompanhar as chamadas de ferramentas. Você verá leituras e edições de arquivos e os comandos de terminal que ele pedir para executar.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para desfazer a alteração e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior da conversa para ver todos os arquivos modificados pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão, por exemplo, para executar um comando de terminal, o agente pausará e exibirá uma solicitação **Allow**. Aprove somente os comandos que considerar seguros.

5. Aguarde o agente concluir a implementação do modo de alto contraste. Ele deverá apresentar uma mensagem final que resume as alterações.

## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, amplie a mesma conversa para adicionar uma alternância do modo claro. Como você continua na mesma sessão de chat, o agente mantém o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt de acompanhamento:

    ```plaintext
    Bom trabalho! Agora adicione uma alternância para o modo claro ao lado da opção de alto contraste. O modo claro deve seguir o mesmo padrão: persistir no armazenamento local e estar disponível na mesma área da interface do usuário (UI). O usuário deve poder ativar o alto contraste e o modo claro de forma independente.
    ```

2. O agente continuará de onde parou, explorará as alterações existentes e proporá o que será necessário para o modo claro.
3. Revise cada nova edição à medida que aparecer, usando **Keep** ou **Undo** no diff.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a UI das alternâncias de uma forma que você não quer —, interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem de acompanhamento para esclarecer o pedido.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts de acompanhamento para pedir ajustes ("simplifique o CSS usando variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione uma terceira alternância; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione também atalhos de teclado para as duas alternâncias").

## Revisar os arquivos alterados

Antes de fazer commit, revise rapidamente tudo que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações em componentes Astro, estilos e testes relacionados.
3. Abra alguns dos arquivos alterados e examine os diffs. Confirme que a implementação segue os padrões de acessibilidade do agente personalizado: atributos da especificação Accessible Rich Internet Applications (ARIA), navegação por teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações pelo painel Source Control. Você as publicará [na próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para monitorar e orientar uma sessão em andamento: revisou as edições à medida que apareciam, aceitou ou rejeitou alterações e enviou prompts de acompanhamento para ampliar o trabalho sem perder o contexto.

Nesta lição, você:

- monitorou o trabalho do agente em tempo real.
- aceitou e rejeitou edições individuais nos arquivos.
- orientou uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em uma solicitação de pull e revisará as alterações produzidas pelo agente personalizado local durante este laboratório.

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
