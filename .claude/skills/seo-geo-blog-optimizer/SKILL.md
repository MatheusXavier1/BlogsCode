---
name: seo-geo-blog-optimizer
description: Audita e reescreve posts de blog aplicando os critérios de pontuação SEO 100/100 (baseados no Rank Math) e os princípios de GEO (Generative Engine Optimization — visibilidade em ChatGPT, Perplexity, Google AI Overviews, Gemini). Use esta skill sempre que o usuário pedir para "otimizar", "auditar", "revisar SEO", "melhorar ranqueamento", "pontuar" ou "preparar para IA" um post/artigo de blog, ou quando mencionar "[SEO-GEO]", "/otimizar-post", "focus keyword", "score 100", "content-length" ou "GEO". Também acione quando o usuário colar o texto de um post e pedir feedback de SEO, mesmo sem usar essas palavras exatas — qualquer pedido de revisão de artigo de blog voltado a ranqueamento ou tráfego orgânico é gatilho para esta skill.
---

# Otimizador de Blog Posts — SEO + GEO

Audita um post de blog existente contra os critérios de pontuação SEO (baseados no framework Rank Math 100/100) e os princípios de GEO (visibilidade em respostas de IA), e entrega uma **versão reescrita e pronta para publicar**.

## Quando usar

- Usuário pede para otimizar, auditar, revisar ou "dar nota" em um post de blog.
- Usuário cola o texto de um artigo e pede feedback de SEO/ranqueamento.
- Usuário aciona com `[SEO-GEO]` ou pede algo relacionado a "score 100", "focus keyword", "GEO", "IA generativa".

## Fluxo de trabalho

### 1. Colete o input

Você precisa de:
- **O texto do post** (colado, ou arquivo enviado)
- **Focus keyword (palavra-chave principal)** — se o usuário não informar, **tente inferir do próprio texto** (título, primeiro parágrafo, repetição de termos). Só pergunte diretamente se não for óbvio pelo conteúdo.
- **Secondary keywords (opcional)** — se existirem candidatas óbvias no texto, trate-as como secundárias; não é bloqueante.

Não pergunte mais do que isso. Não peça confirmação de esboço antes de rodar — essa skill entrega direto (é uma auditoria + reescrita, não uma peça de copy longa do zero).

### 2. Rode a auditoria SEO (checklist Rank Math)

Avalie o post contra cada teste abaixo. Para cada um, marque 🔴 (falhou), 🟡 (parcial) ou 🟢 (passou), com uma nota rápida do porquê.

**Básico (peso alto — reprovar aqui reprova o score geral):**
| Teste | Critério de aprovação |
|---|---|
| Keyword no título | Focus keyword aparece no título, idealmente nos primeiros 50% dos caracteres |
| Keyword na meta description | Aparece nos primeiros 120–160 caracteres da meta description |
| Keyword na URL/slug | Slug contém a focus keyword |
| Keyword no início do conteúdo | Aparece nos primeiros 10% do texto (ou nas primeiras 300 palavras se o post tiver menos de 300 palavras) |
| Keyword no corpo do texto | Focus keyword (e variações plural/singular) aparecem naturalmente ao longo do texto |
| Tamanho do conteúdo | 100% = 2500+ palavras · 70% = 2000–2500 · 60% = 1500–2000 · 40% = 1000–1500 · 20% = 600–1000 · 0% = <600 |

**Adicional:**
| Teste | Critério de aprovação |
|---|---|
| Keyword em subheadings (H2/H3) | Focus keyword aparece em pelo menos um subheading; secundárias também idealmente |
| Keyword em alt text de imagem | **Todas** as imagens do post (mínimo 4) com alt text descritivo contendo a focus keyword ou uma variação natural dela — nunca deixar um `<img>` sem `alt` |
| Densidade de keyword | Entre 1% e 1.5% (alerta se passar de 2.5%). **Conta só o texto visível do corpo** (parágrafos, headings, listas, tabelas) — alt text de imagem, comentário/frontmatter e meta tags NÃO entram nessa conta no Rank Math. Fórmula: (ocorrências da frase exata da keyword) / (total de palavras do corpo visível) × 100. Para subir densidade, edite frases do corpo do texto, nunca só os metadados |
| Tamanho da URL | ≤75 caracteres (URL completa) |
| Links externos | Pelo menos 1 link para fonte externa relevante e confiável (nunca concorrente direto) |
| Link externo "followed" | Pelo menos um dos links externos sem nofollow. Sempre gerar `rel="noopener noreferrer"`, nunca `rel="nofollow"`. Se o Rank Math ainda assim reportar nofollow com o HTML correto, é uma configuração do lado do WordPress/plugin (ex.: opção "Nofollow para links externos" nas configurações gerais do Rank Math) — sinalizar isso ao usuário em vez de tentar "consertar" um HTML que já está certo |
| Links internos | Pelo menos 1 link para outro conteúdo do mesmo site |
| Unicidade da keyword | Nenhuma outra página do mesmo site já mirando na mesma focus keyword (avise se não souber checar) |

**Legibilidade do título:**
| Teste | Critério de aprovação |
|---|---|
| Keyword no início do título | Nos primeiros 50% do título |
| Sentimento/emoção no título | Título evoca emoção (positiva ou negativa), sem virar clickbait vazio |
| Power word no título | Pelo menos uma palavra de poder ("essencial", "comprovado", "definitivo", "grátis", etc.) |
| Número no título | Título contém um número (ex: "7 formas de...") — priorize um número que já exista no conteúdo (quantidade de itens, passos, sinais listados no post) em vez de inventar um; se não houver nenhum número natural, sinalize a lacuna em vez de forçar |

**Legibilidade do conteúdo:**
| Teste | Critério de aprovação |
|---|---|
| Sumário/índice | Presente em posts longos (sugerir estrutura de H2s que sirva de índice) |
| Parágrafos curtos | Nenhum parágrafo com mais de 120 palavras (~150 no limite tolerável). Ao encontrar um mais longo, divida em dois no ponto de quebra natural (mudança de sub-ideia), não no meio da frase |
| Uso de mídia | **Mínimo 4 imagens por post** (1 de capa + 3 inline, distribuídas ao longo do texto — nunca agrupadas), todas com alt text contendo a keyword. Esse é o critério para pontuação máxima; não tratar como opcional |

Referência completa de critérios (se precisar consultar o detalhamento original): `references/seo-rankmath-checklist.md`

### 3. Rode a auditoria GEO (visibilidade em IA generativa)

Estes critérios não vêm do Rank Math — são específicos para aparecer como fonte citada em ChatGPT, Perplexity, Gemini e AI Overviews. Avalie cada um com o mesmo sistema 🔴🟡🟢:

| Critério GEO | O que checar |
|---|---|
| Resposta direta no topo | Os primeiros 150–200 palavras do post respondem a pergunta central de forma autocontida (LLMs citam preferencialmente o topo do conteúdo) |
| Frase de definição | Cada seção principal abre com uma frase-definição clara e extraível isoladamente (ex: "GEO é o processo de..." em vez de rodeios) |
| Estrutura extraível | Uso de listas, tabelas comparativas e blocos "Top N" — formatos que motores de IA conseguem extrair e citar diretamente |
| Perguntas reais como subheadings | Pelo menos alguns H2/H3 formulados como as pessoas de fato perguntam para uma IA (ex: "Como funciona X?" em vez de só "Funcionamento de X") |
| Prova de autoridade | 3–5 citações a fontes externas confiáveis, dados/estatísticas concretas — corroboração aumenta a chance de citação |
| Clareza de entidade | O post deixa claro, sem ambiguidade, quem é a marca/produto/serviço e o que ele faz (nomes próprios claros, não só pronomes) |
| Seção de FAQ | Perguntas frequentes ao final, formuladas como consultas reais — bom para extração por engines de IA |
| Sinal de atualização | Data de publicação/atualização visível — conteúdo sem sinal de frescor perde prioridade de citação |

Referência com mais contexto (dados de mercado, fontes): `references/geo-principles.md`

### 4. Entregue o resultado

Formato de entrega (sempre os dois, nesta ordem):

**a) Resumo da auditoria** — curto, direto, sem enrolação. Liste só os pontos 🔴 e 🟡 encontrados (não repita os 🟢, ninguém precisa disso). Algo como:

```
DIAGNÓSTICO RÁPIDO — SEO + GEO
Focus keyword: [x] | Palavras: [x]

🔴 Faltando keyword na meta description
🔴 Nenhuma resposta direta nos primeiros 200 palavras (GEO)
🟡 Só 1 link externo, sem link interno
🟡 2 parágrafos passam de 120 palavras
```

**b) Post reescrito e otimizado** — a versão completa e pronta para publicar, já incorporando todas as correções (keyword, meta description sugerida, subheadings ajustados, parágrafos quebrados, resposta direta no topo, FAQ se fizer sentido, sugestões de onde inserir imagem/link). Entregue como artifact em markdown se o post for longo (>20 linhas — quase sempre será).

Ao final do post reescrito, inclua um bloco pequeno com:
- **Meta title sugerido** (com keyword, até 60 caracteres)
- **Meta description sugerida** (com keyword, 120–160 caracteres)
- **Slug/URL sugerida**

### Tom e marca

Sempre respeitar o tom de voz de `VOICE.md` e o posicionamento de `BRAND.md` (se existirem), além dos termos proibidos do `CLAUDE.md`. Na ausência deles: direto, sem jargão técnico desnecessário e sem clichês genéricos de marketing ("soluções inovadoras", "excelência em tudo que fazemos").

### Aprendizados de campo (confirmados contra o Rank Math real)

- **Densidade de keyword só conta o corpo visível.** Editar `alt` de imagem, o comentário/frontmatter do arquivo ou meta tags não move o número que o Rank Math mostra. Se a densidade está baixa, a correção tem que entrar em frases de parágrafo, heading ou lista.
- **Focus keyword deve ser curta (2-4 palavras), não o título inteiro.** Ao inferir a keyword do texto, prefira o núcleo semântico do título (ex.: título "Como Usar Dados Para Tomar Decisões de Negócio" → keyword "decisões de negócio", não a frase toda).
- **4 imagens com alt text é o padrão mínimo esperado**, não um bônus — sempre planejar 1 capa + 3 inline distribuídas por post.
- **Nofollow em links externos costuma ser causado pelo WordPress/Rank Math, não pelo HTML.** Verificar o `rel` do HTML primeiro; se já está correto (`noopener noreferrer`, sem nofollow), não há nada a "consertar" no arquivo — apontar a configuração do plugin como causa provável.
- **Número no título deve refletir o conteúdo real** (contagem de itens já presentes no artigo), nunca um número decorativo sem lastro.

### O que NÃO fazer

- Não peça esboço/aprovação antes de entregar — essa skill já entrega o resultado final direto.
- Não repita en extenso os critérios 🟢 no resumo — foque no que precisa de ação.
- Não invente dados de tráfego, ranking ou volume de busca — se precisar desses números, sinalize que não tem e sugira onde o time pode obtê-los (Google Search Console, ferramenta de keyword research).
- Não sacrifique legibilidade humana por causa de densidade de keyword — se otimizar demais deixar o texto forçado, avise o usuário no resumo em vez de forçar a keyword.
