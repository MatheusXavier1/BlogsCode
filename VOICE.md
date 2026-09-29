# Voice Context

> Modelo. Ajuste cada item ao tom da sua marca ou rode `/iniciar` (ou `/configurar`). Este arquivo é lido automaticamente pelas sub-skills do `claude-blog` e pelos comandos do projeto. Os valores abaixo são um padrão razoável para conteúdo B2B.

## Pronoun stance
{{Primeira pessoa ("nossa equipe"), segunda pessoa ("você"), terceira pessoa, ou misto}}

## Lexical rules
- **Contractions**: {{ex.: nenhuma (português formal) | permitidas}}
- **Sentence ceiling**: frases curtas e diretas, com a resposta no início do parágrafo. Evitar frases acima de ~25-30 palavras.
- **Paragraph ceiling**: 150 palavras
- **Summary label**: {{ex.: "Principais Pontos"}}

## Headline patterns
- **Favor**: títulos diretos, orientados a resposta, com a palavra-chave principal perto do início (afirmação, pergunta que reflete busca real, ou formato numerado quando fizer sentido).
- **Avoid**: títulos alarmistas, clickbait, exagero sem sustentação no conteúdo.

## Voice fingerprint
- Tom geral: {{ex.: profissional, direto, consultivo, sem informalidade excessiva}}
- Para controle programático do tom, rode `/blog persona create`.

## Readability target
- Audience tier: {{consumidor | profissional | misto}}
- Flesch Grade / Ease: não definido; seguir as recomendações padrão de SEO/GEO do `claude-blog`.

## Reference samples
- {{cole aqui 1 ou 2 parágrafos que representam o tom ideal, ou "nenhuma amostra ainda"}}
