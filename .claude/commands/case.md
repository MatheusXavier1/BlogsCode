---
description: Cria um case de portfólio (página do tipo portfolio) a partir de um brief e sobe como rascunho no WordPress
argument-hint: <slug-do-case>
---

Você vai produzir um case de portfólio da marca configurada no `CLAUDE.md`. Slug informado pelo usuário: $ARGUMENTS

**Configuração da marca:** leia a tabela "Configuração da marca" e a seção "Regras de escrita" do `CLAUDE.md`. Se ainda houver `{{...}}` nesses campos, **pare** e peça ao usuário para rodar `/iniciar`.

Antes de começar, leia `BRAND.md`, `VOICE.md`, `PUBLICACOES.md` (seção Portfólio) e `portfolio/_modelo/case.html`.

**Regras obrigatórias:**
- **Nunca invente números, clientes, depoimentos ou resultados.** Tudo vem de `portfolio/<slug>/brief.md`. O que faltar no brief vira pergunta ao usuário, não suposição.
- Se a autorização do cliente no brief não estiver marcada, **pare** e avise. Não crie nem rascunho.
- Se o brief disser que o nome do cliente não pode ser divulgado, use "empresa do setor X" em todo o texto, na ficha técnica, no título e no `alt` das imagens.
- Use o nome da marca do `CLAUDE.md`, sempre igual, e respeite os termos proibidos dele.
- Mesmas regras de escrita do `/post`: frases curtas, resposta direta no início, palavra-chave foco no título, no primeiro parágrafo e em um H2.

Pipeline:

1. **Brief** — se `portfolio/<slug>/brief.md` não existir, crie a pasta, copie `portfolio/_modelo/brief.md` para lá e peça ao usuário para preenchê-lo. Não continue sem o brief preenchido.
2. **Escrita** — gere `portfolio/<slug>/case.html` a partir de `portfolio/_modelo/case.html`, trocando todos os `{{...}}` e removendo os comentários de instrução. Mantenha o CSS e a estrutura (resumo, números, desafio, método, entregas, resultado, ficha técnica, FAQ, CTA). O título do case deve ser uma pergunta ou afirmação com o resultado principal e a palavra-chave. Escreva também o excerpt (1-2 frases, até 160 caracteres).
3. **Conferência** — rode grep no `case.html` por `\{\{` e pelos termos proibidos do `CLAUDE.md` e corrija. Confira que cada número do texto existe no brief.
4. **Imagens** — se o brief listar imagens, use-as. Todas com `alt` descritivo. Não baixe fotos de banco de imagens para cases sem pedir, porque case precisa mostrar o trabalho real.
5. **Rascunho no WordPress** — via MCP do WordPress.com (`wpcom-mcp-content-authoring`, carregue com ToolSearch se estiver deferido):
   1. Rode `content-types.get` para `portfolio` e confira o `supports` e o `rest_base` da taxonomia `categoria-portfolio`.
   2. Rode `content-terms.list` da taxonomia `categoria-portfolio` para achar o ID da categoria indicada no brief.
   3. Crie com `content-items.create`, `post_type: "portfolio"`, **`status: "draft"`**, título, slug, excerpt, conteúdo do `case.html` e a categoria. Envie a imagem destacada com `media.create` quando houver.
   4. Confira o campo `_content_warnings` na resposta. O conteúdo é um bloco HTML personalizado, então avise se o WordPress removeu algo.
   5. Devolva os links de edição e de preview. Nunca publique.
6. **Registro** — adicione o case à tabela "Portfólio (cases)" de `PUBLICACOES.md` com pasta, título, slug, ID e status.

Ao final, entregue um resumo curto: arquivos gerados, o que foi enviado ao WordPress, o que ainda precisa ser feito à mão (Rank Math, revisão do cliente, botão Publicar) e qualquer dado do brief que ficou de fora por falta de confirmação.
