---
title: "Exercício 3 - Testar sua funcionalidade com o servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtragem com o modo de agente. Antes de abrir uma solicitação de pull (PR), confirme que ela funciona no navegador. Em vez de testar o aplicativo manualmente, conecte o **servidor Playwright, baseado no Model Context Protocol (MCP)**, e deixe o Copilot controlar um navegador real — depois, publique sua branch e abra a PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP estendem as ferramentas disponíveis para o Copilot.
- adicionar o servidor Playwright MCP ao workspace.
- pedir ao Copilot que use o servidor para testar manualmente a filtragem no navegador.
- publicar sua branch e abrir uma solicitação de pull com as alterações da filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica muito mais útil quando pode acessar recursos além do editor. O Model Context Protocol (MCP) permite isso: é um padrão de comunicação entre o Copilot e ferramentas ou serviços externos.

![Diagrama do funcionamento interno do modo de agente e de sua interação com o contexto, o modelo de linguagem de grande porte (LLM) e as ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos. Assim, os agentes podem acessar informações atualizadas (por meio de recursos) e executar ações em seu nome (por meio de ferramentas).

As ferramentas e os recursos são acessados por meio de um servidor MCP, que funciona como uma ponte entre o agente de IA e ferramentas e serviços externos. O servidor gerencia a comunicação entre o agente e as ferramentas externas, como interfaces de programação de aplicações (APIs) existentes ou ferramentas locais, por exemplo, pacotes npm. Cada servidor MCP representa um conjunto de ferramentas e recursos que o agente pode acessar.

Dois servidores MCP bastante conhecidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: oferece acesso a interfaces de programação de aplicações (APIs) para gerenciar repositórios do GitHub, criar repositórios, atualizar os existentes e gerenciar issues e solicitações de pull.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: oferece recursos de automação de navegador com Playwright, como navegar em páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP disponíveis, com acesso a diferentes ferramentas e recursos. O GitHub mantém um [registro do MCP](https://github.com/mcp) para facilitar a descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Por segurança, trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um, revise cuidadosamente o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use somente servidores MCP em que confia e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Você verá um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Essa entrada `github` vem do modelo do projeto, por isso o Copilot conseguiu ler o backlog de issues no exercício anterior. Ela usa o [servidor GitHub MCP remoto][remote-github-mcp-server], portanto não é necessário instalar nada localmente. O VS Code conecta-se a ele por HTTP e você autentica-se no GitHub na primeira vez que o Copilot usa uma de suas ferramentas.

## Adicionar o servidor Playwright MCP

Agora, você adicionará um segundo servidor. O [servidor Playwright MCP][playwright-mcp-server] oferece ao Copilot um navegador que pode controlar, exatamente o que é necessário para testar a funcionalidade.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github` para que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Diferentemente do servidor remoto `github`, o `playwright` é local: o VS Code o inicia na sua máquina executando `npx @playwright/mcp@latest`. A opção `--headless` instrui o Playwright a executar o navegador sem janela visível, o que é necessário em um codespace sem área de trabalho gráfica.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright nos testes de ponta a ponta, portanto o navegador necessário normalmente já está instalado. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia os servidores MCP **sob demanda**: quando o Copilot precisa usar as ferramentas de um servidor pela primeira vez, o VS Code inicia esse servidor e pede que você confirme se confia nele.

1. Em `.vscode/mcp.json`, aparece uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor e confirmar que a configuração está correta.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e confirme que confia nele para que possa ser iniciado.

Iniciá-lo manualmente é opcional. Se você não fizer isso, o VS Code iniciará o servidor e pedirá que você confirme se confia nele quando o Copilot precisar usar as ferramentas na próxima etapa.

## Testar a funcionalidade de filtragem

O [servidor Playwright MCP][playwright-mcp-server] dá ao Copilot acesso a um navegador real. Em vez de testar o aplicativo manualmente, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou. É uma maneira rápida de confirmar o comportamento da funcionalidade sem sair da conversa.

O servidor Playwright MCP usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. Assim, o agente trabalha com elementos estruturados e identificados, como botões, links e itens de lista, da mesma maneira que a tecnologia assistiva. Portanto, uma verificação funcional rápida também serve como uma validação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste a filtragem:

```text
Usando o servidor Playwright MCP, abra um navegador para o aplicativo em execução em http://localhost:4321 e verifique a nova funcionalidade de filtragem de jogos:

1. Acesse a página de jogos e informe quantos jogos estão listados.
2. Aplique um filtro de categoria e confirme que a lista mostra somente jogos dessa categoria.
3. Limpe o filtro, aplique um filtro de editora e confirme que a lista mostra somente os jogos dessa editora.
4. Combine um filtro de categoria e um de editora e confirme que os resultados respeitam ambos.

Informe o que observou em cada etapa e destaque qualquer comportamento inesperado.
```

O Copilot iniciará um navegador pelo servidor Playwright MCP, percorrerá cada etapa e apresentará as observações. Compare o resumo com os critérios de aceitação da issue. Se algo estiver errado, faça perguntas adicionais ou peça que o Copilot corrija o código antes de abrir uma solicitação de pull.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para este teste. Se você parou o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor Playwright MCP, talvez seja necessário baixar um navegador. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar uma solicitação de pull

Depois de confirmar que a funcionalidade funciona, abra uma solicitação de pull (PR) para que a equipe possa revisá-la. Primeiro, publique a branch `filtering-vscode`.

1. Acesse o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control mostrando as alterações](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar a solicitação de pull

Há várias maneiras de criar uma solicitação de pull, inclusive pelo github.com e pela interface de linha de comando do GitHub (CLI). Como você já está usando o GitHub Copilot, vamos pedir a ele que crie a PR. O Copilot pode localizar a issue relacionada e associá-la à PR.

1. Acesse o painel Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Confirme que **Agent** está selecionado no menu suspenso de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie uma PR:

   ```plaintext
   Encontre no repositório a issue relacionada à filtragem por categoria e editora. Crie uma solicitação de pull para a branch atual e associe-a à issue correta.
   ```

4. Se solicitado, selecione **Continue** para permitir que o Copilot reúna informações e execute operações. Na primeira vez que usar uma ferramenta do GitHub, talvez você precise autenticar-se; siga as instruções.
5. Observe como o Copilot pesquisa as issues, encontra a correta e cria a PR.
6. Selecione o link fornecido pelo Copilot para revisar a solicitação de pull, mas **não faça merge dela ainda**.

## Resumo e próximos passos

Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP estendem as ferramentas disponíveis para o Copilot.
- adicionou o servidor Playwright MCP ao workspace.
- usou o servidor para testar manualmente a filtragem no navegador antes de publicá-la.
- publicou sua branch e abriu uma solicitação de pull com as alterações da filtragem.

Você usou um servidor MCP para *testar* uma funcionalidade, mas o MCP é apenas uma das maneiras de estender os recursos do Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para simplificar tarefas focadas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor Playwright MCP da Microsoft][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [Registro do GitHub MCP][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
