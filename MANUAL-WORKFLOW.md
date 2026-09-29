# Manual do Workflow de Blog

Este manual explica como produzir posts de blog do zero até o rascunho no WordPress, usando os comandos do Claude Code deste projeto. Antes de tudo, configure o projeto para a sua marca (seção 0).

---

## 0. Configuração inicial (uma vez)

1. Rode `/configurar` no Claude Code. Ele pergunta sobre a sua marca, o seu público, as regras editoriais e o tom de voz, e preenche `CLAUDE.md`, `BRAND.md` e `VOICE.md`. Se preferir, edite os três arquivos à mão: todo campo a preencher aparece como `{{...}}`.
2. Salve a chave do Pexels em `.claude/config/pexels_api_key.txt`.
3. Instale o plugin `claude-blog` e conecte o WordPress (veja o README).

Enquanto restar algum `{{...}}` nos campos de marca do `CLAUDE.md`, os comandos `/post` e `/case` pedem para você configurar antes.

---

## 1. Visão geral

| Comando | Para que serve | Resultado |
|---|---|---|
| `/configurar` | Personalizar o projeto para a sua marca | `CLAUDE.md`, `BRAND.md`, `VOICE.md` preenchidos |
| `/calendario <nicho> [mensal\|trimestral]` | Planejar os temas do período | `calendario-editorial.md` na raiz |
| `/post "<título ou palavra-chave>"` | Produzir um post completo | Pasta `posts/<slug>/` + rascunho no WordPress |
| `/case <slug>` | Produzir um case de portfólio (opcional) | `portfolio/<slug>/` + rascunho no WordPress |

Fluxo recomendado:

```
/configurar  →  /calendario  →  escolher tema  →  /post  →  revisar rascunho  →  publicar (manual)  →  mover para published/
```

---

## 2. Estrutura de pastas

```
<projeto>/
├── BRAND.md / VOICE.md        Posicionamento, público e tom de voz (lidos automaticamente)
├── CLAUDE.md                  Configuração da marca e regras do projeto
├── calendario-editorial.md    Gerado pelo /calendario
├── outlines/                  Estrutura de cada artigo (<slug>-outline.md)
├── posts/<slug>/              Posts em produção
│   ├── <slug>.html            Artigo em HTML para WordPress
│   ├── <slug>.txt             Versão em texto puro
│   ├── schema.json            Dados estruturados (JSON-LD)
│   └── images/                cover.jpg + inline1/2/3.jpg
├── published/<slug-real>/     Posts já publicados (mesma estrutura, slug igual ao do WordPress)
├── PUBLICACOES.md             Registro do que está no ar e dos rascunhos
├── portfolio/                 Cases: _modelo/ e <slug>/ (comando /case)
└── .claude/
    ├── commands/              configurar.md, post.md, calendario.md e case.md
    ├── skills/seo-geo-blog-optimizer/   Auditoria SEO + GEO
    └── config/pexels_api_key.txt        Chave da API de imagens (não versionada)
```

**Convenção do slug:** título em kebab-case, sem acentos (ex.: `quando-vale-desenvolver-um-saas-proprio`).

---

## 3. Passo a passo

### 3.1 Planejar com `/calendario`

```
/calendario "seu nicho ou tema" trimestral
```

- Sem período informado, o padrão é mensal.
- Se ainda não existir estratégia para o nicho, o comando sugere rodar `/blog strategy` antes.
- Ao final, lista os títulos planejados. Cada um pode virar post com `/post "<título>"`.

### 3.2 Produzir com `/post`

```
/post "como montar um dashboard de indicadores"
```

Basta informar o tópico ou a palavra-chave. O comando executa 10 etapas em ordem:

| # | Etapa | O que acontece |
|---|---|---|
| 1 | **Outline** | Estrutura SERP-informada, salva em `outlines/` |
| 2 | **Escrita** | HTML com mínimo de 1700 palavras (alvo 1800-2100) e 2-3 links internos para posts já publicados |
| 3 | **Imagens** | 4 imagens do Pexels (1 capa + 3 inline), todas com `alt` contendo a palavra-chave |
| 4 | **SEO check** | Checklist pass/fail de 15 itens, com correção direta no HTML |
| 5 | **Auditoria SEO-GEO** | Pontuação estilo Rank Math + otimização para ChatGPT, Perplexity e Google AI Overviews |
| 6 | **Schema** | `schema.json` (JSON-LD) |
| 7 | **Fact-check** | Cada estatística com URL é conferida na fonte; o que não for verificável é descartado |
| 8 | **Versão .txt** | Texto puro para leitura rápida |
| 9 | **Nota final** | Nota honesta de 0 a 100, separando gaps estruturais de gaps de conteúdo |
| 10 | **Rascunho no WordPress** | Sobe imagens, cria o post como `draft` e devolve links de edição e preview |

Nos primeiros posts, não há posts publicados para linkar. Nesse caso o `/post` não cria links internos e avisa no resumo.

### 3.3 Revisar e publicar

O workflow **nunca publica sozinho**. Depois que ele entregar o resumo:

1. Abra o link de edição do rascunho.
2. Confira a checklist final do resumo (ver seção 6).
3. Revise texto, imagens e links.
4. Clique em **Publicar** manualmente.
5. Confira o slug que ficou no ar. Mova a pasta de `posts/<slug>/` para `published/<slug-real>/`, renomeando a pasta, o `.html`, o `.txt` e o outline para o slug real.
6. Atualize `PUBLICACOES.md`: mova o post da tabela de rascunhos para a de publicados, com título, slug real, ID e data.

---

## 4. Regras de escrita (aplicadas automaticamente)

As regras vivem no `CLAUDE.md` (seção "Regras de escrita") e no `BRAND.md`. Por padrão:

- O nome da marca é escrito sempre do mesmo jeito, inclusive em `author` e `name` do schema.
- Os termos proibidos que você definir não aparecem em texto publicado. O workflow roda um `grep` por eles antes de entregar.
- Links internos usam o formato `https://<seu-domínio>/<caminho-do-blog>/<slug-irmão>`.
- Links externos usam `rel="noopener noreferrer"`, sem `nofollow`.
- Nenhuma estatística sem fonte verificada.

---

## 5. Critérios de SEO que o post precisa cumprir

Palavra-chave foco (PC) de 2 a 4 palavras, presente em:

- H1 e SEO title (**no início** do título)
- Meta descrição (120-160 caracteres)
- URL/slug
- Primeiro parágrafo (nos primeiros 10% do texto)
- Ao menos um H2/H3
- `alt` de todas as imagens (ajuda no teste do Rank Math, mas **não conta** na densidade)

Demais requisitos:

- Mais de 1700 palavras no corpo visível
- Densidade da PC de ~1% a 1,5% (só texto visível do corpo conta)
- Número no SEO title, quando houver forma natural (nunca inventar)
- Parágrafos de no máximo ~120 palavras (ideal: 2-4 frases)
- Pelo menos 1 link externo relevante
- 4 imagens no total

---

## 6. Checklist manual antes de publicar

O passo 10 não consegue preencher tudo. Confira no WordPress:

- [ ] Schema JSON-LD colado (se o site não aceitou automaticamente)
- [ ] Categoria, tags e autor
- [ ] Focus keyword, SEO title e meta description no Rank Math
- [ ] Configuração de nofollow em links externos (Rank Math → Configurações gerais → "Nofollow para links externos"): se o Rank Math reportar nofollow mesmo com o HTML correto, o ajuste é lá
- [ ] Tags OG e canonical (só existem depois de publicar)
- [ ] Imagem destacada = capa
- [ ] Leitura final do texto e teste dos links
- [ ] Status alterado de rascunho para **Publicado**

---

## 7. Dúvidas e problemas comuns

| Situação | O que fazer |
|---|---|
| O `/post` pede para rodar `/configurar` | Ainda há `{{...}}` na configuração da marca do `CLAUDE.md` |
| Uma imagem não veio do Pexels | O workflow tenta uma query mais genérica; se falhar, deixa placeholder e avisa no resumo quais faltaram |
| Densidade da PC baixa | Editar frases do **corpo** do texto. Mexer só em alt ou meta não adianta |
| Estatística sem fonte verificável | É removida no fact-check. Nada é inventado |
| Nota final abaixo de 90 | Normal. Gaps estruturais (autor pessoa física, OG, canonical) só se resolvem no WordPress. O workflow não força a nota |
| Mais de um site no WordPress.com | O workflow pergunta qual usar antes de criar o rascunho |
| Falha em upload ou permissão no WordPress | O workflow informa o que faltou e não tenta contornar. Suba manualmente os arquivos de `posts/<slug>/` |

---

## 8. Comandos avançados (skill claude-blog)

Úteis fora do `/post`, para posts já existentes:

| Comando | Uso |
|---|---|
| `/blog analyze <arquivo>` | Nota 0-100 em 5 categorias |
| `/blog seo-check <arquivo>` | Checklist on-page pass/fail |
| `/blog factcheck <arquivo>` | Verifica estatísticas e fontes |
| `/blog rewrite <arquivo>` | Reescreve/atualiza um post |
| `/blog audit` | Saúde do blog inteiro (canibalização, posts velhos, órfãos) |
| `/blog strategy` | Estratégia e clusters de tópicos |
| `/blog brief` | Brief detalhado de conteúdo |

Para otimizar um texto colado ou existente, peça: *"otimize este post para SEO e GEO, focus keyword: X"*. A skill `seo-geo-blog-optimizer` é acionada automaticamente.

---

## 9. Resumo rápido

1. `/configurar`: uma vez, para a sua marca.
2. `/calendario "<nicho>" trimestral`: define os temas.
3. `/post "<tema>"`: gera tudo, incluindo o rascunho no WordPress.
4. Leia o resumo final: arquivos, créditos das imagens, diagnóstico SEO-GEO, nota e pendências.
5. Faça a checklist manual, publique e mova para `published/`.
