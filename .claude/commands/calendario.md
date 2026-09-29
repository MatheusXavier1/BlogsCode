---
description: Gera um calendário editorial de blog usando a skill claude-blog
argument-hint: <nicho/tema> [mensal|trimestral]
---

Rode `/blog calendar` (mensal por padrão, ou o período indicado em $ARGUMENTS) para o nicho/tema informado em $ARGUMENTS. Se o usuário ainda não tiver rodado `/blog strategy` para esse nicho, sugira rodar antes para embasar os temas do calendário.

Salve o calendário gerado em `calendario-editorial.md` na raiz do projeto.

Ao final, liste os títulos/temas planejados e informe que cada um pode ser produzido rodando `/post "<título>"`.
