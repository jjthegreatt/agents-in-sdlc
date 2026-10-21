---
title: "Exercício 2 - Agente do GitHub Copilot na nuvem"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações, se é que alguma, não enfrentem problemas de dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de funcionalidade que ficaram esquecidas no backlog por falta de tempo para implementá-las. O agente do GitHub Copilot na nuvem foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria uma solicitação de pull (PR) de rascunho para que um desenvolvedor revise o trabalho. Assim, você pode delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em questões mais amplas.

Neste exercício, você explorará os seguintes tópicos com o agente do Copilot na nuvem:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de issues bem delimitadas.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem dívidas técnicas que gostaria de resolver. Os contratados que criaram a primeira versão do site deixaram a documentação em um estado insatisfatório: ela está completamente ausente. Como primeira etapa, a equipe quer adicionar comentários TSDoc a todas as funções exportadas no aplicativo.

Além disso, a equipe de design está pronta para melhorar a descoberta de jogos. Ela quer que cada página de detalhes de jogo exiba jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem explorando. Ainda não é necessário ter uma implementação polida; a equipe quer apenas algo que possa usar para testar a experiência. Essa tarefa é um bloqueio, mas há outras issues de prioridade maior no momento.

Esses são exemplos de tarefas que podem ser adiadas e que são boas candidatas para o agente do Copilot na nuvem. Ele pode trabalhar nelas de forma assíncrona enquanto o desenvolvedor se dedica a outras tarefas e, depois, revisar o resultado.

## Introdução ao agente do GitHub Copilot na nuvem

O [agente do GitHub Copilot na nuvem](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano de forma semelhante a um desenvolvedor. Assim como acontece ao trabalhar com uma pessoa desenvolvedora, há várias maneiras de delegar tarefas, inclusive [atribuindo uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria uma solicitação de pull de rascunho para acompanhar o progresso, configura o ambiente e começa a trabalhar na tarefa. Você pode inspecionar a sessão do Copilot enquanto ela estiver em andamento ou depois de concluída. Quando a solução proposta estiver pronta para revisão, o Copilot mencionará você na solicitação de pull (PR).

## A importância de instruções bem delimitadas

Não há mágica no GitHub Copilot: não é possível deixar de pensar na tarefa. Até operações aparentemente simples ficam complexas quando analisadas em detalhes. Portanto, [defina com cuidado o escopo das tarefas para o agente do Copilot na nuvem][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe em etapas, aprenda, experimente e adapte-se. Os fundamentos do desenvolvimento de software não mudam com a adição de IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram as instruções personalizadas e como elas orientam o Copilot. Se você já concluiu a atividade prática sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação. Caso contrário, aproveite para dar uma olhada rápida. Antes de atribuir trabalho ao agente do Copilot na nuvem, consulte os arquivos de instruções incluídos no repositório sem alterá-los, para que possa observar o efeito deles mais tarde.

Abra os arquivos a seguir na interface web do GitHub ou em um codespace, caso já tenha iniciado um:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, especialmente as orientações sobre convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Quando atribuir a issue `Code lacks documentation` ao agente na nuvem na próxima seção, observe os comentários TSDoc, as convenções TypeScript e os cabeçalhos de comentário na PR resultante. Esses elementos são definidos por esses arquivos de instruções. Voltaremos a esse assunto ao revisar as PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente do Copilot na nuvem

A criação de código normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto — independentemente de quem esteja trabalhando. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de forma semelhante a um engenheiro de software (SWE).

O agente na nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente com um [fluxo de trabalho especial de configuração][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, que é executado antes do início do trabalho. Assim, o agente tem acesso às ferramentas de desenvolvimento e dependências necessárias. Esse fluxo já foi configurado para facilitar o laboratório e permitir esta atividade prática. Ele garante acesso ao Node.js, às dependências do projeto, aos binários de navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e populado do aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório como parte das etapas de configuração, por exemplo para instalar dependências, será necessária a permissão `contents: read`. Se você não clonar o repositório nas etapas, o Copilot fará isso automaticamente quando elas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo - Node.js (agora, todo o aplicativo usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra o banco de dados SQLite local e insere dados para que compilações e testes tenham dados disponíveis.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer outro fluxo de trabalho do GitHub, mas tem alguns detalhes importantes:

- Ele contém uma única tarefa chamada `copilot-setup-steps`. Essa tarefa é executada no GitHub Actions antes de o Copilot começar a trabalhar na solicitação de pull.
- O gatilho `workflow_dispatch` permite executar manualmente o fluxo de trabalho na guia **Actions** do repositório. Isso é útil para testar se o fluxo funciona sem esperar que o Copilot o execute.

## Adicionar documentação

Embora todos entendam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Esse tipo de dívida técnica costuma ficar sem solução, reduzindo a produtividade e dificultando a manutenção da base de código ou a integração de novos integrantes à equipe. Felizmente, o Copilot é eficaz na criação de documentação, e esta é uma issue ideal para atribuir ao agente do Copilot na nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Você revisará o resultado em um exercício futuro.

1. Em uma nova guia do navegador, acesse o repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

    ```plaintext
    Nossa organização exige comentários TSDoc em funções e métodos sempre que forem úteis. Infelizmente, as atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para garantir que os comentários de documentação sejam incluídos quando esclarecerem o comportamento.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra a solicitação de pull (PR) recém-gerada, cujo título será semelhante a `[WIP]: Code lacks documentation`. Se a PR ainda não aparecer na lista, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar alguns minutos para aparecer na PR. O Copilot está configurando o ambiente — executando o fluxo de trabalho mencionado acima —, analisando o projeto e determinando a melhor abordagem.

13. Revise a lista e as tarefas que serão executadas.
14. Percorra a linha do tempo da solicitação de pull e procure uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a página para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot resolve o problema: explora o código, entende o estado, pausa para planejar e tomar decisões e, por fim, cria o código.

O processo pode levar vários minutos. Um dos principais objetivos do agente do Copilot na nuvem é executar tarefas de forma assíncrona, liberando você para trabalhar em outras atividades. Vamos aproveitar esse recurso: atribua outra tarefa ao agente na nuvem e, em seguida, volte sua atenção para a implementação de funcionalidades.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma das grandes vantagens do agente do GitHub Copilot na nuvem é a possibilidade de dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto ele executa outro. Embora a criação de uma seção de jogos relacionados para a equipe de design talvez não leve muito tempo, você poderia usar esse tempo para outras tarefas. Vamos atribuí-la ao agente do Copilot na nuvem.

1. Volte ao repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

    ```markdown
    Queremos ajudar as pessoas a descobrir mais jogos exibindo jogos relacionados na página de detalhes de cada jogo. A equipe de design quer explorar a experiência do usuário (UX) e fazer alguns testes de aceitação. Nossos requisitos são:

   - Adicionar um helper de acesso a dados em `src/lib/` que retorne outros jogos da mesma categoria de um determinado jogo, excluindo esse jogo
   - Exibir uma seção "Related games" na página de detalhes do jogo usando o helper
   - Tratar o caso em que um jogo não tem jogos relacionados
   - Criar testes unitários para o novo helper
   - Antes de criar a PR, confirmar que todos os testes foram aprovados
   ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver uma sequência de 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot está trabalhando na segunda solicitação. O agente na nuvem funciona de modo semelhante a um SWE: você não precisa acompanhá-lo continuamente, apenas revisar o resultado quando terminar. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Este exercício explorou o [agente do GitHub Copilot na nuvem][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para execução assíncrona. Você pode usá-lo para resolver dívidas técnicas, criar funcionalidades ou ajudar na migração de código entre frameworks.

Você explorou os seguintes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de issues bem delimitadas.
- atribuir issues ao Copilot.

Enquanto o agente na nuvem trabalha em segundo plano, vamos criar e usar agentes personalizados. O [agente do Copilot na nuvem também pode usar servidores do Model Context Protocol (MCP)][cloud-agent-mcp] e dispõe de instruções personalizadas, que exploramos em módulos anteriores.

## Recursos

- [Sobre o agente do Copilot na nuvem][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente do Copilot na nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
