---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma forma específica — ou há informações de contexto que o Copilot precisa conhecer —, disponibilize-as. Há várias maneiras de compartilhar contexto específico com o Copilot. Uma das principais são os [arquivos de instruções][instruction-files], usados para orientar a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e dos arquivos específicos por caminho.
- gerar a primeira parte dos dados para filtragem (um helper de editoras) com as instruções atuais.
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`.
- reenviar o mesmo prompt e observar o código gerado adotar o novo padrão.
- fazer commit das instruções atualizadas e da parte de filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode divergir de alguns padrões definidos por você. O Copilot é não determinístico. O objetivo deste exercício é observar a tendência de mudança no comportamento após a atualização das instruções, não reproduzir cada caractere do resultado.

## Arquivos de instruções

### Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário (UI) deve usar um tema escuro e ter aparência moderna.
- O código deve ser documentado com comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você fornecerá ao Copilot as informações necessárias para realizar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Você pode especificar convenções de código, bibliotecas e tipos de comentários. É possível criar instruções para todo o repositório ou para tipos específicos de arquivo e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md` é um único arquivo enviado ao Copilot em **todas** as solicitações relacionadas ao repositório. Ele deve conter informações relevantes para a maioria das solicitações, como a pilha de tecnologias, uma visão geral do que está sendo criado, práticas recomendadas e outras orientações gerais.
- Os arquivos `.github/instructions/*.instructions.md` podem ser criados para tarefas ou tipos de arquivo específicos. Eles podem fornecer diretrizes para linguagens, como TypeScript ou Astro, ou para tarefas como criar um componente de interface do usuário (UI) ou testes unitários.

> [!NOTE]
> No ambiente de desenvolvimento integrado (IDE), os arquivos de instruções são usados para gerar código no Copilot Chat, mas não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, a interface de linha de comando (CLI) do Copilot e o agente do Copilot na nuvem usam tanto as instruções do repositório quanto os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma discussão completa sobre a criação de arquivos de instruções está fora do escopo deste workshop. Os exemplos do projeto demonstram uma abordagem representativa. Em linhas gerais:

- Mantenha as instruções em `copilot-instructions.md` focadas em orientações para o projeto, como uma descrição do que está sendo criado, a estrutura do projeto e os padrões gerais de programação.
- Use arquivos `*.instructions.md` para fornecer instruções específicas para tipos de arquivo (testes unitários, componentes Astro e a camada de dados) ou tarefas.
- Use linguagem natural. Mantenha as orientações claras e dê exemplos de como o código deve ou não deve ser escrito.

Não há uma única maneira correta de criar arquivos de instruções, assim como não há uma única maneira de usar IA. Com a experiência, você descobrirá o que funciona melhor para seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter uma coleção sólida de arquivos de instruções. Ao explorar os arquivos deste projeto, você verá instruções para várias tarefas, como [atualizações da interface do usuário][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de um jeito diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` na CLI). A lição da interface usada explicará a opção relevante.
>
> Para encontrar modelos ou um ponto de partida, explore o [awesome-copilot][awesome-copilot], um repositório repleto de arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Leia os arquivos de instruções incluídos no repositório: há um arquivo principal `copilot-instructions.md` e vários arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a descrição breve do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas de **GitHub Actions Workflows**. Elas se aplicam a todas as interações com o Copilot.
3. Abra a pasta `.github/instructions` e explore os arquivos. Há instruções para arquivos Astro, para a camada de dados Drizzle, para testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um padrão glob relativo à raiz do repositório que determina a quais arquivos as instruções se aplicam. Aqui, qualquer arquivo de teste TypeScript, como um arquivo que corresponda a `**/*.test.ts`, corresponde ao padrão.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Assim, você pode dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar exemplos para o Copilot seguir. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Criar uma branch para as alterações

Vamos seguir algumas práticas recomendadas e criar uma branch para as alterações.

1. Volte ao codespace usado no exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de iniciar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para começar uma conversa limpa. A seleção de modo e modelo varia por exercício; cada etapa indicará as opções relevantes.

Para observar o impacto das instruções personalizadas, primeiro envie um prompt com o arquivo de instruções atual. Depois, atualize o arquivo e reenvie o mesmo prompt para comparar os resultados.

1. Feche as guias abertas no editor para que o Copilot use somente o contexto desejado.
2. Abra `src/lib/publishers.ts` para mostrar ao Copilot onde o helper deve ficar.
3. Selecione **Agent** no menu suspenso de agentes da visualização Chat para que o Copilot possa aplicar alterações aos arquivos.

   ![Captura de tela do seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um helper de acesso a dados que retorne uma lista de todas as editoras, incluindo o nome e a ID de cada uma. Aplique as alterações aos arquivos.
   ```

5. O Copilot explora o projeto e aplica atualizações, muitas vezes tanto ao arquivo do helper quanto aos testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna um array tipado de editoras. Isso segue as convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, aplicadas a `src/lib/*.ts`.
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho de comentário no arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários de documentação mesmo sem essa orientação. Se isso acontecer, tudo bem: o objetivo é observar a consistência das melhorias após a atualização das instruções.

## Adicionar um padrão para o repositório

Como destacado antes, `.github/copilot-instructions.md` fornece informações gerais do projeto ao Copilot. Vamos registrar os padrões de programação para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar por volta da linha 27. Observe que ela documenta os padrões do projeto, mas ainda não inclui uma regra para documentação no código — por isso, o helper gerado não tinha comentários de documentação.
3. Adicione as linhas Markdown a seguir logo depois dos padrões existentes para instruir o Copilot a adicionar cabeçalhos de comentário e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ficar no nível do repositório (`.github/copilot-instructions.md`) para orientações gerais ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o local certo para padrões gerais, como a regra de comentários de documentação que você acabou de adicionar.

## Reenviar o prompt e observar a alteração

Depois de atualizar as instruções, repita o prompt para comparar o código gerado com o resultado anterior.

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Selecione `src/lib/publishers.ts` novamente para que o Copilot se concentre no arquivo correto.
3. Envie o **mesmo prompt** usado anteriormente:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um helper de acesso a dados que retorne uma lista de todas as editoras, incluindo o nome e a ID de cada uma. Aplique as alterações aos arquivos.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a:

   ```typescript
   /**
    * Helpers de acesso a dados de editoras para a plataforma de financiamento coletivo Tailspin Toys.
    * Fornece funções para recuperar informações de editoras do banco de dados.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Retorna uma lista de todas as editoras, com suas IDs e nomes.
    *
    * @param db - Cliente de banco de dados Drizzle.
    * @returns Uma promessa resolvida com um array de objetos de editoras.
    */
   ```

Você orientou o Copilot a seguir um novo padrão e aplicá-lo ao código que servirá de base para o próximo exercício.

## Fazer commit das instruções e enviar a branch

Os arquivos de instruções são tratados como qualquer outro item do repositório e gerenciados pelo controle do código-fonte. Vamos fazer commit das alterações e enviar a branch.

1. Abra uma nova janela do terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme que as alterações nas instruções e no helper estão presentes:

   ```bash
   git status
   ```

3. No terminal, prepare e faça commit da atualização das instruções e de todos os arquivos alterados pelo Copilot para criar a base do helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras alterações de suporte (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e fazer merge de uma solicitação de pull

Com a branch enviada, vamos abrir uma solicitação de pull e associá-la à issue sobre o padrão de documentação do backlog. O Copilot pode localizar a issue, criar uma solicitação de pull (PR) que a feche e, em seguida, fazer merge.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executará ações no GitHub em seu nome. O modelo do projeto já conecta as ferramentas do GitHub ao workspace, então não há nada para configurar. Na primeira vez que o Copilot usar uma delas, o VS Code talvez solicite que você entre no GitHub. Siga as instruções para permitir o acesso. Você verá como essa conexão funciona — por meio do Model Context Protocol — em um exercício posterior.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme que **Agent** está selecionado no seletor de agentes.
3. Peça ao Copilot que encontre a issue sobre a atualização dos arquivos de instruções e crie uma PR da branch atual, descrevendo tanto as atualizações de instruções quanto a nova base do helper de editoras:

   ```
   Encontre a issue relacionada à atualização do arquivo de instruções. Crie uma nova PR a partir da branch atual, informe que a PR fecha essa issue e mencione também que adicionamos a base do helper de editoras para o próximo trabalho de filtragem.
   ```

4. O Copilot começará a localizar a issue e criar a PR.
5. Quando o Copilot pedir permissão para executar ações do GitHub em seu nome, revise o comando e selecione **Allow** se for apropriado.
6. Depois que a PR for criada, peça ao Copilot que faça merge dela em `main` e volte à branch `main` local:

   ```
   Faça merge da PR em main. Em seguida, volte localmente para main e baixe o código mais recente para deixar tudo atualizado.
   ```

7. Quando o Copilot pedir permissão para executar ações do GitHub e comandos de shell em seu nome, revise o comando e selecione **Allow** se for apropriado.

Você acabou de criar e integrar uma solicitação de pull com a ajuda do GitHub Copilot!

## Resumo e próximos passos

Você usou o Copilot Chat no VS Code para:

- explorar as instruções do repositório e os arquivos específicos por caminho.
- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*.
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`.
- reenviar o mesmo prompt e observar o código proposto adotar o novo padrão.
- fazer commit das instruções atualizadas e da base do helper em `main` para que o próximo exercício possa usá-las.

Em seguida, você usará as instruções no [modo de agente][next-lesson], enquanto o Copilot adiciona uma funcionalidade à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
