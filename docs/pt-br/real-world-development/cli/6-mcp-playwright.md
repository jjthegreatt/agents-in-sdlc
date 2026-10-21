---
title: "Lição 6 - Validar a funcionalidade com o Playwright MCP"
description: "Inspecione ou configure o Playwright MCP no Copilot CLI e use-o para explorar a filtragem no navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código envolve mais do que apenas programar. Precisamos trabalhar com dados e serviços externos e disponibilizar automações adicionais ao Copilot. É aí que entram os servidores do Model Context Protocol (MCP). Eles permitem que o Copilot vá além dos recursos integrados à interface de linha de comando (CLI), oferecendo mais ferramentas e serviços.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa.
- adicionar o servidor Playwright MCP, caso ele ainda não esteja disponível.
- pedir ao agente que controle um navegador e explore a funcionalidade de filtragem.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas validar alterações na UI exige interagir com ela. Você quer permitir que o Copilot use o site como um usuário faria para automatizar ainda mais o processo e aumentar a confiança de que as alterações funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes podem acessar informações atualizadas e executar ações em seu nome.

As ferramentas e os recursos são acessados por meio de um servidor MCP, que funciona como uma ponte entre o agente de IA e as ferramentas e os serviços externos. Cada servidor MCP representa um conjunto de ferramentas e recursos que o agente pode acessar.

Dois servidores MCP bastante conhecidos:

- **[GitHub MCP Server][github-mcp]**: oferece acesso a APIs para gerenciar repositórios, issues e solicitações de pull do GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: oferece recursos de automação de navegador com Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub mantém um [registro do MCP][mcp-registry] para facilitar a descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um, revise o código-fonte, verifique quem o publicou e considere as implicações de segurança.

## Adicionar o servidor Playwright MCP

Se o servidor Playwright MCP ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como um usuário faria.

> [!NOTE]
> Se o Playwright MCP já estiver configurado no Copilot CLI, pule as etapas de configuração e continue na próxima seção.

1. Volte ao seu Codespace.
2. Abra a caixa de diálogo para adicionar um servidor MCP digitando este comando no Copilot CLI:

   ```plaintext
   /mcp add
   ```

3. Digite `playwright` como nome e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o texto a seguir na caixa de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo MCP.

## Pedir ao Copilot que explore a funcionalidade com Playwright

Antes, você confirmou manualmente o comportamento da funcionalidade. Agora, vamos usar o servidor Playwright MCP disponível no Copilot para fazer a mesma verificação.

1. Use este prompt para pedir ao Copilot que valide a funcionalidade com o servidor Playwright MCP:

   ```plaintext
   Inicie o aplicativo e use o Playwright MCP para verificar a filtragem com base na issue e no nosso plano. Informe o que funciona e o que não funciona, sem fazer alterações. Pare o servidor iniciado quando terminar.
   ```

> [!NOTE]
> Não é necessário dizer ao Copilot que use o servidor MCP, pois ele normalmente identifica a ferramenta apropriada. Mas, como você sabe qual ferramenta deve ser usada, indicar isso pode ajudar a obter resultados mais consistentes e economizar alguns tokens.

2. Observe as etapas que o Copilot executa no navegador para confirmar que a funcionalidade funciona.
3. Leia o relatório e confirme que tudo funciona como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, interromperá o servidor e apresentará um relatório.

## Resumo e próximos passos

Parabéns, você usou o servidor Playwright MCP para explorar sua funcionalidade em um navegador real pelo Copilot CLI. Em resumo, você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa.
- adicionou o servidor Playwright MCP, caso ele ainda não estivesse disponível.
- pediu ao agente que controlasse um navegador e explorasse a funcionalidade de filtragem.

Em seguida, você vai [criar um agente personalizado de QA][next-lesson] que reúne a skill e as ferramentas do navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor Playwright MCP da Microsoft][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
