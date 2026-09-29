---
description: Primeiro uso do projeto: confere os pré-requisitos, conecta o WordPress e configura a sua marca
argument-hint: (sem argumentos)
---

Você vai conduzir o primeiro uso deste projeto, em ordem. Faça uma etapa por vez, diga o que encontrou e só avance quando ela estiver resolvida (ou o usuário decidir pular). Fale em português, de forma curta.

**Regras:** nunca peça nem escreva chaves, senhas ou tokens no chat ou em arquivos do projeto. Nunca crie, edite ou apague nada no WordPress nesta etapa: só leia. Não invente dados da empresa.

## 1. Diagnóstico

Rode as checagens abaixo e mostre uma tabela com ✅ / ❌ para cada item:

| Item | Como checar |
|---|---|
| Chave do Pexels | O arquivo `.claude/config/pexels_api_key.txt` existe e não está vazio (`test -s`). **Não imprima o conteúdo.** |
| Plugin `claude-blog` | Os comandos `/blog ...` estão disponíveis (ferramenta `ListPlugins`, se estiver deferida carregue com ToolSearch), ou a skill `blog` aparece na lista de skills. |
| Conector do WordPress.com | A ferramenta `wpcom-user-sites` está disponível e responde. |
| `.gitignore` | Contém `.claude/config/` e `.claude/settings.local.json`. |
| Configuração da marca | Ainda há `{{...}}` em `CLAUDE.md`, `BRAND.md`, `VOICE.md` (`grep -c "{{" ...`). |

## 2. Resolver o que faltar

- **Chave do Pexels:** explique que ela é gratuita em pexels.com/api. Crie a pasta `.claude/config/` (`mkdir -p`) e peça ao usuário para salvar a chave, só ela e sem espaços, em `.claude/config/pexels_api_key.txt`, por conta própria. Confira de novo com `test -s`.
- **Plugin `claude-blog`:** peça ao usuário para rodar, dentro da pasta do projeto:
  ```
  /plugin marketplace add ./vendor/claude-blog
  /plugin install claude-blog@agricidaniel-blog
  ```
  Depois de instalar, pode ser preciso reiniciar a sessão. Avise isso.
- **WordPress.com:** explique os requisitos (Jetpack ativo e conectado ao WordPress.com, papel de Editor ou superior, conector do WordPress.com ligado no Claude). Se o usuário ainda não tem site WordPress, avise que o `/post` vai gerar os arquivos, mas não vai conseguir criar o rascunho, e pergunte se quer continuar assim.

## 3. Conectar ao seu site

Se o conector estiver ativo: rode `wpcom-user-sites`, mostre os sites e pergunte qual é o do blog. Guarde o `blog_id` e o domínio. Não invente: se a lista vier vazia, volte ao item do WordPress.

Depois rode uma leitura só de consulta no site escolhido (por exemplo, `wpcom-mcp-content-authoring` com a lista de categorias) e guarde os nomes das categorias existentes para a etapa 5.

## 4. Configurar a marca

Siga o fluxo de `.claude/commands/configurar.md` (perguntas sobre marca, público, regras, categorias, escopo e tom de voz, e preenchimento de `CLAUDE.md`, `BRAND.md` e `VOICE.md`). Já preencha `blog_id` e domínio com o que você obteve na etapa 3, sem perguntar de novo. Se o usuário quiser voltar a ajustar só a marca depois, ele roda `/configurar`.

## 5. Categorias e portfólio

- Compare as categorias definidas em `BRAND.md` com as que existem no WordPress (etapa 3). Liste as que faltam e avise que o usuário deve criá-las no wp-admin (ou peça para o `/post` sugerir as existentes). Não crie nada.
- Pergunte se o site usa o tipo **Portfolio** (Jetpack). Se não usa, avise que pode ignorar `/case` e a pasta `portfolio/`. Se usa, confira com `content-types.get` que o tipo `portfolio` e a taxonomia `categoria-portfolio` existem e, se os nomes forem outros, ajuste `.claude/commands/case.md`.

## 6. Fechamento

1. Rode `grep -rn "{{" CLAUDE.md BRAND.md VOICE.md PUBLICACOES.md` e liste o que ficou pendente.
2. Preencha em `PUBLICACOES.md` o site e o `blog_id`.
3. Entregue um resumo curto: tabela final de pré-requisitos, o que foi configurado, o que ficou "a definir" e o que o usuário ainda precisa fazer à mão.
4. Sugira o próximo passo: `/calendario "<nicho>" mensal` para planejar temas, ou `/post "<tópico>"` para o primeiro post.
5. Lembre que nada foi criado no WordPress, e que o `/post` só cria **rascunhos**.
