---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações não enfrentem dívidas técnicas, como problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos esquecidas no backlog por falta de tempo. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir o trabalho, ele cria um pull request (PR) de rascunho para revisão por uma pessoa desenvolvedora. Assim, você pode delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em objetivos mais amplos.

Neste exercício, você explorará como:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- criar issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem dívidas técnicas para resolver. As pessoas contratadas para criar a primeira versão do site deixaram a documentação em um estado inadequado: ela está totalmente ausente. Como primeiro passo, a equipe quer comentários de documentação do TypeScript (TSDoc) em todas as funções exportadas do aplicativo.

A equipe de design também quer melhorar a descoberta de jogos. Cada página de detalhes deve mostrar outros jogos da mesma categoria para que as pessoas continuem navegando. Ainda não é necessário um acabamento visual; o objetivo é permitir testes de aceitação da experiência. Esse trabalho é um impedimento, mas há issues de prioridade mais alta no momento.

Esses são exemplos de tarefas que podem ser adiadas e que são adequadas para atribuição ao agente de nuvem do Copilot. Enquanto o agente trabalha de forma assíncrona, você pode se concentrar em outras tarefas e depois revisar o resultado.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de forma semelhante a uma pessoa desenvolvedora. Por exemplo, você pode [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o andamento, prepara o ambiente e inicia a tarefa. Você pode examinar a sessão durante a execução ou depois da conclusão. Quando a solução estiver pronta para revisão, o agente marcará você no pull request.

## A importância de instruções com escopo adequado

O GitHub Copilot não é mágico: ainda é necessário pensar. Mesmo tarefas aparentemente simples podem ter complexidade. Por isso, [defina o escopo das tarefas com cuidado][cloud-agent-best-practices]. Trate o Copilot como uma pessoa parceira de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a IA generativa.

## Instruções personalizadas neste repositório

Exercícios anteriores apresentaram as instruções personalizadas e como elas orientam o Copilot. Se você já concluiu a atividade prática sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação. Caso contrário, este é um bom momento para consultar os arquivos de instruções deste repositório antes de atribuir trabalho ao agente de nuvem.

Abra os arquivos a seguir na interface Web do GitHub ou em um Codespace ativo:

- `.github/copilot-instructions.md`: examine a seção **Code standards**, principalmente as expectativas sobre convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que limita essas instruções aos arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente de nuvem, observe no PR os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário definidos por esses arquivos. Voltaremos a isso ao revisar os PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente de nuvem

A criação de código normalmente exige um ambiente específico e scripts de configuração. O mesmo vale para tarefas atribuídas ao Copilot, que trabalha de maneira semelhante a um engenheiro de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo com um [fluxo de trabalho de configuração][setup-workflow] em `.github/workflows/copilot-setup-steps.yml`, executado antes do início do trabalho. O modelo do laboratório já inclui esse fluxo de trabalho para configurar Node.js, dependências do projeto, navegadores para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido usado pelo aplicativo Astro:

```yaml
name: "Copilot Setup Steps"
# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch
env:
  ASTRO_TELEMETRY_DISABLED: "1"
jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no nível mínimo necessário para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Se quiser clonar o repositório como parte da configuração, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se não clonar o repositório durante a configuração, o Copilot fará isso automaticamente depois que as etapas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5
      # Configuração do front-end/aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
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
      # Migre e preencha o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com outros fluxos de trabalho do GitHub, mas tem alguns pontos importantes:

- Há um único trabalho, `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o fluxo manualmente na guia **Actions** do repositório. Isso é útil para verificar se ele funciona sem aguardar uma execução do Copilot.

## Adicione documentação

A documentação desatualizada ou inexistente é uma dívida técnica que reduz a produtividade e dificulta a manutenção do código e a integração de novas pessoas à equipe. O Copilot pode ajudar a criar documentação em segundo plano; você revisará o resultado em um exercício posterior.

1. Em uma nova guia do navegador, acesse seu repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

   ```plaintext
   Nossa organização exige que funções e métodos tenham comentários de documentação TSDoc quando forem úteis. Infelizmente, as atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para incluir comentários que esclareçam o comportamento.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

   ![Detalhes da atribuição ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está preparando o ambiente, executando o fluxo de trabalho descrito acima, analisando o projeto e definindo uma abordagem para a tarefa.

13. Revise a lista e as tarefas que o Copilot pretende executar.
14. Role a linha do tempo do pull request até encontrar a atualização indicando que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Percorra a sessão em tempo real e observe como o Copilot resolve o problema: explora o código, entende o estado do projeto, planeja a abordagem e cria código.

Isso pode levar vários minutos. Uma das vantagens do agente de nuvem é executar tarefas de forma assíncrona, liberando você para trabalhar em outras atividades. Aproveite essa possibilidade: atribua outra tarefa ao agente e, enquanto isso, comece a criar funcionalidades para o aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Uma vantagem do agente de nuvem é dividir o trabalho: você pode se concentrar em uma tarefa enquanto o agente cuida de outra. Atribua ao agente a seção de jogos relacionados solicitada pela equipe de design.

1. Volte ao repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

   ```markdown
   Queremos ajudar as pessoas a descobrir mais jogos exibindo jogos relacionados na página de detalhes de cada jogo. A equipe de design quer explorar a experiência do usuário (UX) e fazer testes de aceitação. Estes são os requisitos:
   - Adicione em `src/lib/` um helper de acesso a dados que retorne outros jogos da mesma categoria de um jogo, sem incluir o próprio jogo.
   - Exiba na página de detalhes uma seção "Related games" que use esse helper.
   - Trate o caso em que um jogo não tenha jogos relacionados.
   - Crie testes de unidade para o novo helper.
   - Antes de criar o PR, confirme que todos os testes foram aprovados.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá o emoji 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot está trabalhando na segunda solicitação. Como o agente de nuvem funciona de forma semelhante a uma pessoa engenheira de software, não é necessário monitorá-lo continuamente; revise o trabalho quando ele terminar. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Neste exercício, você conheceu o [agente de nuvem do GitHub Copilot][copilot-agents]. Você pode atribuir issues ao Copilot para execução assíncrona, seja para resolver dívidas técnicas, criar funcionalidades ou ajudar a migrar código entre frameworks.

Você explorou:

- como personalizar o ambiente de geração de código.
- como garantir que as operações sejam realizadas com segurança.
- a importância de issues com escopo claro.
- como atribuir issues ao Copilot.

Enquanto o agente trabalha em segundo plano, você pode criar e usar agentes personalizados. O [agente de nuvem também pode usar servidores do Model Context Protocol (MCP)][cloud-agent-mcp] e arquivos de instruções personalizadas, explorados em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://github.com/features/actions
