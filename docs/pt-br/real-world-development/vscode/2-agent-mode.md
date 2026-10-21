---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo costumam exigir alterações em vários arquivos e operações como executar testes. O fluxo de trabalho típico de um desenvolvedor envolve localizar todos os arquivos necessários, fazer alterações, executar testes, depurar, identificar o arquivo esquecido, fazer outra alteração e assim por diante.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para atuar com mais autonomia no IDE. Ele se comporta de maneira semelhante a um desenvolvedor: começa explorando a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o modo de agente para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente na nuvem oferecem experiências diferentes. O modo de agente executa tarefas no IDE, permitindo ciclos rápidos de feedback e interação. O agente na nuvem atua como um colega de programação, trabalha de forma assíncrona e interage com você por meio de issues e solicitações de pull.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e fazer alterações coordenadas.
- o modo de agente do GitHub Copilot pode implementar funcionalidades na interface do usuário (UI) e na camada de dados.
- revisar as alterações e os testes gerados pelo modo de agente do Copilot antes de integrá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que os usuários filtrem por categoria e editora. No exercício anterior, você adicionou um helper de editoras. Agora, concluirá o restante do trabalho na camada de dados, na UI e nos testes com o modo de agente do Copilot.

## Executar o site Tailspin Toys

Antes de fazer alterações, explore o site Tailspin Toys para conhecer os recursos atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com um tema voltado a desenvolvedores. Ele permite listar jogos e exibir detalhes. O site é um único aplicativo Astro que gera páginas HTML estáticas durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando Drizzle ORM: não há uma API de backend separada nem um framework de UI do lado do cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é implementada com um pequeno `<script>` Astro com escopo próprio e APIs DOM padrão.

### Iniciar o site

Para facilitar a execução do site, há uma tarefa `npm run dev` que inicia o único servidor de desenvolvimento Astro. Execute-a no GitHub Codespace seguindo estas etapas:

1. Volte ao codespace. Você deve estar na branch `main` depois do exercício anterior.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o comando a seguir para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá um aviso com a URL, semelhante a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo perguntando se você quer abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando o **x**.

5. Abra o site mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionada e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, ao selecionar o link localhost no terminal, você será redirecionado para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o codespace, que agora hospeda o servidor web.

### Explorar o site

Com o site em execução, explore seus recursos principais:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você acessa uma página de detalhes com mais informações, incluindo título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de iniciar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para começar uma conversa limpa. A seleção de modo e modelo varia por exercício; cada etapa indicará as opções relevantes.

A implementação inicial do site funciona, mas queremos adicionar novos recursos. Vamos começar revisando o backlog. Ao configurar o projeto a partir do modelo, foi criado automaticamente um backlog de issues. Peça ao GitHub Copilot que mostre esses itens.

1. Selecione **Agent** no menu suspenso de agentes da visualização Chat. O agente selecionado de forma autônoma planeja e implementa alterações em vários arquivos, executa comandos de terminal e chama ferramentas.

   ![Captura de tela do seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão indicando preferência por um modelo específico. Durante a criação do workshop, usamos Claude Sonnet 4.5 e o mencionamos para que as sugestões de código fossem relativamente consistentes e proporcionassem uma boa experiência. No entanto, como os modelos de linguagem de grande porte (LLMs) são probabilísticos, as sugestões podem ser diferentes das descritas aqui. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar outro comando MCP, mas ainda deve conseguir concluir a tarefa.

3. Envie o prompt a seguir para pedir ao Copilot que mostre o backlog de issues:

   ```plaintext
   Mostre o backlog de itens do meu repositório GitHub. Ajude-me a priorizá-los com base nos que serão mais úteis para os usuários.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista gerada.

Observe que o Copilot também priorizou os itens de acordo com o que considera mais útil para os usuários.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere o código, revise o arquivo de instruções que fornecerá contexto para o trabalho do Copilot. Você usará o arquivo de instruções da [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que descreve como abordar a adição de funcionalidades ao site.

1. No codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para arquivos de instruções específicos de tipos de arquivo, incluindo Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Assim, é possível reutilizar orientações, dividir conjuntos maiores em partes menores e apontar exemplos e modelos para o Copilot.

## Implementar a funcionalidade de filtragem

Para concluir a filtragem, são necessárias pelo menos três alterações distintas no aplicativo:

- adicionar ou aperfeiçoar a lógica de filtragem restante na camada de dados (`src/lib/`).
- adicionar ou atualizar testes para o comportamento da filtragem.
- atualizar a página de listagem de jogos para incluir a UI de filtragem.

Além disso, os testes precisam ser executados e aprovados antes do merge. O modo de agente do Copilot pode executar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue na conversa atual do Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** para adicionar o arquivo de instruções.

   ![Captura de tela da seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado no menu suspenso de agentes da visualização Chat.

   ![Captura de tela do seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue relacionada no backlog:

   ```plaintext
   Atualize o site para incluir filtros por editora e categoria de acordo com os requisitos da issue relacionada no backlog do GitHub. Já existe um helper de editoras do exercício anterior; preserve e aperfeiçoe esse trabalho conforme necessário ao concluir as alterações restantes na camada de dados, na UI e nos testes. Confirme que todos os testes foram aprovados antes de concluir. O servidor já está em execução, portanto não é necessário iniciá-lo.
   ```

6. Acompanhe o Copilot enquanto ele explora o projeto e localiza os arquivos associados à funcionalidade, incluindo os helpers da camada de dados, a UI e os testes. Em seguida, ele começa a alterar os arquivos e executar os testes.

   ![Captura de tela do Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, alterar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, você verá a opção **Keep** ou **Undo** para todas as alterações; não é necessário selecioná-las enquanto o trabalho estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela da caixa de diálogo do Copilot Chat solicitando confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez ocorram pausas e alguns testes falhem durante o processo. Isso é normal. O Copilot alterna entre gerar código e executar testes até concluir a tarefa sem detectar erros.

   ![Captura de tela de uma sessão concluída do Copilot Chat no modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado e procure possíveis problemas.

> [!CAUTION]
> É importante revisar sempre o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Neste exercício, exploramos como usar o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Aprendemos como:

- o modo de agente do GitHub Copilot pode implementar funcionalidades na UI e na camada de dados.
- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e fazer alterações coordenadas.
- revisar as alterações e os testes gerados pelo modo de agente do Copilot antes de integrá-los à base de código.

Agora, vamos [testar sua funcionalidade com o servidor Playwright MCP][next-lesson] e abrir uma solicitação de pull.

### Exercício de exploração bônus — Implementar paginação

À medida que a lista de jogos cresce, será necessário adicionar paginação. Com as habilidades deste exercício, peça ao Copilot que atualize o site para implementar a paginação. Considere:

- seguir as práticas recomendadas, inclusive usar os arquivos de instruções existentes.
- decidir como implementar a paginação: permitir que o usuário escolha o tamanho da página ou defini-lo de forma fixa.
- incluir no prompt as orientações necessárias para que o Copilot implemente o comportamento desejado.
- iterar com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
