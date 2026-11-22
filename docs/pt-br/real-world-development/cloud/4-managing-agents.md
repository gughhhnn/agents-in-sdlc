---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas distintas para melhorar a experiência das pessoas usuárias e adicionar funcionalidades. Embora o agente de nuvem trabalhe de forma assíncrona e autônoma, ainda é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel de controle, você vê todas as tarefas dos agentes com pull requests (PRs) abertos. É possível explorar as operações executadas e até orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento e solicitar uma funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação do modo de alto contraste, a equipe percebeu que seria um bom momento para adicionar também um tema claro. Como o trabalho já envolvia atualizar o estilo do site e adicionar seletores de modo, pareceu lógico incluir esse recurso. Você orientará o agente para garantir que ele implemente tanto o tema claro quanto o modo de alto contraste.

## Revisar as tarefas do agente de nuvem do Copilot

Vamos conferir o status atual de todas as tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel esquerdo. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
    - atualizar a documentação do código.
    - gerar APIs para modificar jogos.
    - adicionar um modo de alto contraste ao site.
3. Selecione uma das tarefas em andamento. Revise as operações executadas pelo Copilot, que podem incluir:
    - baixar o código do repositório.
    - criar o ambiente de trabalho do Copilot.
    - configurar servidores MCP.
    - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas listadas variam conforme o estado do trabalho do Copilot e a abordagem usada.

4. Observe também o painel de pull requests que aparece à direita. Ele permite acompanhar o PR e os arquivos alterados.

## Orientar o agente de nuvem

Agora que você conhece as tarefas ativas, vamos pedir ao Copilot que inclua o tema claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à criação do modo de alto contraste. O título exato varia conforme o nome usado pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no painel de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até o Copilot concluir a configuração e começar o trabalho. Você saberá que isso aconteceu quando vir mensagens semelhantes aos exemplos.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o seguinte prompt:

    ```plaintext
    Enquanto trabalhamos no modo de alto contraste, vamos adicionar também um tema claro. Deve haver um seletor para esse modo, para que as pessoas usuárias escolham a aparência desejada.
    ```

    ![Captura de tela da tarefa do agente de nuvem na página de agentes, com a caixa de diálogo de orientação destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe o Copilot reconhecer o prompt e incluí-lo no fluxo de trabalho.

## Deixar o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa em andamento quando você enviou a mensagem.

Isso levará alguns minutos, então aproveite para fazer uma pausa e refletir sobre tudo o que aprendeu até aqui.

## Resumo e próximos passos

Esta lição apresentou a página de agentes do Copilot, seu centro para acompanhar e orientar tarefas do agente de nuvem do GitHub Copilot. No painel de controle, você pode acompanhar todas as tarefas ativas e concluídas, revisar o trabalho e redirecionar tarefas em andamento para ajustar o escopo ou fornecer orientações adicionais.

Você explorou estes conceitos:

- usar o painel de controle e a página de agentes para acompanhar as tarefas do agente de nuvem.
- redirecionar uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui as funcionalidades de acessibilidade, vamos [iterar nos pull requests que ele criou][next-lesson].

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
