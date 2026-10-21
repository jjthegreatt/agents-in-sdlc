---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente do Copilot na nuvem que assumisse três tarefas distintas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente na nuvem trabalhe de forma assíncrona e autônoma, é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente na nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel de controle, você pode ver todas as tarefas de agentes que têm solicitações de pull (PRs) abertas. Também pode explorar as operações realizadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para monitorar as tarefas do agente na nuvem.
- orientar uma sessão em andamento para solicitar uma funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação de um modo de alto contraste, a equipe decidiu que também seria um bom momento para adicionar um modo claro. Como já havia trabalho em andamento para atualizar o estilo do site e adicionar a alternância de temas, pareceu lógico incluir essa funcionalidade. Você orientará o agente para que ele adicione o modo claro além do modo de alto contraste.

## Revisar as tarefas do agente do Copilot na nuvem

Vamos verificar o estado atual de todas as tarefas atribuídas ao agente do Copilot na nuvem.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma das tarefas em andamento. Revise as operações realizadas pelo Copilot, que podem incluir:
   - clonar o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores do Model Context Protocol (MCP).
   - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exibidas variam conforme o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel de solicitações de pull (PRs), que aparece à direita. Ele permite acompanhar a PR e os arquivos alterados.

## Orientar o agente na nuvem

Agora que você viu as tarefas ativas, vamos pedir ao Copilot que inclua a alternância para o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente à adição do modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no painel de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração foi concluída e o trabalho começou. Você saberá que isso aconteceu quando aparecerem mensagens semelhantes às mostradas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione este prompt:

    ```plaintext
    Enquanto trabalhamos no modo de alto contraste, vamos adicionar também um modo claro. Deve haver um seletor para esse modo, para que os usuários escolham a opção de exibição desejada.
    ```

    ![Captura de tela da tarefa do agente na nuvem na página de agentes, com a caixa de diálogo Steer active session while copilot is working destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe que o Copilot reconhece o prompt e o inclui no fluxo de trabalho.

## Deixar o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incluirá a nova solicitação no fluxo depois de concluir a etapa que estava executando quando você enviou a mensagem.

Como esse processo também levará alguns minutos, aproveite para fazer uma pausa e refletir sobre o que já aprendeu e explorou.

## Resumo e próximos passos

Este exercício explorou a página de agentes do Copilot, o ponto central para monitorar e orientar as tarefas do agente do GitHub Copilot na nuvem. Nesse painel de controle, você acompanha tarefas ativas e concluídas, revisa o trabalho e redireciona tarefas em andamento para ajustar o escopo ou fornecer orientações.

Você explorou estes conceitos:

- explorou o painel de controle e a página de agentes para monitorar as tarefas do agente na nuvem.
- redirecionou uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar nas solicitações de pull criadas pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre os agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
