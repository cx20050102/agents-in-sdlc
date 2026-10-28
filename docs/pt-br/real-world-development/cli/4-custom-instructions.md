---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica, disponibilize essa orientação ao Copilot. Os [arquivos de instruções][instruction-files] descrevem não apenas *o que* você quer que o código faça, mas também *como* ele deve ser estruturado. Agora que você criou a filtragem, explore as instruções usadas pelo Copilot, adicione um padrão de documentação e aplique-o ao código.

Nesta lição, você vai:

- explorar como instruções do repositório e instruções associadas a caminhos chegam ao agente.
- atualizar o arquivo de instruções para garantir que os padrões de código sejam seguidos.
- observar o impacto dos arquivos de instruções no código.

## Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para suas práticas:

- Comentários devem explicar a intenção e decisões não óbvias, não repetir o código.
- Funções exportadas em `db/` e `src/lib/` devem documentar com TSDoc/JSDoc sua finalidade, parâmetros e valores de retorno, incluindo o argumento `db` injetável quando houver.
- Componentes Astro reutilizáveis devem documentar os contratos `Props`, e os comentários devem continuar atualizados quando o código relacionado mudar.
- As orientações existentes de formatação e lint devem ser preservadas.

Os arquivos de instruções ajudam a garantir que o Copilot receba as informações necessárias para trabalhar de acordo com essas práticas.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências para que o Copilot entenda melhor o estilo e os requisitos do código. Elas permitem indicar convenções, bibliotecas e comentários desejados, e podem ser aplicadas a todo o repositório ou a tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Inclua informações gerais relevantes à maioria das solicitações.
- `.github/instructions/*.instructions.md`: arquivos com diretrizes para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> O suporte a outros formatos de instrução varia conforme o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explore os arquivos de instruções personalizados do projeto

O projeto inicial já inclui alguns arquivos de instruções. Explore-os antes de fazer alterações para perceber o impacto.

1. Volte ao Codespace.
2. No editor do Codespaces, não no terminal, abra `.github/copilot-instructions.md`.
3. Examine a descrição breve do projeto e as orientações de código. Elas se aplicam a todas as interações com o Copilot neste repositório.
4. Abra `.github/instructions` e examine os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
5. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo` no início. Ele define um glob que determina os arquivos aos quais as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e arquivos existentes do projeto. Assim, conjuntos maiores de instruções podem ser divididos em arquivos menores e reutilizáveis e podem apontar para exemplos que orientam o Copilot.

## Atualize as instruções conforme as diretrizes da equipe

Os arquivos atuais são um bom começo, mas ainda falta uma regra. Atualize `.github/copilot-instructions.md` para solicitar comentários TSDoc no novo código TypeScript.

1. Em `.github/copilot-instructions.md`, localize **Code formatting guidance**, que deve estar próxima da linha 35.
2. Adicione o item a seguir ao final da seção:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente.

## Use as orientações atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa começa. Depois de salvar as instruções, saia e retome a conversa de filtragem para que o Copilot carregue as novas orientações sem perder o contexto do recurso.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit` para encerrá-la. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório, na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch `game-filters-cli` está ativa.

3. Peça ao Copilot para aplicar as orientações atualizadas:

   ```plaintext
   Acabamos de atualizar as instruções e as diretrizes de código. Atualize o código que você gerou para seguir essas orientações.
   ```

4. Digite `/diff` e leia os arquivos TypeScript alterados. Observe os novos comentários TSDoc e confirme se descrevem o código corretamente.

## Resumo e próximos passos

Você explorou como o Copilot CLI recebe contexto dos arquivos de instruções e aplicou um novo padrão ao recurso. Nesta lição, você:

- explorou como instruções do repositório e instruções associadas a caminhos chegam ao agente.
- atualizou o arquivo de instruções para garantir o cumprimento dos padrões de código.
- observou o impacto dos arquivos de instruções no código.

Em seguida, você [personalizará e executará a skill reutilizável quality-checks][next-lesson] para executar testes e lint de maneira consistente.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
