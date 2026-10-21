---
title: "Exercício 1 - Instruções personalizadas (agente na nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente do Copilot na nuvem →][next-lesson]

É provável que poucas organizações, se é que alguma, não enfrentem problemas de dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de funcionalidade que ficaram esquecidas no backlog por falta de tempo para implementá-las. O agente do GitHub Copilot na nuvem foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria uma PR de rascunho para que um desenvolvedor revise o trabalho. Assim, você pode delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em questões mais amplas.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e dos arquivos específicos por caminho.
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos ambientes de VS Code e da interface de linha de comando (CLI), aqui você não comparará prompts de antes e depois. O agente do Copilot na nuvem trabalha de forma assíncrona com issues do GitHub, portanto é mais difícil demonstrar o impacto lado a lado em tempo real. Você verá a influência do arquivo de instruções mais tarde, ao revisar as PRs produzidas pelo agente na nuvem neste ambiente.

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

- `.github/copilot-instructions.md` é um único arquivo enviado ao Copilot em **todas** as solicitações relacionadas ao repositório. Ele deve conter informações relevantes para a maioria das solicitações de chat ou CLI, como a pilha de tecnologias, uma visão geral do que está sendo criado, práticas recomendadas e outras orientações gerais.
- Os arquivos `.github/instructions/*.instructions.md` podem ser criados para tarefas ou tipos de arquivo específicos. Eles podem fornecer diretrizes para linguagens, como TypeScript ou Astro, ou para tarefas como criar um componente de UI ou testes unitários.

> [!NOTE]
> No ambiente de desenvolvimento integrado (IDE), os arquivos de instruções são usados para gerar código no Copilot Chat, mas não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente do Copilot na nuvem usam tanto as instruções do repositório quanto os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma discussão completa sobre a criação de arquivos de instruções está fora do escopo deste workshop. Os exemplos do projeto demonstram uma abordagem representativa. Em linhas gerais:

- Mantenha as instruções em `copilot-instructions.md` focadas em orientações para o projeto, como uma descrição do que está sendo criado, a estrutura do projeto e os padrões gerais de programação.
- Use arquivos `*.instructions.md` para fornecer instruções específicas para tipos de arquivo (testes unitários, componentes Astro, a camada de dados) ou tarefas.
- Use linguagem natural. Mantenha as orientações claras e dê exemplos de como o código deve ou não deve ser escrito.

Não há uma única maneira correta de criar arquivos de instruções, assim como não há uma única maneira de usar IA. Com a experiência, você descobrirá o que funciona melhor para seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter uma coleção sólida de arquivos de instruções. Ao explorar os arquivos deste projeto, você verá instruções para várias tarefas, como [atualizações da UI][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de um jeito diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI). A lição da interface usada explicará a opção relevante.
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
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um padrão glob (relativo à raiz do repositório) que determina a quais arquivos as instruções se aplicam. Aqui, qualquer arquivo de teste TypeScript, como um arquivo que corresponda a `**/*.test.ts`, corresponde ao padrão.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Assim, você pode dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar exemplos para o Copilot seguir. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Adicionar um padrão para o repositório

Agora, você fará a única edição deste exercício: adicionará uma regra para que a documentação fique no código, em comentários TSDoc e em um cabeçalho de comentário no arquivo. O agente na nuvem usará essa regra nas issues atribuídas a ele nas próximas atividades.

Antes de editar, crie uma branch. O agente na nuvem lerá as instruções da branch de destino da issue, mas você também fará commit das alterações de forma organizada.

1. No terminal do seu codespace, crie e selecione uma nova branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## Fazer commit, enviar e integrar a atualização das instruções

O agente na nuvem lê os arquivos de instruções da branch de destino da issue. Na próxima atividade, você atribuirá uma issue ao Copilot, que criará uma branch a partir de `main`. Portanto, as instruções precisam estar em `main` para que o agente possa usá-las.

1. Prepare e faça commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. Em github.com, abra uma solicitação de pull da branch `custom-instructions-cloud` para `main` e faça merge dela. Assim, o agente na nuvem poderá usar essas instruções ao trabalhar na próxima atividade.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, pule as etapas da branch e faça commit diretamente em `main`. A branch está incluída para que o workshop reflita como você trabalharia em um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções do projeto e adicionou um novo padrão geral a `.github/copilot-instructions.md`. Você verá esse padrão aplicado nas solicitações de pull que o agente na nuvem produzirá nas próximas atividades.

Em seguida, você vai [atribuir sua primeira issue ao agente do Copilot na nuvem][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
