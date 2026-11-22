---
title: "Lição 3 - Modos do agente: Plan e Autopilot"
description: "Use Plan para definir uma abordagem, Autopilot para criar os filtros de uma issue e Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando uma pequena funcionalidade ao projeto. Porém, alterações maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi criado para trabalhar com o fluxo de trabalho já existente da organização e ajudar a construir as coisas certas da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento orientado por agentes, começando por uma issue para criar uma funcionalidade, verificando se o código é válido e se a funcionalidade se comporta como esperado e, por fim, fazendo o merge no projeto.

> [!NOTE]
> Você usará a mesma conversa e branch ao longo do fluxo da funcionalidade. Em geral, você usaria branches ou PRs diferentes para cada tipo de arquivo, mas vamos simplificar para focar nos conceitos principais.

Nesta lição, você vai:

- iniciar uma nova conversa do Copilot a partir de uma issue do GitHub.
- definir os requisitos no modo Plan.
- implementar a funcionalidade usando o modo Autopilot.
- revisar o código.
- validar manualmente a funcionalidade em um navegador encaminhado.

À medida que continuar o trabalho, você atualizará as instruções do repositório, personalizará a skill `quality-checks` existente, adicionará a validação MCP, criará um agente de QA e abrirá o PR da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas detalhes como a combinação de categorias precisam ser definidos antes de começar a codificar. Você usará o modo Plan para decidir esses pontos e, em seguida, autorizará uma implementação delimitada com o Autopilot.

## Contexto

Adicionar agentes de programação de IA ao fluxo de desenvolvimento não muda os fundamentos. Na verdade, eles ficam ainda mais importantes! O fluxo da maioria das pessoas desenvolvedoras se parece com este:

1. Começar por uma issue registrada que descreva o trabalho necessário.
2. Criar um plano do que precisa ser implementado.
3. Criar e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, fazer o merge do código.

> [!NOTE]
> Os detalhes exatos variam conforme a equipe e a organização, mas a maioria dos fluxos segue alguma variação desse processo.

Seguir essa abordagem padrão ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano primeiro e não pode editar arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma: escreve código, executa testes e itera até considerar a tarefa concluída.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Começar pela issue

Antes de começar a filtragem, volte ao Codespace e confirme que o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme que o PR de avaliação por estrelas foi integrado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as setas para destacar a issue intitulada **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` (ou um número semelhante). O caractere `#` permite incluir no contexto uma issue ou pull request (PR) do GitHub.

## Planejar a filtragem

O planejamento permite definir a abordagem de implementação de uma funcionalidade ou tarefa antes de delegá-la ao Copilot. É sempre uma boa ideia reservar algum tempo para planejar o trabalho complexo. Vamos mudar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme se o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite o seguinte prompt:

   ```plaintext
   Crie um plano para implementar esta funcionalidade.
   ```

O Copilot criará o plano. Primeiro, ele explorará o projeto e depois determinará a melhor abordagem.

3. Durante o processo, o Copilot talvez faça perguntas sobre o funcionamento dos filtros. Responda de acordo com suas preferências; não há respostas erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Role a tela para cima e para baixo para revisar o plano.
6. Peça ao Copilot que revise qualquer parte do plano que não corresponda às suas decisões.

## Aprovar o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo. Vamos deixar o Copilot trabalhar usando o Autopilot.

O Autopilot permite que o Copilot itere sobre o problema até considerar que a tarefa está concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção equivalente na versão instalada.
2. Confirme se o indicador abaixo do prompt mostra **Autopilot**.
3. Observe o Copilot seguir o plano definido, gerar o código e executar os testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há um conflito de portas, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revisar e verificar a implementação

Depois que o código for gerado, ele precisa ser revisado antes do merge, como qualquer outro código. Vamos revisar o código e executar o site para confirmar que tudo funciona.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não mostra mais **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e as decisões tomadas durante o planejamento.
4. Depois da revisão, pressione <kbd>Esc</kbd> para sair da visualização do diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que corrija eventuais falhas.

## Explorar a nova funcionalidade

Tudo parece certo, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa testar a funcionalidade de filtragem no navegador. Informe o URL e deixe o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste os filtros por categoria, por editora e as combinações definidas no plano.
4. Confirme que a redefinição dos filtros e o comportamento sem resultados correspondem à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que interrompa o servidor de desenvolvimento iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para criar e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma nova conversa do Copilot a partir de uma issue do GitHub.
- definiu os requisitos no modo Plan.
- implementou a funcionalidade usando o modo Autopilot.
- revisou o código.
- validou manualmente a funcionalidade em um navegador encaminhado.

Em seguida, vamos aprofundar como o código é gerado para seguir as práticas documentadas, [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
