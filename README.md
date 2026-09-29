# Blog com Claude Code e WordPress

Modelo de projeto para produzir posts de blog com o [Claude Code](https://claude.com/claude-code): do calendário editorial ao rascunho no WordPress.

Você informa o tema. O Claude gera outline, texto, imagens, schema, checagem de SEO e de fatos, e sobe o post como **rascunho**. A publicação final é sempre manual.

O projeto vem **sem nenhuma marca**. Você configura o nome da empresa, o público, o tom de voz e as regras editoriais uma vez (com o comando `/iniciar`), e a partir daí todos os comandos seguem a sua configuração. Os textos e os comandos estão em português do Brasil, mas dá para trabalhar em outro idioma: defina o idioma no `CLAUDE.md`.

## O que você precisa

| Item | Para quê |
|---|---|
| [Claude Code](https://claude.com/claude-code) (app desktop ou CLI) | Rodar os comandos do projeto |
| Plugin `claude-blog` | Fornece os comandos `/blog ...` usados pelo `/post` |
| Site WordPress com Jetpack e conta no WordPress.com, e o conector do WordPress.com no Claude | Criar rascunhos e subir imagens (veja [a seção do WordPress](#conectar-ao-wordpress-jetpack)) |
| Chave gratuita da API do Pexels | Baixar as imagens dos posts |
| Git | Clonar e versionar o projeto |

## Começando

**1. Use este repositório como modelo** (botão *Use this template* no GitHub) ou clone-o numa pasta fora do OneDrive (o OneDrive pode corromper a pasta `.git`):

```bash
git clone https://github.com/MatheusXavier1/BlogsCode.git
```

**2. Crie a sua chave do Pexels** em pexels.com/api e salve, só com a chave e sem espaços, em:

```
.claude/config/pexels_api_key.txt
```

Essa pasta está no `.gitignore`. Cada pessoa usa a própria chave, e ela nunca vai para o repositório.

**3. Instale o plugin `claude-blog`.** Uma cópia fixa dele está em `vendor/claude-blog/` (veja a seção [Plugin claude-blog](#plugin-claude-blog)). No Claude Code, dentro da pasta do projeto:

```
/plugin marketplace add ./vendor/claude-blog
/plugin install claude-blog@agricidaniel-blog
```

**4. Conecte o WordPress** seguindo a seção [Conectar ao WordPress (Jetpack)](#conectar-ao-wordpress-jetpack). As credenciais ficam com você, nunca em arquivos do projeto.

**5. Abra a pasta no Claude Code e rode o comando de iniciação:**

```
/iniciar
```

O Claude confere os pré-requisitos (chave do Pexels, plugin e WordPress), conecta o seu site, lê o `blog_id` e as categorias, e pergunta sobre a sua empresa, o público, as regras editoriais, as categorias e o tom de voz, e preenche `CLAUDE.md`, `BRAND.md` e `VOICE.md`. Se preferir, edite os três à mão: todo campo a preencher aparece como `{{...}}`.

**6. Planeje um mês de temas e produza o primeiro post:**

```
/calendario "seu nicho ou tema" mensal
/post "título do primeiro post"
```

## O que personalizar

| Arquivo | O que definir |
|---|---|
| `CLAUDE.md` | Nome da marca, descritor, domínio, caminho do blog, `blog_id`, idioma, termos proibidos |
| `BRAND.md` | Público, posicionamento, regras editoriais, categorias, escopo de temas |
| `VOICE.md` | Tom de voz, pessoa, formalidade, exemplos |
| `PUBLICACOES.md` | Começa vazio. O `/post` preenche a cada rascunho |
| `portfolio/_modelo/case.html` | Cores (variáveis `--accent*` no topo do CSS), fonte e textos do layout de cases |
| `.claude/commands/*.md` | Se o seu site usar outros nomes de tipo/taxonomia, ou se você quiser mudar etapas do pipeline |

## Comandos

| Comando | O que faz |
|---|---|
| `/iniciar` | Primeiro uso: confere os pré-requisitos, conecta o WordPress e configura a marca |
| `/configurar` | Reajusta só a marca (`CLAUDE.md`, `BRAND.md`, `VOICE.md`), por perguntas |
| `/calendario <nicho> [mensal\|trimestral]` | Gera `calendario-editorial.md` |
| `/post "<tópico>"` | Pipeline completo: outline, texto, 4 imagens, SEO, schema, fact-check e rascunho no WordPress |
| `/case <slug>` | Cria um case de portfólio a partir de um brief e sobe como rascunho (opcional; veja [Portfólio](#portfólio-cases)) |

O passo a passo detalhado, os critérios de SEO e a checklist antes de publicar estão no [MANUAL-WORKFLOW.md](MANUAL-WORKFLOW.md).

## Estrutura

```
CLAUDE.md                  Configuração da marca e regras que o Claude segue
BRAND.md / VOICE.md        Posicionamento, público e tom de voz
MANUAL-WORKFLOW.md         Manual de uso
PUBLICACOES.md             Registro do que está no ar (título, slug real, ID, status)
outlines/                  Estrutura de cada artigo
posts/<slug>/              Posts em produção (html, txt, schema.json, images/)
published/<slug>/          Posts já publicados, com o slug real do WordPress
portfolio/_modelo/         Modelo de case (case.html) e de brief (brief.md)
portfolio/<slug>/          Cases em produção (o status fica em PUBLICACOES.md)
vendor/claude-blog/        Cópia do plugin claude-blog
.claude/commands/          Comandos /iniciar, /configurar, /post, /calendario e /case
.claude/skills/            Auditoria SEO + GEO
```

## Conectar ao WordPress (Jetpack)

O Claude fala com o site por meio do conector do WordPress.com, que usa o plugin **Jetpack** para ligar o site a uma conta do WordPress.com. Não precisa de senha do wp-admin nem de plugin extra.

**O que precisa estar pronto**

1. **Jetpack ativo e conectado no site.** No wp-admin, em Jetpack, o site deve aparecer como conectado a uma conta do WordPress.com.
2. **Sua conta no WordPress.com** com papel de **Editor** ou superior no site, o mínimo para criar posts e enviar mídia.
3. **O conector do WordPress.com ligado no Claude**, com essa conta. Nas configurações de conectores do Claude, adicione o WordPress.com e autorize.

**Como conferir que funcionou**

Peça ao Claude: *"liste meus sites do WordPress.com"*. O seu site deve aparecer com o `blog_id`. Anote esse número em `CLAUDE.md` e `PUBLICACOES.md`. Depois peça: *"liste os posts publicados desse site"*.

**O que o Claude faz e não faz**

- Lê posts, páginas, mídia, categorias e o tipo Portfolio.
- Cria rascunhos, envia imagens e define a imagem destacada. **Toda criação, edição ou exclusão pede a sua confirmação antes de rodar.**
- Só cria como rascunho. Publicar é sempre manual, no wp-admin.
- **Não consegue** preencher os campos do Rank Math (título SEO, descrição e palavra-chave foco). Eles são descartados pela API e precisam ser preenchidos à mão.
- A exclusão pela API só manda para a lixeira. Apagar de vez também é no wp-admin.

**Se der erro**

| Sintoma | O que verificar |
|---|---|
| O site não aparece na lista | A conta autorizada no conector tem acesso ao site? O Jetpack está conectado? |
| Consegue ler, mas não criar | O seu papel no site é Editor ou superior? |
| Mais de um site na lista | O `/post` pergunta qual usar |
| Falha no envio de imagem | Informe o erro ao Claude. Ele não tenta contornar; suba a imagem manualmente na biblioteca de mídia |

## Portfólio (cases)

Opcional. Só funciona se o seu site tiver o tipo de conteúdo **Portfolio** (do Jetpack) com a taxonomia `categoria-portfolio`. Se o seu tema usa outros nomes, ajuste `.claude/commands/case.md`. Se você não usa portfólio, ignore o `/case` e a pasta `portfolio/`.

```
portfolio/
├── _modelo/
│   ├── case.html      Layout padrão do case
│   └── brief.md       Formulário com os fatos do projeto
└── <slug>/
    ├── brief.md       Fatos confirmados, com a autorização do cliente
    ├── case.html      Texto final, gerado a partir do modelo
    └── images/        Fotos do projeto (opcional)
```

Ao contrário do blog, os cases ficam **em uma pasta só**. O status fica na tabela "Portfólio" do [PUBLICACOES.md](PUBLICACOES.md).

**Como criar um case**

1. Crie `portfolio/<slug>/brief.md` copiando `portfolio/_modelo/brief.md` e preencha com os dados reais, incluindo a **autorização do cliente** para divulgar.
2. Rode `/case <slug>`.
3. O Claude gera o `case.html`, confere as regras e sobe como **rascunho**.
4. Revise, preencha o Rank Math e publique manualmente.

**Regras de case**

- Nenhum número, cliente ou depoimento é inventado: tudo vem do brief. O que faltar vira pergunta.
- Sem autorização do cliente no brief, o `/case` não cria nem rascunho.
- Se o cliente não autorizar o nome, o texto usa "empresa do setor X".

## Regras que valem para todo post

- O nome da marca é sempre escrito do mesmo jeito.
- Termos proibidos (siglas internas, promessas indevidas) não aparecem em texto publicado.
- Nenhuma estatística sem fonte verificada.
- Todo post é criado como rascunho no WordPress. Quem revisa é quem clica em Publicar.

A lista completa está no [CLAUDE.md](CLAUDE.md).

## Plugin claude-blog

O `/post` depende do plugin [claude-blog](https://github.com/AgriciDaniel/claude-blog), de AgriciDaniel, distribuído sob licença MIT. Uma cópia dele está em `vendor/claude-blog/`, na versão do commit `aec971a` (23/07/2026), com o `LICENSE` original. Isso garante que todos usem a mesma versão.

- **Não edite** os arquivos em `vendor/`. Ajustes do seu fluxo vão em `.claude/commands/` e `.claude/skills/`.
- **Para atualizar:** substitua o conteúdo de `vendor/claude-blog/` pela versão nova do repositório original, atualize o commit citado acima e teste um `/post` antes de fazer o commit.

## Boas práticas de equipe

1. Crie uma branch por post: `git checkout -b post/<slug>`.
2. Rode `/post` e revise o resultado.
3. Abra um pull request para outra pessoa revisar antes de publicar.
4. Depois de publicado no WordPress, mova a pasta de `posts/<slug>/` para `published/<slug-real>/`, **renomeando para o slug que ficou no ar** (o `.html`, o `.txt` e o outline também), e registre título, slug e ID em [PUBLICACOES.md](PUBLICACOES.md).

## Cuidados

- Nunca faça commit de chaves, tokens ou senhas.
- Se você usar este modelo numa empresa, **mantenha o repositório privado** ou não versione `posts/`, `published/`, `outlines/`, `PUBLICACOES.md` e `portfolio/`: eles guardam conteúdo ainda não divulgado, IDs internos e dados de clientes.
- Os slugs publicados no WordPress podem diferir dos planejados. Use sempre o slug real de [PUBLICACOES.md](PUBLICACOES.md) nos links internos.
- O Rank Math (título SEO, descrição e palavra-chave foco) não é preenchido pela API. Preencha manualmente no wp-admin.

## Licença

O conteúdo deste modelo (comandos, skill, documentos e modelos) usa a licença [MIT](LICENSE). O plugin em `vendor/claude-blog/` mantém a licença MIT original do seu autor.
