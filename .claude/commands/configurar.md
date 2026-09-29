---
description: Configura o projeto para a sua marca (CLAUDE.md, BRAND.md e VOICE.md) por perguntas
argument-hint: (sem argumentos)
---

Você vai personalizar este projeto para a marca do usuário. Os arquivos `CLAUDE.md`, `BRAND.md`, `VOICE.md` e `PUBLICACOES.md` vêm com campos `{{...}}` a preencher.

1. Rode `grep -n "{{" CLAUDE.md BRAND.md VOICE.md PUBLICACOES.md` para ver o que falta. Se nada faltar, avise que o projeto já está configurado e pergunte o que o usuário quer ajustar.
2. Pergunte ao usuário, em blocos curtos (no máximo 4 perguntas por vez), na ordem abaixo. Ofereça sugestões quando puder, mas **nunca invente** dados da empresa: se o usuário não souber, deixe o campo como "a definir" e avise.
   1. **Marca:** nome exato como deve aparecer nos textos, descritor obrigatório (se houver), domínio, caminho do blog (ex.: `/blog`), página de contato, idioma dos textos.
   2. **Público e posicionamento:** quem lê, que problema tem, o que a empresa faz de diferente, o que ela não é, concorrentes (opcional).
   3. **Regras editoriais:** o que sempre fazer, o que nunca escrever, termos proibidos (siglas internas, promessas indevidas), avisos obrigatórios.
   4. **Categorias e escopo:** categorias do blog (devem existir no WordPress) e temas dentro, parcialmente dentro e fora do escopo.
   5. **Tom de voz:** pessoa (nós/você), formalidade, contrações, exemplos de frases no tom desejado.
   6. **WordPress:** o `blog_id`. Se o usuário não souber, peça para ele pedir "liste meus sites do WordPress.com" e use o resultado (a ferramenta `wpcom-user-sites`, que pode estar deferida: carregue-a via ToolSearch).
3. Escreva as respostas nos arquivos, substituindo cada `{{...}}` e apagando as instruções em itálico dos modelos. Mantenha a estrutura dos arquivos. Ajuste a seção "Regras de escrita" do `CLAUDE.md` ao que o usuário definiu.
4. Rode o grep do passo 1 de novo. Liste o que ficou pendente.
5. Termine com um resumo curto: o que foi preenchido, o que ficou "a definir", e o próximo passo (`/calendario "<nicho>"` ou `/post "<tópico>"`). Lembre que a chave do Pexels ainda precisa estar em `.claude/config/pexels_api_key.txt` e que o conector do WordPress.com precisa estar ligado (veja o README).

Não invente nada sobre a empresa. Não escreva credenciais em nenhum arquivo.
