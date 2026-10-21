---
title: "Exercício 5: Iterar no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que adicionasse documentação ao código, criasse um recurso de jogos relacionados para a equipe de design avaliar e implementasse recursos de acessibilidade, incluindo alternâncias para os modos de alto contraste e claro. Vamos explorar as alterações no código e, se necessário, enviar comentários ao Copilot para melhorar o resultado.

### Cenário

Como destacamos várias vezes, os fundamentos de design de software e DevOps não mudam com a adição de IA generativa. Sempre precisamos revisar o código gerado e seguir nosso processo normal de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para a documentação, a funcionalidade de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e o agente do GitHub Copilot na nuvem

Como o agente do Copilot na nuvem trabalha de forma assíncrona e sem supervisão, há restrições de segurança para garantir que o ambiente permaneça protegido. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente na nuvem é executado no GitHub Actions, que cria um ambiente temporário separado para o trabalho.
- Um ser humano precisa aprovar todos os fluxos de trabalho do GitHub Actions antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores do Model Context Protocol (MCP).

## Revisar a documentação gerada

Vamos começar explorando a primeira solicitação de pull (PR) criada pelo agente do GitHub Copilot na nuvem: a que adiciona documentação ao código. Você usará a interface padrão de PR do GitHub.com.

> [!NOTE]
> Ao explorar a PR, talvez você veja um aviso de que o GitHub Copilot está bloqueado por um firewall. Isso é **esperado**, pois o Copilot tem acesso limitado a recursos externos por padrão, incluindo chamadas a servidores MCP externos. Se quiser, você pode [personalizar ou desabilitar o firewall do agente do Copilot na nuvem][agent-firewall].

1. Volte ao repositório em github.com.
2. Selecione **Pull Requests** para abrir a lista de solicitações de pull.
3. Abra a PR intitulada algo como **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, a PR terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos; aproveite para fazer uma pausa ou refletir sobre o que já aprendeu.

4. Quando a PR estiver pronta, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui comentários TSDoc e outra documentação. As alterações exatas podem variar.

    Ao revisar o código, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Esses elementos vêm dos arquivos de instruções personalizadas que você revisou no início do Exercício 2.

6. Depois de revisar as alterações, volte à guia **Conversation** e role a página para baixo.
7. Procure um indicador de que alguns fluxos de trabalho estão aguardando aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão aparecer na seção de verificações da solicitação de pull. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Pedir alterações ao GitHub Copilot

Trabalhar com o Copilot em uma solicitação de pull não é uma via de mão única. Você também pode mencionar o Copilot em comentários da PR, como faria com outros integrantes da equipe, ou em comentários associados a trechos do código. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados são não determinísticos, não podemos prescrever exatamente o que pedir. Por exemplo, você pode pedir ao Copilot que:

- adicione cabeçalhos de comentário no início de cada arquivo de código, com uma breve descrição da finalidade.
- adicione comentários TSDoc aos arquivos TypeScript e Astro.
- crie um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário pedindo uma alteração na documentação gerada e mencione **@copilot**, como faria com qualquer usuário. Use uma das ideias acima ou outra sugestão de documentação que queira ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar à solicitação de pull.

    ![Voltar à solicitação de pull](../../../_images/ex7-back-to-pr.png)

4. Depois que o Copilot concluir as alterações, deverá aparecer um novo commit na PR.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, você pode marcar a PR de rascunho como pronta para revisão e fazer merge dela na branch principal.

![Marcar a PR como pronta para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar a funcionalidade de jogos relacionados

Vamos voltar à PR criada pelo Copilot para resolver a issue de exibir jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione a PR intitulada algo como **Show related games on the game details page**.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as alterações, volte à guia **Conversation** e role a página para baixo.
6. Procure um indicador de que alguns fluxos de trabalho estão aguardando aprovação.
7. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

8. Os fluxos de trabalho deverão aparecer na seção de verificações da solicitação de pull. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você também pode mudar para essa branch no Codespace e testar manualmente a funcionalidade de jogos relacionados. Acesse o Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot, como `copilot/fix-8`):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou a funcionalidade de jogos relacionados. Como antes, você pode iterar com o agente do Copilot na nuvem para solicitar atualizações. Por exemplo, pode pedir que ajuste o número de jogos relacionados ou que adicione cabeçalhos de comentário e comentários TSDoc (lembre-se: essa tarefa foi atribuída **antes** da atualização das instruções personalizadas). Como antes, adicione um comentário na guia **Conversation** para que o Copilot o veja e inicie outra sessão.

## Revisar os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados com o agente personalizado de acessibilidade. Esta PR deve incluir tanto o modo de alto contraste atribuído no Exercício 3 quanto o modo claro solicitado no painel de controle durante o Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione a PR intitulada algo como **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, a PR terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho, que pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação e preste atenção especial a:
   - componentes de alternância da interface do usuário (UI) entre os modos.
   - uso do armazenamento local para persistir as preferências do usuário.
   - alterações de CSS ou estilo para os modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos da especificação Accessible Rich Internet Applications (ARIA) e navegação por teclado.
   - código JavaScript/TypeScript que controla a alternância dos modos.
6. Depois de revisar as alterações, volte à guia **Conversation** e role a página para baixo.
7. Procure um indicador de que alguns fluxos de trabalho estão aguardando aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão aparecer na seção de verificações da solicitação de pull. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** você pode mudar para essa branch no Codespace para testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste as alternâncias dos modos de alto contraste e claro no navegador. Confirme se funcionam e se as preferências são mantidas após recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot a implementar esses recursos de acordo com as práticas recomendadas de acessibilidade. Se encontrar problemas de acessibilidade ou oportunidades de melhoria, mencione **@copilot** em um comentário para pedir atualizações, como fez nas PRs anteriores.

## Exercício opcional — continuar delegando

O agente na nuvem é mais útil quando você pode atribuir itens reais do backlog e se concentrar em outras tarefas. Para criar esse hábito, registre mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na página de listagem de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o ambiente de agente na nuvem. Ao longo destas lições, você:

- **Inspecionou os arquivos de instruções personalizadas incluídos no repositório** para observar como influenciam o resultado do agente na nuvem.
- **Atribuiu issues ao agente do Copilot na nuvem** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade e adicionou alternâncias para os modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel de controle** para monitorar e orientar a sessão de acessibilidade em andamento.
- **Revisou e iterou nas solicitações de pull do agente na nuvem**, mencionando `@copilot` para pedir alterações e aprovando fluxos de trabalho.

## Revisão e próximos passos

Você concluiu o ambiente do agente na nuvem. Se quiser continuar explorando, os outros ambientes complementam o que praticou aqui:

- 🖥️ **[Ambiente VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração MCP diretamente no IDE.
- 💻 **[Ambiente CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal usando os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, Playwright MCP, Agent Merge e uma ponte opcional `/delegate` para o agente na nuvem.

No seu próprio repositório, experimente estas ideias:

- atribua ao agente na nuvem uma issue de refatoração ou cobertura de testes.
- crie um agente personalizado para outro domínio.
- configure `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente do Copilot na nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente do Copilot na nuvem][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
