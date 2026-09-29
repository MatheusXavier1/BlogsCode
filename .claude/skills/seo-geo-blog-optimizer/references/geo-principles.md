# Princípios de GEO (Generative Engine Optimization)

GEO é a otimização de conteúdo para aparecer como fonte citada/mencionada em respostas geradas por IA
(ChatGPT, Google AI Overviews, Perplexity, Gemini, Copilot). Diferente do SEO tradicional, o objetivo
não é ranquear nos "links azuis", e sim ser a fonte que o modelo puxa via RAG (retrieval-augmented generation)
para montar a resposta.

## Por que importa

Uma fatia crescente das buscas termina sem clique no site (zero-click search) porque a IA já entrega a resposta
direto. O objetivo de GEO é continuar sendo citado/mencionado mesmo nesse cenário — a nova pergunta não é
"estamos ranqueando?" e sim "estamos sendo citados quando a IA responde?".

## Princípios práticos (usados nesta skill)

1. **Resposta direta no topo** — motores de IA citam preferencialmente o conteúdo dos primeiros ~30% do
   texto. Um bloco de "resposta rápida" nos primeiros 150–200 palavras aumenta muito a chance de citação.

2. **Frase de definição por seção** — cada H2/H3 deve abrir com uma frase autocontida e extraível
   isoladamente (ex: "GEO é o processo de otimizar conteúdo para citação em IA generativa."). Modelos
   preferem extrair definições concisas em vez de garimpar o parágrafo inteiro.

3. **Estrutura extraível** — listas, tabelas comparativas e formatos "Top N" são desproporcionalmente mais
   citados que texto corrido, porque são mais fáceis de extrair e reformatar.

4. **Perguntas reais como subheadings** — pessoas conversam com IA fazendo perguntas diretas. Content
   estruturado como pergunta-resposta tem mais chance de casar com o prompt do usuário.

5. **Prova de autoridade / corroboração** — citar 3–5 fontes externas confiáveis e incluir dados/estatísticas
   concretas aumenta a confiança do modelo na página como fonte. Corroboração multi-fonte (o mesmo dado
   aparecendo em vários domínios independentes) reforça ainda mais.

6. **Clareza de entidade** — a IA precisa entender sem ambiguidade quem é a marca/produto/serviço.
   Evitar excesso de pronomes vagos; usar o nome próprio da marca/produto quando relevante.

7. **FAQ ao final** — seção de perguntas frequentes formuladas como consultas reais (schema FAQPage,
   quando o CMS permitir) facilita a extração direta pela IA.

8. **Sinal de atualização/frescor** — conteúdo sem data de atualização visível perde prioridade de citação
   ao longo do tempo. Recomenda-se revisar e sinalizar atualizações periodicamente.

## O que fica de fora do escopo desta skill (por enquanto)

- Implementação técnica de schema markup (JSON-LD) — a skill pode *recomendar* que o time técnico implemente,
  mas não gera o código de schema.
- Monitoramento de citações em plataformas de IA (isso é um processo contínuo de tracking, não uma tarefa
  de otimização de texto pontual).
- Distribuição/PR para acelerar indexação por engines de IA — fora do escopo de "otimizar um post".

## Fontes consultadas

- seoTuners — Best Practices for Generative Engine Optimization (GEO) 2026
- GenOptima — Generative Engine Optimization Best Practices: The Complete 2026 Playbook
- ShopOS — Generative Engine Optimization Best Practices for 2026
- Pesquisa original de GEO: Aggarwal et al., 2024, arXiv:2311.09735 (Princeton)

Nota: dados de percentuais específicos (ex: "74.2% das citações vêm de listicles") vêm de relatórios de
agências de marketing e não são estudos acadêmicos peer-reviewed — trate como direção geral de mercado,
não como número absoluto garantido.
