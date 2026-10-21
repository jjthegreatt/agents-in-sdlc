---
title: "Lição 2 - Adicionar avaliações por estrelas: uma melhoria rápida"
description: "Use a interface de linha de comando (CLI) do GitHub Copilot para fazer uma pequena alteração nos cartões dos jogos, revise-a em um navegador usando a porta encaminhada e faça merge como sua primeira solicitação de pull."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou a interface de linha de comando (CLI) do GitHub Copilot — o Copilot CLI — e experimentou uma conversa, chegou a hora de fazer sua primeira alteração no projeto. Ela será pequena: os dados dos jogos já incluem uma avaliação por estrelas, mas os cartões da página inicial ainda não a exibem. Você pedirá ao agente que mostre essa avaliação, revisará a alteração e fará merge dela como sua primeira solicitação de pull.

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em uma branch de funcionalidade.
- pedir ao agente que faça uma pequena alteração no projeto.
- revisar a alteração com `/diff`.
- executar o aplicativo e confirmar a alteração em um navegador usando a porta encaminhada.
- abrir e fazer merge da sua primeira solicitação de pull.

## Cenário

Cada jogo do Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes do jogo. Porém, os cartões de jogos na página inicial exibem apenas o título, a categoria, a editora e a descrição. Como aquecimento, você pedirá ao agente que mostre a avaliação existente em cada cartão — uma alteração pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma conversa

Uma **conversa** é o espaço onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal do CLI usa o repositório e a branch atualmente selecionados no terminal, em vez de criar uma worktree dedicada. As conversas salvas permitem retomar a mesma discussão mais tarde, enquanto os arquivos e a branch continuam sendo o estado comum do Git no disco.

Em uma conversa, você verá três elementos: seus prompts e as respostas do agente; a atividade do agente com ferramentas enquanto ele explora e edita arquivos; e as alterações que você pode inspecionar com `/diff`.

## Iniciar uma conversa e pedir a alteração

Comece uma nova conversa para implementar a funcionalidade.

1. Volte ao seu Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se ainda não estiver aberto, inicie o Copilot com este comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão usando o comando de barra `/new` e pressionando <kbd>Enter</kbd>.
5. Use este prompt para pedir a alteração:

   ```plaintext
   Mostre a avaliação starRating de cada jogo, em uma escala de 5, nos cartões da página de listagem. Se a avaliação for null, mostre "No rating yet". Mantenha o layout dos cartões, adicione testes e execute as verificações relevantes.
   ```

O Copilot explora o projeto, localiza os arquivos usados para exibir os detalhes dos jogos e cria o código necessário. Você acabou de adicionar uma funcionalidade com o Copilot CLI!

## Revisar o diff

Todas as alterações geradas por IA precisam ser revisadas antes do merge, mesmo as pequenas. Vamos conferir as alterações diretamente no Copilot CLI.

1. Digite `/diff` e inspecione cada arquivo alterado.
2. Confirme que o cartão do jogo exibe a avaliação numérica quando ela existe e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes cobrem os dois casos.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija eventuais falhas.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como toda ferramenta de IA generativa, o Copilot é probabilístico, não determinístico, portanto seu código pode ser diferente. Revise o comportamento em vez de esperar uma implementação específica.

## Verificar as alterações

Não devemos apenas ler o código e presumir que ele funciona. Vamos pedir ao Copilot que inicie o site para podermos examinar a interface do usuário (UI) atualizada no navegador usando a porta encaminhada pelo Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa inspecionar no navegador a alteração da avaliação por estrelas. Informe o endereço (URL) e deixe o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cartões dos jogos exibem as avaliações em uma escala de cinco estrelas.
4. Volte ao Copilot e peça que ele pare o servidor iniciado:

   ```plaintext
   Pare o servidor de desenvolvimento que você iniciou.
   ```

## Abrir e fazer merge da sua primeira solicitação de pull

Você criou a funcionalidade! Agora, crie uma solicitação de pull (PR) para fazer merge do novo código no projeto.

1. Peça ao agente padrão que faça commit da alteração:

   ```plaintext
   Faça commit das alterações revisadas na avaliação por estrelas com uma mensagem de commit apropriada.
   ```

2. Digite `/pr create`. Ao criar a PR, o Copilot CLI envia o commit existente e exibe a URL da PR.
3. Abra a PR mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionada e selecionando a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao seu Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você publicou sua primeira alteração com o GitHub Copilot CLI. Em particular, você:

- iniciou uma conversa focada do Copilot em uma branch de funcionalidade.
- orientou o agente a fazer uma pequena alteração nos cartões dos jogos.
- revisou a alteração com `/diff`.
- executou o aplicativo e confirmou a avaliação por estrelas em um navegador usando a porta encaminhada.
- abriu e fez merge da sua primeira solicitação de pull.

Em seguida, você partirá da issue de filtragem e usará os modos Plan e Autopilot para [criar uma funcionalidade maior][next-lesson].

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
