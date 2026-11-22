---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que quase todas as organizações tenham dificuldades com dívida técnica: problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de funcionalidades que ficaram paradas no backlog por falta de tempo para implementá-las. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria um rascunho de PR para que uma pessoa desenvolvedora o revise. Isso permite delegar tarefas repetitivas, acelerar o desenvolvimento e deixar mais tempo para trabalhos de maior impacto.

Neste exercício, você explorará o agente de nuvem do Copilot para:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de issues bem delimitadas.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas para resolver. As pessoas contratadas inicialmente para criar a primeira versão do site deixaram a documentação em um estado insatisfatório: você perceberá que ela está completamente ausente. Como primeiro passo, a equipe quer adicionar comentários de documentação TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Ela quer que a página de detalhes de cada jogo mostre jogos relacionados — outros títulos da mesma categoria — para que as pessoas possam continuar navegando. Ainda não é necessário um acabamento visual, pois o objetivo é permitir testes de aceitação da experiência. O trabalho está bloqueado, mas há issues de prioridade maior no momento.

Esses são exemplos de tarefas que podem perder prioridade rapidamente e que são ótimas para atribuir ao agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona enquanto você se dedica a outras tarefas e, depois, você volta para revisar o trabalho e confirmar se está tudo certo.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano de maneira semelhante a uma pessoa desenvolvedora. Assim como no trabalho com uma pessoa, há várias formas de atribuir tarefas, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot criará um rascunho de pull request para acompanhar o progresso, configurará um ambiente e começará o trabalho. Você pode examinar a sessão enquanto o Copilot trabalha ou depois que terminar. Quando a solução estiver pronta para revisão, ele marcará você no pull request.

## A importância de definir bem o escopo do trabalho

Não há mágica no GitHub Copilot: não é possível pular a etapa de pensar. Até operações aparentemente simples ficam complexas quando examinadas de perto. Por isso, [defina o escopo das tarefas do agente de nuvem com cuidado][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: divida o trabalho em etapas, aprenda, experimente e adapte. Os fundamentos do desenvolvimento de software não mudam com a chegada da IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você aprendeu como as instruções personalizadas orientam o Copilot. Se já concluiu uma atividade prática sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para dar uma olhada rápida. Antes de atribuir trabalho ao agente de nuvem do Copilot, leia os arquivos de instruções já incluídos no repositório para observar o efeito deles mais tarde.

Abra os arquivos a seguir na interface Web do GitHub ou em um Codespace já iniciado:

- `.github/copilot-instructions.md`: revise a seção **Code standards**, principalmente as expectativas sobre convenções TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que limita essas instruções aos arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe no pull request os comentários TSDoc, as convenções TypeScript e os cabeçalhos de comentário. Esses elementos vêm dos arquivos de instruções e serão mencionados novamente ao revisar os PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem do Copilot

Criar código normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de forma semelhante a uma pessoa engenheira de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente com um [workflow de configuração especial][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, que é executado antes do início do trabalho. Isso dá ao agente acesso às ferramentas e dependências necessárias para o desenvolvimento. O ambiente já foi configurado para facilitar o fluxo do laboratório e permitir que você explore esse recurso. Ele disponibiliza Node.js, dependências do projeto, binários de navegador para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro único:

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
      # Se quiser clonar o repositório como parte das etapas de configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
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

      # Migrar e preencher o banco de dados SQLite local para que as compilações e os testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Esse arquivo se parece com qualquer outro workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar se ele funciona, em vez de aguardar o início do trabalho do Copilot.

## Adicionar documentação

Todo mundo entende a importância da documentação, mas a maioria dos projetos tem informações desatualizadas ou nenhuma documentação. Esse tipo de dívida técnica costuma ficar sem solução, reduzindo a produtividade, dificultando a manutenção do código e a integração de novas pessoas à equipe. Felizmente, o Copilot é muito bom em criar documentação, e esta é uma issue ideal para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para adicionar a documentação necessária. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina o **Title** como `Code lacks documentation`.
6. Defina a **Description** como:

    ```plaintext
    Nossa organização exige que funções e métodos incluam comentários TSDoc quando forem úteis. Infelizmente, as atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para incluir comentários de documentação quando eles esclarecerem o comportamento.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele não aparecer na lista, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar alguns minutos para aparecer no PR. O Copilot está configurando o ambiente (executando o workflow descrito anteriormente), analisando o projeto e determinando a melhor abordagem.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo do pull request para baixo. Você verá uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot resolve o problema: explora o código e seu estado, pausa para pensar e definir um plano e, em seguida, cria o código.

Isso provavelmente levará alguns minutos. Um dos principais objetivos do agente de nuvem do Copilot é executar tarefas de forma assíncrona, liberando você para se concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente de nuvem e, em seguida, escreveremos código para adicionar funcionalidades ao aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma das grandes vantagens do agente de nuvem do GitHub Copilot é poder dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto ele trabalha em outro. Mesmo que a seção de jogos relacionados solicitada pela equipe de design não leve muito tempo, você poderia usar esse tempo em outras tarefas. Vamos atribuí-la ao agente de nuvem do Copilot.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o template em branco.
5. Defina o **Title** como `Show related games on the game details page`.
6. Defina a **Description** como:

    ```markdown
    Queremos ajudar as pessoas a descobrir mais jogos mostrando títulos relacionados na página de detalhes de cada jogo. A equipe de design quer explorar a experiência do usuário e fazer testes de aceitação. Estes são os requisitos:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Mostrar uma seção "Jogos relacionados" na página de detalhes do jogo, usando o auxiliar
   - Tratar o caso em que um jogo não tem jogos relacionados
   - Criar testes unitários para o novo auxiliar
   - Confirmar que todos os testes passam antes de criar o PR
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver alguns emojis 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando na segunda solicitação. O agente de nuvem funciona de forma semelhante a uma pessoa engenheira de software, então não é necessário acompanhá-lo continuamente; você pode revisar o trabalho quando ele terminar. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Esta lição apresentou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para execução assíncrona. O Copilot pode resolver dívidas técnicas, criar funcionalidades ou ajudar a migrar código de um framework para outro.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de issues bem delimitadas.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, vamos criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e receber instruções personalizadas, que foram abordadas nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

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
