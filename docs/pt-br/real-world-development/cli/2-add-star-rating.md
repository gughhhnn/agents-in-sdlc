---
title: "Lição 2 - Adicionar avaliações por estrelas: uma vitória rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cartões dos jogos, revisá-la em um navegador encaminhado e fazer o merge do primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, é hora de fazer sua primeira alteração no projeto. Você começará com algo simples: os jogos já têm uma avaliação por estrelas nos dados, mas os cartões da página inicial ainda não a exibem. Você pedirá ao agente que mostre a avaliação, revisará a alteração e fará o merge como seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em uma branch de funcionalidade.
- pedir ao agente que faça uma pequena alteração no projeto.
- revisar a alteração com `/diff`.
- executar o aplicativo para confirmar a alteração em um navegador encaminhado.
- abrir e fazer o merge do primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes. Porém, os cartões de jogos na página inicial exibem apenas o título, a categoria, a editora e a descrição. Como aquecimento, você pedirá ao agente que mostre a avaliação existente em cada cartão: uma alteração pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal no CLI usa o repositório e a branch atualmente em uso no terminal, em vez de criar uma worktree dedicada. As conversas salvas permitem retomar a mesma discussão depois, enquanto os arquivos e a branch continuam sendo o estado Git normal no disco.

Em uma conversa, você verá três coisas: seus prompts e as respostas do agente, as atividades do agente com ferramentas enquanto explora e edita arquivos e as alterações que você pode inspecionar com `/diff`.

## Iniciar uma conversa e solicitar a alteração

Vamos iniciar uma nova conversa para implementar a funcionalidade.

1. Volte ao Codespace.
2. Se o terminal não estiver aberto desde antes, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Se ainda não estiver em execução, inicie o Copilot com o seguinte comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com o comando slash `/new` e pressione <kbd>Enter</kbd>.
5. Use o seguinte prompt para solicitar a alteração:

   ```plaintext
   Mostre o starRating de cada jogo, de zero a cinco, nos cartões da página de listagem. Se a avaliação for null, mostre "No rating yet". Mantenha o layout atual dos cartões, adicione testes e execute as verificações relevantes.
   ```

O Copilot explorará o projeto, localizará os arquivos usados para exibir os detalhes dos jogos e criará o código necessário. Você acabou de adicionar uma funcionalidade com o Copilot CLI!

## Revisar o diff

Todas as alterações geradas por IA devem ser revisadas antes do merge, mesmo as pequenas. Vamos analisar as alterações diretamente no Copilot CLI.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme que o cartão do jogo mostra a avaliação numérica quando ela existe e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes cobrem os dois estados.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija as falhas.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico, então seu código pode ser diferente. Revise o comportamento em vez de esperar uma implementação exata.

## Verificar as alterações

Não devemos apenas ler o código e presumir que funciona. Vamos pedir ao Copilot que inicie o site para podermos examinar a interface atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa inspecionar no navegador a alteração da avaliação por estrelas. Informe o URL e deixe o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cartões dos jogos exibem suas avaliações de zero a cinco.
4. Volte ao Copilot e peça que interrompa o servidor iniciado:

   ```plaintext
   Interrompa o servidor de desenvolvimento que você iniciou.
   ```

## Abrir e fazer o merge do primeiro pull request

Você criou a funcionalidade. Agora, crie um pull request (PR) para fazer o merge do novo código no projeto.

1. Peça ao agente padrão que faça commit da alteração:

   ```plaintext
   Faça commit das alterações revisadas das avaliações por estrelas com uma mensagem de commit adequada.
   ```

2. Digite `/pr create`. Ao criar o PR, o Copilot CLI enviará o commit existente e exibirá o URL do PR.
3. Abra o PR mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionado e selecionando o URL exibido pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize o `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração com o GitHub Copilot CLI. Especificamente, você:

- iniciou uma conversa focada do Copilot em uma branch de funcionalidade.
- orientou o agente a fazer uma pequena alteração nos cartões dos jogos.
- revisou a alteração com `/diff`.
- executou o aplicativo para confirmar a avaliação por estrelas em um navegador encaminhado.
- abriu e fez o merge do primeiro pull request.

Em seguida, você vai [começar pela issue de filtragem e usar os modos Plan e Autopilot][next-lesson] para criar uma funcionalidade maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
