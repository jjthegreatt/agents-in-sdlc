---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use o modo Plan para combinar uma abordagem, Autopilot para criar a filtragem a partir de uma issue e Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando uma pequena funcionalidade ao projeto. Alterações maiores exigem um processo mais robusto. Felizmente, a interface de linha de comando (CLI) do GitHub Copilot, chamada Copilot CLI, foi criada para trabalhar com o fluxo já usado pela organização e ajudar a criar as soluções certas da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento com agentes: partir de uma issue para criar uma funcionalidade, validar o código, confirmar o comportamento e, por fim, fazer merge da alteração no projeto.

> [!NOTE]
> Você continuará o fluxo da funcionalidade na mesma conversa e branch. Em geral, você usaria branches ou PRs diferentes para os diversos tipos de arquivo, mas vamos simplificar para focar nos conceitos principais.

Nesta lição, você vai:

- iniciar uma nova conversa do Copilot a partir de uma issue do GitHub.
- definir requisitos no modo Plan.
- implementar a funcionalidade no modo Autopilot.
- revisar o código.
- validar manualmente a funcionalidade em um navegador usando a porta encaminhada.

Ao continuar o desenvolvimento da funcionalidade, você atualizará as instruções do repositório, personalizará a skill existente `quality-checks`, adicionará validação com MCP, criará um agente de QA e abrirá a PR da funcionalidade.

## Cenário

O catálogo do Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas detalhes como combinar categorias precisam ser definidos antes da implementação. Você usará o modo Plan para resolver essas decisões e, em seguida, autorizará uma implementação delimitada com Autopilot.

## Contexto

Adicionar agentes de programação com IA ao fluxo de desenvolvimento não muda os fundamentos — na verdade, eles se tornam ainda mais importantes. Muitos desenvolvedores seguem um processo parecido com este:

1. Começar por uma issue registrada que descreve o que precisa ser feito.
2. Planejar o que precisa ser criado.
3. Criar e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar uma solicitação de pull (PR).
7. Depois que o código for revisado e o processo de integração contínua for aprovado, fazer merge do código.

> [!NOTE]
> Os detalhes variam conforme a equipe e a organização, mas a maioria dos processos segue uma variação do fluxo acima.

Seguir essa abordagem padrão ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de validação do código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham juntos. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria primeiro um plano e não pode editar arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma — escreve código, executa testes e repete o processo até considerar a tarefa concluída.

Comece no modo Plan, revise o plano e depois use Autopilot para implementá-lo.

## Começar pela issue

Antes de começar a filtragem, volte ao Codespace e prepare o repositório e o terminal.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme que o merge da PR de avaliação por estrelas foi feito.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para a filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as setas para destacar a issue intitulada **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` (ou um número semelhante). O caractere `#` permite incluir no contexto uma issue ou solicitação de pull (PR) do GitHub.

## Planejar a filtragem

O planejamento permite definir a abordagem antes de delegar ao Copilot a implementação de uma funcionalidade ou a execução de uma tarefa. Planejar por alguns minutos é sempre uma boa ideia para tarefas complexas. Vamos mudar para o modo Plan e pedir ao Copilot que crie um plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite o seguinte prompt:

   ```plaintext
   Crie um plano para implementar esta funcionalidade.
   ```

O Copilot começa a trabalhar no plano: primeiro explora o projeto e, em seguida, determina a melhor abordagem.

3. Durante o processo, o Copilot pode fazer perguntas sobre o funcionamento dos filtros. Responda de acordo com suas preferências. Não há respostas certas ou erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano e revise-o.
6. Peça ao Copilot que revise qualquer parte que não corresponda às suas decisões.

## Aprovar o Autopilot

Com o plano elaborado e revisado, chegou a hora de implementá-lo. Vamos deixar o Copilot trabalhar usando o modo Autopilot.

O Autopilot permite que o Copilot repita o processo até considerar que o problema foi resolvido.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção com nome semelhante na versão instalada.
2. Confirme que o indicador abaixo do prompt mostra **Autopilot**.
3. Acompanhe o Copilot enquanto ele segue o plano, gera o código e executa os testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano antes. Se o Copilot informar que faltam dependências ou que há conflito de portas, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revisar e verificar a implementação

Depois que o código for gerado, revise-o antes do merge, como faria com qualquer outro código. Vamos revisar o código e executar o site para confirmar que tudo funciona.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não mostra mais **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação dos filtros e os testes.
3. Compare o resultado com a issue e com as decisões tomadas durante o planejamento.
4. Depois da revisão, pressione <kbd>Esc</kbd> para sair da visualização de diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que resolva as falhas encontradas.

## Explorar a nova funcionalidade

O código parece bom, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site no navegador pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa testar a filtragem no navegador. Informe o endereço (URL) e deixe o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste os filtros por categoria, por editora e as combinações definidas no plano.
4. Confirme que a redefinição dos filtros e o comportamento sem resultados correspondem à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que pare o servidor de desenvolvimento que iniciou.

## Resumo e próximos passos

Você usou diferentes modos de conversa para criar e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma nova conversa do Copilot a partir de uma issue do GitHub.
- definiu requisitos no modo Plan.
- implementou a funcionalidade no modo Autopilot.
- revisou o código.
- validou manualmente a funcionalidade em um navegador usando a porta encaminhada.

Em seguida, vamos analisar mais a fundo como o código é gerado e garantir que ele siga as práticas documentadas usando [instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
