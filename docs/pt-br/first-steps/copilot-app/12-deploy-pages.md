---
title: "Lição 12 - Implantar o site com o GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um workflow do Pages, revisar o pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dê ao Space Quiz um endereço público implantando-o com o GitHub Pages. O Copilot pode preparar o workflow e orientar a implantação, mas você continua responsável por revisar as alterações e confirmar que o site está disponível.

Nesta lição, você vai:

- pedir ao Copilot que configure a implantação do GitHub Pages.
- revisar o workflow e o pull request antes do merge.
- acompanhar a implantação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório GitHub, mas as pessoas ainda precisam de uma maneira simples de experimentá-lo. Publicar o site de arquivo único com o GitHub Pages o disponibiliza em um URL público sem adicionar um servidor ou dependência de runtime.

## Pedir ao Copilot que prepare a implantação

> [!WARNING]
> Criar um pull request pode fazer commit e enviar código. Revise as configurações de aprovação e o repositório de destino antes de enviar este prompt. Se **Approve all** estiver selecionado, não espere uma solicitação de confirmação separada.

Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

```plaintext
Prepare uma implantação do GitHub Pages para este repositório. Crie um workflow do GitHub Actions que compile o site, se necessário, e o implante no GitHub Pages depois que as alterações chegarem à branch padrão. Se ainda não houver um README, adicione um que descreva o aplicativo e explique como executá-lo localmente. Crie um pull request com essas alterações e informe o link e quaisquer configurações ou permissões do Pages necessárias. Não faça merge, não habilite o auto-merge, não altere as configurações do Pages e não implante o site. Pare e aguarde minha revisão.
```

## Revisar o workflow e o pull request

1. Confirme se o README foi adicionado somente se o repositório ainda não tinha um.
2. Revise as alterações do pull request, incluindo as etapas de compilação e implantação do workflow, os gatilhos e as permissões.
3. Confirme se o workflow implanta a partir da branch padrão, não de um pull request ainda não integrado.
4. Solicite as correções necessárias e revise o diff atualizado. Aguarde a aprovação das verificações obrigatórias do pull request antes de autorizar o merge.

## Autorizar o merge e a implantação

> [!WARNING]
> O prompt a seguir autoriza o agente a fazer merge do pull request revisado, alterar as configurações do Pages do repositório e publicar o site. Envie-o somente depois de concluir a revisão e confirmar o repositório e o pull request. Dependendo das configurações de aprovação, essas ações podem ser executadas sem outra solicitação de confirmação.

Substitua `<PR_NUMBER>` pelo número do pull request revisado e envie este prompt separado:

```plaintext
Revisei o pull request <PR_NUMBER> neste repositório e autorizo o merge e a implantação. Habilite o GitHub Pages usando o GitHub Actions, faça o merge desse pull request somente depois que as verificações obrigatórias forem aprovadas e acompanhe a implantação até ela ser concluída com sucesso. Informe o URL final do site publicado. Se você não tiver permissão para habilitar o Pages ou fazer o merge, diga exatamente o que preciso aprovar ou fazer; não informe sucesso até que o site esteja publicado.
```

![Diagrama em três etapas: um pull request do space-quiz integrado; uma execução bem-sucedida de Deploy website no GitHub Actions com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz publicado em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Verificar a implantação

1. Acompanhe a execução de implantação do GitHub Actions até que ela seja aprovada.
2. Abra o URL informado e confirme se o Space Quiz é carregado.

Se o agente não puder habilitar o Pages ou fazer merge do pull request, siga as instruções específicas que ele fornecer. Não considere a tarefa concluída até que a implantação seja aprovada e o site esteja disponível.

## Resumo e próximos passos

Seu Space Quiz agora está publicado, e você verificou a implantação em vez de confiar apenas no relatório do agente. Continue para [Lição 13: Revisão e próximos passos][next-lesson].

[next-lesson]: ../13-review/
