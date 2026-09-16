---
name: resumo-de-tarefa
description: "Gera um resumo do que foi corrigido/implementado em uma tarefa, incluindo qual projeto foi usado, pronto para colar em um ticket ou PR (título + descrição). Use sempre que o usuário pedir para resumir uma tarefa, gerar descrição de PR, fechar um ticket, ou perguntar 'o que eu fiz nessa tarefa'. Tira as informações do diff/código alterado quando disponível, e complementa com o contexto da conversa quando o diff não cobrir tudo."
---

# Resumo de Tarefa

Skill para gerar um resumo do que foi corrigido ou implementado numa tarefa, pronto para colar em um ticket ou PR.

## Quando usar

- Usuário pede pra resumir o que foi feito numa tarefa
- Usuário pede uma descrição de PR/ticket
- Usuário termina de implementar/corrigir algo e quer documentar o que mudou
- Usuário pergunta "o que eu fiz até agora nessa tarefa"

## De onde tirar a informação

- Se houver diff/código alterado disponível (arquivos modificados na conversa, git diff, etc.), usar isso como fonte principal — é mais preciso que memória de conversa
- Complementar com o contexto da conversa (o que o usuário disse que estava tentando resolver, decisões tomadas, motivação) quando o diff sozinho não explica o "porquê"
- Se só houver contexto de conversa (sem diff/código visível), montar o resumo a partir disso, deixando claro que é baseado na conversa e não no código em si

## Formato de saída

Sempre gerar **duas versões**: a completa e uma resumida.

### Versão completa

Pronta para colar em ticket/PR: título curto + descrição estruturada.

```markdown
**Título:** [resumo em uma linha, no imperativo — ex: "Corrige cálculo de reembolso parcial no cancelamento"]

**Projeto:** [nome do projeto/repositório usado]

**O que foi feito:**
- [item 1 — mudança concreta]
- [item 2]
- [...]

**Motivo:**
[1-2 frases explicando por que essa mudança foi necessária — o bug, a necessidade, o pedido original]

**Como testar/validar:**
- [passo ou cenário pra confirmar que funciona]
```

### Versão resumida

Só o essencial — título + projeto + 2-3 linhas no máximo, sem "Motivo" nem "Como testar/validar". Serve pra colar em updates rápidos (Slack, standup, etc.).

```markdown
**Título:** [mesmo título da versão completa]
**Projeto:** [mesmo projeto]

[1-3 frases resumindo o que foi feito, sem detalhar item por item]
```

### Regras

- Título sempre no imperativo (ex: "Corrige", "Adiciona", "Remove"), nunca no gerúndio ou passado
- "O que foi feito" lista mudanças concretas, não intenções — se algo foi tentado mas não terminou, deixar claro que está incompleto
- "Projeto" deve identificar claramente qual repositório/projeto foi alterado — se a tarefa envolveu mais de um projeto, listar todos
- Omitir "Como testar/validar" se não houver informação suficiente pra preencher com algo real, em vez de inventar um passo genérico
- Ser específico: em vez de "corrigido bug no cadastro", preferir "corrigido cálculo de raio que ignorava áreas desativadas no filtro do painel"

## Perguntas a fazer quando faltar contexto

- Qual foi o projeto/repositório usado nessa tarefa?
- Isso já está pronto pra revisão, ou ainda falta algo?
