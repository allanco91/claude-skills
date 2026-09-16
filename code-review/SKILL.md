---
name: code-review
description: Revisa código — seja um diff/PR colado ou anexado, seja um arquivo/trecho que o usuário pede para avaliar. Use sempre que o usuário pedir "revisão de código", "code review", colar um diff, anexar um PR, ou pedir para avaliar/analisar um trecho de código já escrito. Cobre boas práticas gerais (legibilidade, bugs, segurança, performance) combinadas com o checklist pessoal do usuário abaixo. Produz um resumo estruturado por categoria, não comentários inline soltos.
---

# Code Review

Skill para revisar código de forma consistente, combinando boas práticas gerais de engenharia com os critérios pessoais do usuário.

## Quando usar

- Usuário cola ou anexa um diff/PR e pede revisão
- Usuário pede para revisar/avaliar um arquivo ou trecho de código específico
- Usuário menciona "code review", "revisão de código", ou pede feedback sobre código que escreveu

## Checklist pessoal do usuário

> Preencher/ajustar conforme o usuário for refinando a skill. Por enquanto, critérios de exemplo — substitua pelos seus:

- [ ex: nomenclatura de variáveis e funções deve ser descritiva, sem abreviações obscuras ]
- [ ex: funções não devem passar de ~30-40 linhas; se passar, sinalizar oportunidade de quebrar ]
- [ ex: preferir early return a aninhamento profundo de if/else ]
- [ ex: nenhum código comentado ("morto") deve ficar no PR final ]

## Categorias de boas práticas gerais

Ao revisar, sempre considerar (quando aplicável ao código em questão):

1. **Bugs / corretude** — lógica quebrada, edge cases não tratados, off-by-one, condições de corrida
2. **Segurança** — injeção, dados sensíveis expostos, validação de entrada ausente
3. **Legibilidade / manutenibilidade** — nomes, estrutura, duplicação, complexidade desnecessária
4. **Performance** — loops ineficientes, queries N+1, alocações desnecessárias
5. **Testes** — cobertura de casos relevantes, testes ausentes para lógica nova/alterada
6. **Estilo pessoal** — aderência ao checklist pessoal acima

## Formato de saída

Sempre um **resumo estruturado por categoria**, não comentários inline soltos. Estrutura:

```markdown
## Resumo da revisão

### 🐛 Bugs / Corretude
- [item] — [linha/trecho de referência] — [por quê é um problema]

### 🔒 Segurança
- ...

### 📖 Legibilidade / Manutenibilidade
- ...

### ⚡ Performance
- ...

### ✅ Testes
- ...

### 🎨 Estilo pessoal
- ...

### Resumo geral
[2-3 frases: o código está pronto para merge? quais são os pontos mais críticos a resolver antes?]
```

Se uma categoria não tiver nenhum ponto relevante, omitir a categoria em vez de escrever "nada a apontar" — só as categorias com conteúdo real aparecem no resumo.

Sempre indicar a severidade de cada ponto (crítico / importante / sugestão) quando não for óbvio pelo contexto.
