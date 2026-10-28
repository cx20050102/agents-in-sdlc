---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que realizasse três tarefas distintas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente de nuvem trabalhe de forma assíncrona e autônoma, é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel de controle, você vê todas as tarefas do agente com pull requests (PRs) abertos, explora as operações realizadas e direciona uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para monitorar as tarefas do agente de nuvem.
- direcionar uma sessão em andamento para solicitar funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação de um modo de alto contraste, a equipe percebeu que seria útil adicionar também um modo claro. Como o trabalho já inclui atualizar o estilo do site e adicionar alternância entre modos, é lógico incluir essa funcionalidade. Direcione o trabalho do agente para garantir que ele implemente também o modo claro.

## Revise as tarefas do agente de nuvem do Copilot

Confira o status atual das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você verá as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações realizadas pelo Copilot. Elas podem incluir:
   - obter o código do repositório.
   - configurar o ambiente de trabalho do Copilot.
   - configurar servidores do Model Context Protocol (MCP).
   - executar várias etapas para concluir a tarefa.

> [!NOTE]
> As etapas exatas variam conforme o estado do trabalho e a abordagem do Copilot.

4. Observe também o painel de pull requests à direita, que permite acompanhar o PR e os arquivos alterados.

## Direcione o agente de nuvem

Agora que você conhece as tarefas ativas, peça ao Copilot para incluir a alternância para o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente à adição do modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado atual do trabalho.

   ![Sessão de acessibilidade no painel de controle](../../../_images/ex6-accessibility-session.png)

2. Observe a sessão por alguns minutos, até que a configuração seja concluída e o trabalho comece. Você verá mensagens semelhantes às mostradas no exercício.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o seguinte prompt:

   ```plaintext
   Enquanto trabalhamos no modo de alto contraste, vamos adicionar também um modo claro. Ele também deve ter um controle para que as pessoas escolham o modo de exibição desejado.
   ```

   ![Tarefa do agente de nuvem na página de agentes, com a caixa de diálogo para direcionar a sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento do prompt e o inclui no fluxo de trabalho.

## Deixe o Copilot trabalhar

Assim como antes, o Copilot continuará a tarefa atualizada. Ele incorporará a nova solicitação depois de concluir a etapa em andamento quando você enviou a mensagem.

Esse processo levará alguns minutos, então aproveite para fazer uma pausa e refletir sobre o que você aprendeu e explorou até agora.

## Resumo e próximos passos

Nesta lição, você explorou a página de agentes do Copilot, o painel central para monitorar e orientar tarefas do agente de nuvem do GitHub Copilot. Nele, você pode acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar tarefas em andamento ou fornecer orientações adicionais.

Você explorou:

- como usar o painel de controle e a página de agentes para monitorar tarefas do agente de nuvem.
- como redirecionar uma sessão em andamento para solicitar funcionalidade adicional.

Com o Copilot concluindo o trabalho nos recursos de acessibilidade, agora você pode [iterar nos pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
