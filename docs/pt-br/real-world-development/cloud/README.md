---
slug: pt-br/real-world-development/cloud
title: "Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O **[agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** permite que o GitHub Copilot trabalhe de forma assíncrona na nuvem. Você atribui uma tarefa no GitHub, e o agente de nuvem a executa em segundo plano — explora o repositório, faz alterações e abre um pull request — enquanto você fica livre para fazer outras coisas.

Ao longo destes exercícios, você adicionará instruções personalizadas que o agente de nuvem seguirá e atribuirá uma issue do GitHub para que ele implemente o trabalho. Você revisará e usará agentes personalizados para orientar a abordagem, acompanhará e direcionará sessões pelo painel de agentes e terminará revisando os pull requests e iterando sobre os resultados.

## Exercícios

| Exercício | Tópico | Descrição |
|----------|-------|-----------|
| [0. Pré-requisitos][ex0] | Configuração | Criar o repositório e o Codespace |
| [1. Instruções personalizadas][ex1] | Contexto | Adicionar instruções personalizadas para o agente de nuvem |
| [2. Agente de nuvem][ex2] | Agente assíncrono | Atribuir issues ao agente de nuvem do Copilot |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revisar e usar agentes personalizados |
| [4. Gerenciar agentes][ex4] | Monitoramento | Acompanhar e orientar sessões de agentes |
| [5. Iterar][ex5] | Revisão | Revisar PRs, iterar no trabalho do Copilot e escolher próximos passos |

## Pré-requisitos

Antes de participar deste workshop, confirme se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] O agente de nuvem do Copilot habilitado para sua conta ou organização

> [!TIP]
> Não tem um plano pago? Estudantes com verificação válida podem usar o GitHub Copilot gratuitamente pelo [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui o agente, MCP, revisão de código e os recursos do Copilot CLI usados neste workshop, para que você possa concluir todos os roteiros com ele.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> O agente de nuvem está disponível nos planos **Copilot Student, Pro, Pro+, Business e Enterprise**; o Copilot Free é o único plano que não o inclui. No Copilot Business ou Enterprise, um administrador precisa habilitá-lo para a organização.

> [!NOTE]
> Este roteiro não aborda MCP. Para aprender a usar servidores MCP com o Copilot, consulte o [roteiro do CLI](../cli/) ou o [roteiro de VS Code](../vscode/).

## Começar

**[Comece pelo Exercício 0: Pré-requisitos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
