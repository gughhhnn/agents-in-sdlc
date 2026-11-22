---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo geralmente exigem alterações em vários arquivos e operações como executar testes. O fluxo de trabalho de quem desenvolve normalmente envolve localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou faltando, fazer outra alteração... e por aí vai.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para atuar com mais autonomia no IDE. Ele funciona de maneira semelhante a uma pessoa desenvolvedora: começa explorando a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como você pode usar o modo de agente para adicionar novas funcionalidades ao site.

> [!NOTE]
> Apesar dos nomes parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente executa tarefas no IDE, permitindo ciclos rápidos de feedback e interação. O agente de nuvem funciona como uma pessoa programadora parceira, trabalhando de forma assíncrona como integrante da equipe e interagindo por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- o modo de agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados.
- revisar alterações e testes gerados pelo modo de agente do Copilot antes de integrá-los ao código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas usuárias filtrem por categoria e editora. Você já adicionou um auxiliar de editoras no exercício anterior e agora concluirá o trabalho restante na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Executar o site Tailspin Toys

Antes de fazer alterações, vamos explorar o site Tailspin Toys para entender as funcionalidades atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que renderiza as páginas como HTML estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local por meio do Drizzle ORM no frontmatter; não há uma API de backend separada nem um framework de interface do lado do cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com um pequeno `<script>` Astro específico, usando as APIs DOM padrão.

### Iniciar o site

Para facilitar a execução do site, há uma tarefa `npm run dev` que inicia o único servidor de desenvolvimento Astro. Siga estas etapas para executá-la no GitHub Codespace:

1. Volte ao Codespace. Você deve estar na branch `main` depois do exercício anterior.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma branch e mude para ela para trabalhar na filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o seguinte comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, você verá um aviso com o URL, semelhante a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo pedir para abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Abra o site mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionado e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um Codespace, selecionar o link para o URL localhost no terminal redirecionará automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o Codespace, onde o servidor Web está sendo executado.

### Explorar o site

Com o site em execução, você pode explorar suas funcionalidades principais:

- **Página inicial**: mostra uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá uma página de detalhes com mais informações, incluindo título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção do modelo variam por exercício; cada etapa informa o que usar quando necessário.

A implementação inicial do site funciona, mas queremos aprimorá-la com novos recursos. Vamos começar revisando o backlog. Ao configurar o projeto a partir do template, um backlog de issues foi criado automaticamente. Peça ao GitHub Copilot que mostre esses itens.

1. Selecione **Agent** no seletor de agentes da exibição Chat. O agente **Agent** planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos do terminal e invoca ferramentas.

   ![Captura de tela mostrando o seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo específico. Ao criar o workshop, usamos o Claude Sonnet 4.5 e o incluímos nas instruções para que as sugestões de código sejam relativamente consistentes e proporcionem uma boa experiência. Como os LLMs são probabilísticos, porém, as sugestões podem ser diferentes das indicadas no workshop. Isso é normal e esperado.

> [!NOTE]
> Como os LLMs são probabilísticos, o Copilot talvez use um comando MCP diferente, mas ainda deverá conseguir concluir a tarefa.

3. Envie o seguinte prompt para pedir ao Copilot que mostre o backlog de issues:

   ```plaintext
   Mostre o backlog de itens do meu repositório do GitHub. Ajude-me a priorizá-los com base no que será mais útil para as pessoas usuárias.
   ```

4. Selecione **Continue** para executar o comando e listar todas as issues.
5. Revise a lista gerada.

Observe que o Copilot também priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revisar os arquivos de instruções

Antes de iniciar o agente para gerar código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de instruções da [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como abordar a adição de funcionalidades ao site.

1. No Codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para arquivos de instruções específicos de tipos de arquivo, incluindo:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Assim, é possível reutilizar orientações, dividir conjuntos maiores em partes menores e usar exemplos e templates.

## Implementar a filtragem

Para concluir a filtragem, será necessário fazer pelo menos três alterações separadas no aplicativo:

- adicionar ou aprimorar a lógica restante de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento dos filtros.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes do merge. O modo de agente do Copilot pode realizar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue a conversa atual com o Copilot ou inicie uma nova selecionando **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela mostrando um exemplo de seleção do arquivo de instruções UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme se **Agent** continua selecionado no seletor de agentes da exibição Chat.

   ![Captura de tela mostrando o seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme se **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade de acordo com a issue relacionada no backlog, usando o seguinte prompt:

   ```plaintext
   Atualize o site para incluir filtros por editora e categoria de acordo com os requisitos da issue relacionada no backlog. Já existe um auxiliar de editoras criado no exercício anterior; preserve e refine esse trabalho conforme necessário para concluir as alterações restantes na camada de dados, na interface e nos testes. Confirme se todos os testes passam antes de concluir. O servidor já está em execução, então não é necessário iniciá-lo.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relacionados à funcionalidade, incluindo os auxiliares da camada de dados, a interface e os testes. Em seguida, ele modificará os arquivos e executará os testes.

   ![Captura de tela mostrando o Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e do código. Durante o processo, talvez apareçam controles **Keep** e **Undo** no editor. Quando o Copilot terminar, você verá um controle **Keep** ou **Undo** para cada alteração; não é necessário selecioná-los durante o trabalho.

7. Quando o Copilot solicitar, selecione **Continue** para executar os testes.

   ![Captura de tela mostrando uma caixa de diálogo no painel Copilot Chat pedindo confirmação para executar os testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez ocorram pausas e alguns testes falhem durante o processo. Tudo bem: o Copilot alterna entre geração de código e testes até concluir a tarefa sem detectar erros.

   ![Captura de tela de uma sessão completa do modo de agente do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Lembre-se: é importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, exploramos como usar o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Aprendemos como:

- o modo de agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados.
- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- revisar alterações e testes gerados pelo modo de agente do Copilot antes de integrá-los ao código.

Agora vamos [testar sua funcionalidade com o servidor Playwright MCP][next-lesson] e abrir um pull request.

### Exercício de exploração bônus – Implementar paginação

À medida que a lista de jogos cresce, será necessário habilitar a paginação. Use o que aprendeu neste exercício e peça ao Copilot que implemente paginação no site. Considere:

- seguir as práticas recomendadas existentes, inclusive usando os arquivos de instruções do projeto.
- decidir como a paginação deve funcionar: permitir que a pessoa usuária selecione o tamanho da página ou definir esse valor no código.
- fornecer ao Copilot as orientações necessárias no prompt para criar a implementação desejada.
- iterar com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
