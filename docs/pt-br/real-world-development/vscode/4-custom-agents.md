---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados para tarefas ou áreas específicas do fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você fornece instruções focadas, práticas recomendadas, padrões de programação e conhecimento do domínio para orientar o Copilot em tipos de trabalho específicos. As equipes podem registrar sua experiência em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica as [Diretrizes de Acessibilidade para Conteúdo da Web (WCAG)][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo contém frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a experiência e as instruções do agente.

### Agentes personalizados e skills de agente

Há alguma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e ensinam a IA a executar operações. A distinção mais simples é: um **agente personalizado** é o responsável pelo trabalho, e as **skills** são as ferramentas.

Os agentes personalizados têm sua própria janela de contexto e podem coordenar skills — e até outros agentes — durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site segundo as diretrizes de acessibilidade; como parte desse trabalho, ele poderia chamar skills, como uma skill de fluxo de trabalho de solicitação de pull ou outra para executar e gerenciar testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Como acontece com qualquer recurso de IA, teste e ajuste a solução para os ambientes e cenários em que será usada.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer garantir que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes de usuários apontaram que algumas pessoas têm dificuldade para ler o tema escuro atual devido ao contraste insuficiente entre o texto e o plano de fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou a implementação de um modo de alto contraste que os usuários possam ativar e desativar.

Como a acessibilidade é essencial, a equipe quer implementar essa funcionalidade o mais rápido possível. Você usará um agente personalizado para gerar a funcionalidade.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar um modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado para você. Revise o conteúdo para entender como ele orientará o Copilot.

Volte ao codespace e, em seguida, abra um terminal e crie uma branch nova a partir de `main` para o trabalho de acessibilidade. Assim, a solicitação de pull (PR) de filtragem do Exercício 3 ficará separada.

1. No terminal, atualize `main` e crie a branch para o trabalho de acessibilidade:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abra `.github/agents/accessibility.md`.
3. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

4. Percorra as seções seguintes, que apresentam:
   - responsabilidades principais na geração de código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - uma lista de problemas e erros comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code mostra todos os agentes personalizados definidos em `.github/agents` no menu suspenso de agentes, na parte inferior da visualização Copilot Chat. Você pode selecionar um agente personalizado para usar as instruções e ferramentas dele na conversa.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de iniciar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para começar uma conversa limpa. A seleção de modo e modelo varia por exercício; cada etapa indicará as opções relevantes.

1. Se **Agent** ainda não estiver selecionado, selecione-o no menu suspenso de agentes da visualização Chat.

   ![Captura de tela do seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione o menu suspenso de agentes na parte inferior da visualização Chat. Ele mostra o agente ativo, que por padrão é **default**.
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie este prompt ao agente de acessibilidade:

    ```plaintext
    Adicione ao site um modo de alto contraste. Deve haver uma opção que o usuário possa ativar para habilitar o alto contraste, e a preferência deve persistir após recarregar a página, usando o armazenamento local do navegador.
    ```

5. O Copilot Chat explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para revisão.
6. À medida que as edições forem feitas, o indicador **Files changed** será atualizado para mostrar os arquivos modificados pelo agente.

> [!NOTE]
> O processo pode levar alguns minutos. O Copilot está fazendo alterações reais no repositório: pode editar componentes Astro, CSS e testes relacionados.

Você revisará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Este exercício explorou os [agentes personalizados][custom-agents] do GitHub Copilot: assistentes de IA especializados para tarefas e domínios específicos. Com agentes personalizados, você pode registrar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o Copilot em tipos de trabalho específicos.

Você explorou estes conceitos:

- como os agentes personalizados são definidos.
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você vai [monitorar e orientar o trabalho do agente][next-lesson], revisar as alterações enquanto são feitas e adicionar uma alternância para o modo claro à mesma sessão.

## Recursos

- [Sobre os agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar sua funcionalidade com o servidor Playwright do Model Context Protocol (MCP)][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
