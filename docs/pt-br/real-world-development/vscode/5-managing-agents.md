---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você usa o GitHub Copilot no modo de agente, ele trabalha de forma autônoma: explora o código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece na sua máquina, em tempo real, o Copilot Chat no VS Code mostra as chamadas de ferramentas e as edições de arquivos à medida que ocorrem. Você pode revisar cada diff no próprio chat, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts complementares para aperfeiçoar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições de arquivo propostas.
- orientar o agente durante a sessão para adicionar um seletor do tema claro junto com o modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design fez uma solicitação adicional: também quer um seletor do tema claro para pessoas que consideram o tema padrão escuro demais. Em vez de começar uma nova conversa do zero, você orientará a sessão existente para ampliar o trabalho.

## Acompanhar o progresso do Copilot

A exibição Copilot Chat mostra cada etapa do agente: os arquivos que explora, os comandos que executa e as edições que propõe. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, role a conversa para acompanhar cada chamada de ferramenta. Você verá leituras e edições de arquivos e os comandos de terminal que ele solicitará executar.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
    - Selecione **Keep** para aceitar a alteração.
    - Selecione **Undo** para desfazê-la e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todos os arquivos modificados pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão, por exemplo para executar um comando do terminal, o agente pausará e mostrará uma solicitação **Allow**. Aprove apenas os comandos que considerar seguros.

5. Aguarde o agente terminar de implementar o modo de alto contraste. Uma mensagem final resumirá as alterações feitas.

## Orientar o agente para adicionar o tema claro

Agora que o modo de alto contraste está implementado, você ampliará a mesma conversa para adicionar um seletor de tema claro. Como continuará na mesma sessão de chat, o agente manterá todo o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie o seguinte prompt complementar:

    ```plaintext
    Muito bom! Agora adicione um seletor de tema claro junto com o seletor de alto contraste. O tema claro deve seguir o mesmo padrão: persistir no armazenamento local e ficar acessível na mesma área da interface. As pessoas usuárias devem poder ativar o alto contraste e o tema claro de forma independente.
    ```

2. O agente continuará o trabalho, explorando as alterações existentes e propondo adições para o tema claro.
3. Revise cada nova edição assim que aparecer, como fez com o modo de alto contraste. Use **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada, por exemplo tentar refatorar a interface dos seletores de uma maneira que você não prefere, interrompa o trabalho com o botão **Stop** ao lado do campo de entrada e envie uma instrução complementar mais clara.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts complementares para pedir ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione um terceiro seletor; integre o tema claro ao painel de configurações existente") ou ampliar o escopo ("adicione também atalhos de teclado para os dois seletores").

## Revisar o conjunto de alterações

Antes de fazer commit, revise rapidamente tudo o que o agente alterou.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e em testes relacionados.
3. Abra alguns arquivos alterados e percorra os diffs. Confirme que eles seguem os padrões de acessibilidade do agente personalizado: atributos ARIA, navegação pelo teclado, HTML semântico e persistência por armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts complementares para ampliar o trabalho sem perder o contexto.

Nesta lição, você explorou:

- como acompanhar o trabalho de um agente em tempo real.
- como aceitar e rejeitar edições individuais.
- como orientar uma sessão em andamento com prompts complementares.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as alterações feitas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
