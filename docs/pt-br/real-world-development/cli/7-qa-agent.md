---
title: "Lição 7 - Criar e usar um agente de garantia de qualidade (QA)"
description: "Crie um agente personalizado de QA que combine os requisitos da issue, a skill quality-checks e Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o Playwright MCP para observar o recurso de filtragem no navegador. Agora, reúna esses recursos em um agente personalizado com um processo de QA bem definido.

Nesta lição, você vai:

- explorar como um agente personalizado trabalha com instruções, skills e ferramentas do Model Context Protocol (MCP).
- criar e inspecionar um perfil reutilizável de garantia de qualidade.
- selecionar o agente de QA e revisar as descobertas em relação à issue de filtragem.

## Cenário

A Tailspin Toys quer revisar requisitos, qualidade do código, verificações automatizadas, cobertura de testes e comportamento no navegador de forma consistente antes de abrir um pull request (PR). Um agente personalizado pode coordenar esse processo de QA e gerar um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot, definida em um perfil Markdown. O perfil descreve a finalidade, as instruções e as ferramentas disponíveis ao agente. Neste workshop, você definirá a função de QA em `.github/agents/qa.agent.md` e a selecionará no Copilot CLI.

As personalizações utilizadas até agora têm funções distintas. As instruções do repositório descrevem os padrões da equipe. A skill `quality-checks` reúne verificações repetíveis. O Playwright MCP fornece ferramentas de navegador. O perfil de QA informa ao Copilot como usar esses recursos para avaliar os requisitos e relatar as descobertas. O perfil não substitui esses recursos nem exige uma nova conversa.

## Crie o perfil de QA

Antes de abrir o PR do recurso, peça ao Copilot para criar um perfil de QA reutilizável. Ele definirá as verificações a executar e os limites que deve respeitar.

1. Volte ao Codespace e confirme que a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão que crie o agente personalizado:

   ```plaintext
   Crie um agente personalizado chamado QA em `.github/agents/qa.agent.md`. Ele deve comparar os recursos com as respectivas issues e os requisitos acordados, seguir as instruções do repositório, executar a skill `quality-checks`, usar o Playwright MCP para verificar o comportamento e adicionar testes quando a cobertura estiver incompleta.

   Peça que ele classifique cada requisito como `pass`, `fail` ou `blocked` e apresente as evidências correspondentes. Ele deve pedir autorização antes de alterar o código de implementação e não deve fazer commits nem abrir pull requests.

   Por enquanto, apenas crie o perfil para que eu possa revisá-lo.
   ```

## Inspecione o perfil

Antes de usar o novo agente, revise o perfil para confirmar se o Copilot registrou o fluxo de QA e os limites esperados. Assim, você evita um agente incompleto ou amplo demais que altere o recurso quando você só quer verificá-lo.

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. `description` é obrigatório; `name` é opcional, mas fornece um nome de exibição claro.
3. Leia as instruções e confirme se o QA começa pelos requisitos, segue as instruções do repositório, executa a skill `quality-checks` e usa Playwright MCP.
4. Confirme se o QA apresenta evidências, pergunta antes de alterar código de implementação e não faz commits nem abre pull requests.
5. Se o perfil não incluir alguma dessas responsabilidades ou algum desses limites, peça ao agente padrão que o revise antes de continuar.

## Execute o QA com base na issue

O Copilot CLI carrega os agentes do projeto quando uma conversa começa. Retome a mesma conversa de filtragem depois de criar o perfil e selecione o agente QA para que ele possa usar o contexto da issue e as decisões de planejamento.

1. Ative o agente com este comando:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como você acabou de criar o agente, talvez ele ainda não apareça na lista de agentes disponíveis. Mesmo assim, o comando acima o ativa.

2. Use este prompt para pedir ao agente QA que revise o recurso:

   ```plaintext
   Revise o recurso de filtragem com base na issue e nas decisões do nosso plano. Ele está pronto para um PR?
   ```

3. Aguarde o agente QA concluir a revisão.
4. Leia o relatório apresentado.

## Resumo e próximos passos

Você adicionou uma função especializada reutilizável ao fluxo de trabalho e revisou o resultado. Nesta lição, você:

- explorou como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criou e inspecionou um perfil reutilizável de garantia de qualidade.
- selecionou o agente de QA e revisou suas descobertas em relação à issue de filtragem.

Agora você tem a implementação, a atualização da skill, o perfil de QA, os testes e o relatório de verificação prontos para revisão. Em seguida, você [reunirá tudo em um PR do recurso e usará o Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
