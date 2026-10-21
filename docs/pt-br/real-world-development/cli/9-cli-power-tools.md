---
title: "Lição 9 - Comandos de barra do GitHub Copilot CLI"
description: "Explore comandos de barra para gerenciar o contexto, selecionar modelos, compartilhar sessões e delegar tarefas à nuvem opcionalmente."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como toda boa ferramenta de interface de linha de comando (CLI), o GitHub Copilot CLI inclui vários comandos de barra para interagir com ele. Esses comandos dão acesso a recursos avançados, informações internas e opções de configuração adicionais. Você já usou comandos como `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Vamos explorar alguns outros comandos úteis.

Nesta lição, você vai:

- usar `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa.
- usar `/model` para explorar os modelos disponíveis.
- aprender como `/share` pode exportar ou compartilhar uma sessão.
- explorar comandos opcionais para trabalho paralelo, worktrees e delegação ao agente na nuvem.

## Cenário

Você concluiu o fluxo de trabalho principal do CLI. Agora, vamos conhecer mais alguns recursos: gerenciamento de contexto, troca de modelos, compartilhamento de sessões e delegação opcional de tarefas ao [agente do Copilot na nuvem][about-cloud-agent].

## Explorar o contexto do Copilot CLI

Em tarefas maiores ou mais complexas, você pode atingir o limite da janela de contexto do modelo. O Copilot CLI compacta a conversa automaticamente quando necessário, mas você também pode inspecionar ou compactar o contexto com comandos de barra.

1. Volte ao Codespace e inicie o Copilot CLI na raiz do repositório, caso ele ainda não esteja aberto.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e a divisão do contexto entre instruções do sistema, ferramentas, mensagens e espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare o resultado. A diferença pode ser pequena se a conversa já for curta.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente quando a janela fica cheia. Use `/compact` para escolher quando fazer isso. Use `/clear` ou `/new` ao mudar para uma tarefa sem relação com a atual e iniciar uma conversa nova.

## Escolher um modelo

Modelos diferentes têm pontos fortes diferentes, e os desenvolvedores podem ter preferências distintas. O Copilot CLI permite listar e selecionar o modelo que você quer usar.

1. Digite:

   ```plaintext
   /model
   ```

2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhar uma sessão

Trabalhar em equipe e compartilhar aprendizados ajuda todos a aproveitar melhor as ferramentas de IA. O comando `/share` pode exportar uma sessão para um arquivo Markdown ou HTML, criar um link compartilhável ou publicar um GitHub gist.

1. Digite `/help` e revise as opções de `/share` disponíveis na versão instalada.
2. Se quiser compartilhar a sessão, escolha o destino adequado, como `/share file` para exportá-la como arquivo Markdown local.
3. Revise o conteúdo exportado antes de enviá-lo a alguém ou publicá-lo. As exportações da sessão podem incluir prompts, respostas e detalhes do projeto.

A publicação de um link ou gist é opcional. Não publique conteúdo do repositório ou da conversa que sua equipe não pretenda compartilhar.

## Opcional: ampliar ou delegar o trabalho

O fluxo principal do workshop terminou. O Copilot CLI também oferece comandos para tarefas maiores:

- `/fleet` pode dividir subtarefas independentes entre subagentes e executá-las em paralelo.
- `/worktree` pode criar uma worktree Git isolada para uma tarefa separada.
- `/delegate` pode enviar uma tarefa ao agente do Copilot na nuvem, que trabalha de forma assíncrona e pode abrir uma solicitação de pull.

Esses comandos são opcionais, pois podem criar worktrees adicionais ou executar trabalho remoto. Comece uma tarefa nova e bem delimitada antes de experimentá-los e revise o resultado usando seu fluxo habitual. Para saber mais sobre o trabalho assíncrono com agentes, continue com o [workshop sobre agentes na nuvem][cloud-workshop].

## Resumo e próximos passos

Os comandos de barra do Copilot CLI permitem configurar a ferramenta, compartilhar sessões e observar o que acontece nos bastidores. Nesta lição, você:

- usou `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa.
- usou `/model` para explorar os modelos disponíveis.
- aprendeu como `/share` pode exportar ou compartilhar uma sessão.
- explorou comandos opcionais para trabalho paralelo, worktrees e delegação ao agente na nuvem.

Há muitos outros comandos de barra para explorar no Copilot CLI! Vamos encerrar nossa jornada [revisando o que aprendemos][next-lesson] e vendo os próximos passos.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o agente do Copilot na nuvem][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
