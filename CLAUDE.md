# Blog com Claude Code e WordPress

Projeto de produção de posts de blog, do planejamento até o rascunho no WordPress. Os textos são escritos em **{{IDIOMA, ex.: português do Brasil}}**.

> **Primeiro uso:** este arquivo tem campos `{{...}}`. Rode `/configurar` para o Claude preencher este arquivo, o `BRAND.md` e o `VOICE.md` com você, ou edite à mão. Enquanto houver `{{...}}`, os comandos `/post` e `/case` pedem para você configurar antes.

## Configuração da marca
Os comandos leem estes valores. Mude aqui, não nos comandos.

| Campo | Valor |
|---|---|
| Nome da empresa/marca (como aparece nos textos) | {{NOME_DA_MARCA}} |
| Descritor obrigatório (uma frase, ou "nenhum") | {{DESCRITOR, ex.: "consultoria de engenharia para pequenas empresas"}} |
| Domínio do site | {{DOMINIO, ex.: exemplo.com.br}} |
| Caminho do blog | {{CAMINHO_DO_BLOG, ex.: /blog}} |
| URL base dos posts | `https://{{DOMINIO}}{{CAMINHO_DO_BLOG}}/<slug-real>` |
| Nome de autor no schema | {{AUTOR_NO_SCHEMA, normalmente o nome da marca}} |
| `blog_id` do WordPress.com | {{BLOG_ID, veja o README}} |
| Página de contato/CTA | {{URL_CONTATO, ex.: /contato}} |

## Leia antes de escrever
- `BRAND.md`: público, posicionamento, regras editoriais, categorias e escopo de temas.
- `VOICE.md`: tom de voz.
- `MANUAL-WORKFLOW.md`: passo a passo completo.

## Comandos do projeto
- `/configurar`: preenche este arquivo, `BRAND.md` e `VOICE.md` com você, por perguntas.
- `/calendario <nicho> [mensal|trimestral]`: gera `calendario-editorial.md`.
- `/post "<tópico>"`: pipeline de 10 etapas, do outline ao rascunho no WordPress (`.claude/commands/post.md`). É a fonte da verdade do processo; não duplique as etapas aqui.
- `/case <slug>`: cria um case de portfólio a partir de `portfolio/<slug>/brief.md` e sobe como rascunho. Nunca inventa números nem cliente, e exige a autorização do cliente no brief. Só faz sentido se o site tiver o tipo de conteúdo Portfolio.
- Skill `seo-geo-blog-optimizer` (`.claude/skills/`): auditoria SEO (Rank Math) e GEO. É acionada pelo `/post` e também quando o usuário pede para otimizar um texto.
- Os comandos `/blog ...` vêm do plugin `claude-blog`, que precisa estar instalado (veja o README).

## Estrutura de pastas
- `outlines/<slug>-outline.md`: estrutura de cada artigo.
- `posts/<slug>/`: post em produção (`<slug>.html`, `<slug>.txt`, `schema.json`, `images/` com `cover.jpg` e `inline1-3.jpg`).
- `published/<slug>/`: posts já publicados, mesma estrutura. **O nome da pasta é o slug real do WordPress.**
- `PUBLICACOES.md`: registro do que está no ar (título, slug real, ID, status) e dos rascunhos. É a fonte da verdade sobre status.
- `portfolio/_modelo/` e `portfolio/<slug>/`: cases (brief + `case.html`). Uma pasta só; o status fica em `PUBLICACOES.md`.
- Slug de post novo: título em kebab-case, sem acentos. Depois de publicado, se o WordPress ficou com outro slug, renomeie pasta, arquivos e outline para o slug real.

## Regras de escrita (valem para todo texto publicado)
As regras abaixo são um ponto de partida. Ajuste ao seu caso.
- O nome da marca é sempre o da tabela acima, escrito de um único jeito, inclusive em `author` e `name` do schema.
- **Termos proibidos** (siglas internas, nomes de clientes não autorizados, promessas que a empresa não pode fazer): {{TERMOS_PROIBIDOS, ex.: "sigla XYZ", ou "nenhum"}}. Antes de entregar, rode grep por eles no HTML, no `.txt` e no `schema.json`.
- Descritor obrigatório: se a tabela de configuração tiver um descritor, ele aparece ao menos uma vez no texto.
- Links internos: `https://{{DOMINIO}}{{CAMINHO_DO_BLOG}}/<slug-real>`. Links externos: `rel="noopener noreferrer"`, sem `nofollow`.
- **Nunca inventar estatísticas.** O que não for verificado na fonte é removido.

## WordPress
- Conexão via MCP do WordPress.com (site com Jetpack). Veja o README.
- **Sempre criar como `draft`. Nunca publicar.** A publicação é manual.
- **O slug que fica no ar pode diferir do planejado** (o WordPress encurta ou o editor muda). Links internos usam **sempre o slug real** de `PUBLICACOES.md`; nunca o slug do outline. Não linke para posts que ainda não foram publicados.
- **Ao criar um rascunho ou ao ver que um post foi publicado, atualize `PUBLICACOES.md` e mova/renomeie a pasta.** Se houver dúvida, consulte o WordPress: ele manda, o registro e as pastas seguem.
- Rascunhos antigos e duplicados no WordPress não devem ser reaproveitados sem conferir.
- Portfólio (opcional): tipo `portfolio`, taxonomia `categoria-portfolio`. Use `content-types.get`, `content-terms.list` e `content-items.*`, sempre como `draft`. Fluxo em `.claude/commands/case.md`. Se o seu site usa outro nome de tipo ou taxonomia, ajuste lá.
- Campos do Rank Math (`rank_math_title`, `rank_math_description`, `rank_math_focus_keyword`) **não são gravados pela API**. Preencher manualmente no wp-admin e avisar no resumo.
- Pendências manuais comuns: schema JSON-LD, categoria/tags/autor, nofollow nas configurações do Rank Math, imagem destacada, OG e canonical (só existem após publicar).

## Imagens
- 4 por post (1 capa + 3 inline), do Pexels, todas com `alt` contendo a palavra-chave foco. Registrar o crédito no comentário do topo do HTML.
- A chave da API fica em `.claude/config/pexels_api_key.txt`.

## Segurança e trabalho em equipe
- **Não versionar nem compartilhar `.claude/config/pexels_api_key.txt`.** Cada pessoa usa a própria chave. `.claude/config/` e `.claude/settings.local.json` já estão no `.gitignore`.
- Credenciais do WordPress ficam no conector de cada pessoa, nunca em arquivos do projeto.
- Se você usar este projeto para uma empresa, **não publique** os seus `posts/`, `published/`, `outlines/`, `PUBLICACOES.md` e `portfolio/` num repositório público sem revisar: eles contêm conteúdo ainda não divulgado, IDs internos e dados de clientes.
