---
description: Cria um post de blog completo (pesquisa, escrita, SEO, schema, fact-check, imagem) pronto para publicar no WordPress
argument-hint: <tópico ou palavra-chave>
---

Você vai produzir um post de blog completo para publicação no WordPress, usando a skill `claude-blog`. Tópico/palavra-chave fornecido pelo usuário: $ARGUMENTS

**Configuração da marca:** leia a tabela "Configuração da marca" e a seção "Regras de escrita" do `CLAUDE.md`. Se ainda houver `{{...}}` nesses campos, **pare** e peça ao usuário para rodar `/configurar`.

Antes de começar, leia `BRAND.md` e `VOICE.md` na raiz do projeto (se existirem) para posicionamento, público e tom de voz. Leia também `PUBLICACOES.md`: **links internos só podem apontar para posts da tabela "Blog: publicados", usando o slug real dela**. Posts que existem apenas em `posts/` (não publicados) não podem ser linkados, porque a URL ainda não existe.

**Regras de escrita obrigatórias (valem para outline, HTML, .txt, schema, título, meta e excerpt):**
- Chame a empresa sempre pelo **nome da marca** do `CLAUDE.md`, escrito de um único jeito (inclusive no `author`/`name` do schema.json). Inclua o descritor obrigatório, se houver.
- **Nunca use os termos proibidos** do `CLAUDE.md` em texto publicado. Eles só podem aparecer em notas internas (outline/BRAND.md).
- Antes de entregar, rode um grep no HTML, no .txt e no schema.json pelos termos proibidos e por variações erradas do nome da marca, e corrija qualquer ocorrência.

Siga esta pipeline, nessa ordem, sem pular etapas:

1. **Outline** — rode `/blog outline $ARGUMENTS` para gerar a estrutura SERP-informada do artigo. Salve em `outlines/<slug>-outline.md`.
2. **Write** — rode `/blog write $ARGUMENTS`, usando o outline gerado, mínimo de 1700 palavras no corpo (alvo ~1800-2100), para passar no SEO check. Formato de saída: HTML compatível com WordPress (não MDX). Inclua 2-3 links internos reais para posts irmãos **já publicados** (slug real de `PUBLICACOES.md`, URL no formato `https://<domínio>/<caminho do blog>/<slug-real>`, conforme o `CLAUDE.md`). Salve em `posts/<slug>/<slug>.html` (crie a pasta `posts/<slug>/` primeiro; slug = título em kebab-case sem acentos).
3. **Imagens (Pexels) — 4 por post, todas com alt text** — 1 capa + 3 inline, distribuídas ao longo do artigo (não agrupadas). Para cada uma:
   ```bash
   KEY=$(cat "$(pwd)/.claude/config/pexels_api_key.txt" | tr -d '\r\n')
   QUERY="<2-3 palavras em inglês descrevendo a cena>"
   mkdir -p "posts/<slug>/images"
   RESPONSE=$(curl -s -H "Authorization: $KEY" "https://api.pexels.com/v1/search?query=$(echo "$QUERY" | sed 's/ /%20/g')&per_page=1&orientation=landscape")
   ```
   Extraia com `grep -oP` (não `sed` com `/` no meio do padrão, quebra): `'"large":"\K[^"]*'` para a URL, `'"photographer":"\K[^"]*'` e `'"url":"\Khttps://www.pexels.com[^"]*'` para o crédito. Baixe cada imagem com `curl -s -o "posts/<slug>/images/<nome>.jpg" "<src.large>"` (nomes: `cover.jpg`, `inline1.jpg`, `inline2.jpg`, `inline3.jpg`).
   Distribuição recomendada: capa logo após o H1; inline1 perto do 2º-3º H2 (ex.: onde o artigo lista exemplos/casos práticos); inline2 perto do meio (seção de processo/decisão); inline3 perto do 6º-7º H2 (seção de risco/governança/conclusão prática). Nunca duas imagens seguidas sem parágrafo de texto entre elas.
   **Todo `<img>` precisa de `alt` descritivo contendo a focus keyword (ou uma variação natural dela)** — este é um teste que o RankMath verifica e que travou pontuação em posts anteriores. Nunca deixe um `<img>` sem `alt`.
   Grave no comentário do topo do arquivo: `coverImage`, `coverImageAlt`, `imageCredit` (capa) e, opcionalmente, uma lista `inlineImageCredits` com fotógrafo + URL de cada imagem inline — não deixe os créditos das inline soltos apenas na memória da conversa.
   Se uma busca não retornar foto, tente uma query mais genérica; se ainda assim falhar, deixe placeholder nessa posição específica e avise no resumo final quais das 4 faltaram.
4. **SEO check** — rode `/blog seo-check posts/<slug>/<slug>.html` e valide TODOS os itens abaixo como pass/fail, aplicando as correções diretamente no HTML antes de seguir:
   1. **Palavra-chave foco (PC) definida** — frase de 2-4 palavras, a mesma do outline.
   2. **PC no título** (H1 e SEO title).
   3. **PC no início do SEO title** — o título deve começar com a PC.
   4. **Número no SEO title** — quando houver forma natural de encaixar (itens/passos/sinais do artigo); nunca invente número.
   5. **Meta descrição contém a PC** (e fica em ~120-160 caracteres).
   6. **PC na URL/slug**.
   7. **PC nos primeiros 10% do conteúdo** — no 1º parágrafo, dentro dos primeiros 10% das palavras do corpo.
   8. **PC encontrada no conteúdo** — ocorre no corpo do texto visível.
   9. **Conteúdo com mais de 1700 palavras** (contar só o corpo visível).
   10. **PC em pelo menos um subtítulo** (H2/H3).
   11. **PC no `alt` de imagens** — todo `<img>` com alt descritivo contendo a PC ou variação natural.
   12. **Densidade da PC alta o suficiente** — mínimo ~1% (meta 1-1,5%) contando só o texto visível do corpo; reporte nº de ocorrências e palavras.
   13. **Links externos** — ao menos 1 link externo relevante, com `rel="noopener noreferrer"`.
   14. **Parágrafos curtos** — nenhum acima de ~120 palavras; prefira 2-4 frases.
   15. **Contém imagens** — capa + 3 inline (4 no total).
   Reporte a tabela pass/fail no resumo final e só avance com todos os itens em pass (ou justifique explicitamente os que são impossíveis).
5. **Auditoria SEO-GEO (RankMath + IA generativa)** — invoque a skill `seo-geo-blog-optimizer` (`.claude/skills/seo-geo-blog-optimizer/SKILL.md`) passando o texto do artigo e a focus keyword (a keyword primária definida no outline do passo 1 — prefira uma frase de 2-4 palavras, não o título inteiro). Ela roda os testes de pontuação estilo Rank Math e os princípios de GEO. Regras aprendidas que a auditoria deve sempre verificar:
   - **Densidade de keyword conta só o texto visível do corpo** (parágrafos, headings, listas, tabelas) — texto de `alt` de imagem, comentário do topo do arquivo e meta tags NÃO contam para o cálculo do RankMath. Para subir densidade, edite frases do corpo, não só os metadados.
   - **Meta title (SEO title) precisa conter um número** sempre que houver uma forma natural de encaixar um (contagem de itens/passos/sinais já presente no artigo, ex.: "4 Usos", "6 Indicadores") — não invente um número que não reflita o conteúdo.
   - **Nenhum parágrafo pode passar de ~120-150 palavras** — quebre os mais longos em dois.
   - **Links externos**: usar `rel="noopener noreferrer"`, nunca `nofollow`, a menos que o usuário peça explicitamente. Se o RankMath reportar nofollow mesmo com o HTML correto, é uma configuração do lado do WordPress/RankMath (ex.: "Nofollow para links externos" nas configurações gerais do plugin) — sinalize isso no resumo em vez de tentar consertar no HTML.
   - **4 imagens com alt text contendo a keyword** (verificado no passo 3).
   Aplique as correções diretamente no HTML (não deixe como sugestão à parte) — inclusive a meta title e meta description sugeridas, que substituem as do passo de escrita se forem mais fortes. Guarde o diagnóstico 🔴🟡🟢 retornado pela skill para incluir no resumo final.
6. **Schema** — rode `/blog schema posts/<slug>/<slug>.html`. No `ImageObject`, use a URL local (`images/cover.jpg`) ou a URL original da Pexels se o post for hospedado sem re-upload. Salve em `posts/<slug>/schema.json`.
7. **Fact-check** — rode `/blog factcheck posts/<slug>/<slug>.html`. Para cada claim com URL, use WebFetch para confirmar que a fonte realmente diz o que o texto afirma. Se uma URL redirecionar para um domínio/empresa diferente do esperado, corrija a atribuição no texto. Descarte (não invente) qualquer estatística que não conseguir verificar diretamente na fonte.
8. **Versão .txt** — gere `posts/<slug>/<slug>.txt` com o conteúdo do artigo em texto puro (sem tags HTML, headings marcados com linhas em maiúsculas ou `##`, mantendo parágrafos e a seção de FAQ), para leitura rápida ou colagem em editores que não aceitam HTML.
9. **Nota final** — rode `/blog analyze posts/<slug>/<slug>.html` e calcule a nota honesta 0-100 nas 5 categorias do claude-blog. NÃO force a nota para 90+ nem repita reescritas para tentar empurrar itens que são estruturais (autor pessoa física com bio, tags OG/canonical que só existem após publicar no WordPress). Reescreva (máx. 2 iterações) apenas se houver problema real de conteúdo (claim não verificado, erro factual, hierarquia de heading quebrada, ou 🔴 crítico do passo 5 que não foi corrigido).
10. **Publicar como rascunho (WordPress.com / Jetpack MCP)** — use as ferramentas do MCP do WordPress.com (`wpcom-user-sites`, `wpcom-mcp-content-authoring`; carregue-as via ToolSearch se estiverem deferidas). Só rode depois que os passos 1-9 estiverem concluídos.
    1. Identifique o site do blog com `wpcom-user-sites` (se houver mais de um, pergunte ao usuário qual usar antes de continuar).
    2. Faça upload das 4 imagens de `posts/<slug>/images/` para a biblioteca de mídia, com o `alt` de cada uma. Guarde o ID e a URL de cada mídia; defina a capa (`cover.jpg`) como imagem destacada.
    3. Crie o post com **status `draft`** (nunca `publish`), usando o conteúdo do HTML (sem o comentário do topo), trocando os `src` locais das imagens pelas URLs da mídia enviada. Preencha título, slug, excerpt/meta description e SEO title/focus keyword quando o site permitir; defina categoria/tags se já existirem no site e forem óbvias.
    4. Insira o JSON-LD de `schema.json` no post (bloco HTML personalizado) ou avise no resumo que precisa ser colado manualmente, caso o site não aceite.
    5. Confirme que o post ficou como rascunho e devolva o link de edição e o de preview. Se algum passo falhar (permissão, upload, campo não suportado), não tente contornar: informe o que faltou.
    6. Registre o rascunho na tabela "Blog: rascunhos no WordPress" de `PUBLICACOES.md` (pasta, título, slug e ID).

Ao final, entregue um resumo curto para o usuário com:
- Caminho dos arquivos gerados (`posts/<slug>/`: `.html`, `.txt`, `schema.json`, `images/cover.jpg` + `inline1/2/3.jpg`)
- Crédito de cada uma das 4 imagens (fotógrafo + link Pexels)
- Diagnóstico SEO-GEO (passo 5): itens 🔴/🟡 encontrados e já corrigidos, focus keyword usada, densidade final (com nº de ocorrências e contagem de palavras do corpo)
- Nota final com breakdown por categoria, separando gaps estruturais (pré-publicação) de gaps de conteúdo reais
- Link de edição e de preview do rascunho no WordPress, e o que foi enviado (post, 4 imagens, imagem destacada, schema)
- Checklist do que falta fazer manualmente antes de publicar (o que o passo 10 não conseguiu preencher, como schema, categoria/tags/autor; conferir a configuração de nofollow em links externos nas opções do RankMath; revisar o rascunho e clicar em Publicar)

Nunca publique de fato — o passo 10 cria apenas **rascunho**. A publicação final é sempre manual, feita pelo usuário.
