---
name: documentacao-funcional
description: Gera documentação funcional em markdown — cobrindo tanto specs/requisitos de algo a ser implementado quanto fluxos de negócio/regras de um sistema já existente. Use sempre que o usuário pedir para "documentar" uma funcionalidade, regra de negócio, fluxo, requisito, ou pedir uma "spec", especialmente quando ele descrever o que um sistema faz ou deve fazer em linguagem de negócio (não código). Produz em formato de user stories com critérios de aceite, escrito em linguagem acessível tanto para público técnico quanto não-técnico.
---

# Documentação Funcional

Skill para documentar funcionalidades — seja especificando algo que ainda vai ser construído, seja registrando como um fluxo/regra de negócio já existente funciona.

## Quando usar

- Usuário pede pra documentar uma funcionalidade, fluxo ou regra de negócio
- Usuário pede uma "spec" ou requisitos antes de implementar algo
- Usuário descreve o comportamento de um sistema existente e quer isso registrado
- Usuário pede user stories ou critérios de aceite

## Formato de saída

Sempre em **markdown estruturado**, usando o formato de **user story + critérios de aceite**. Linguagem acessível — evitar jargão técnico desnecessário, já que o documento serve tanto para público técnico (dev/PM) quanto não-técnico (negócio/cliente). Quando um termo técnico for indispensável, explicar brevemente entre parênteses na primeira vez que aparecer.

### Template

```markdown
# [Nome da funcionalidade/fluxo]

## Contexto
[2-4 frases explicando o "porquê" — que problema isso resolve ou que parte do negócio isso cobre]

## User Story
Como [persona/tipo de usuário]
Quero [ação/funcionalidade]
Para [benefício/objetivo de negócio]

## Regras de negócio
- [Regra 1 — em linguagem clara, sem ambiguidade]
- [Regra 2]
- [...]

## Critérios de aceite
### Cenário: [nome do cenário]
- **Dado** [contexto/pré-condição]
- **Quando** [ação realizada]
- **Então** [resultado esperado]

### Cenário: [outro cenário, incluindo casos de borda/exceção]
- **Dado** ...
- **Quando** ...
- **Então** ...

## Fora de escopo
[O que essa funcionalidade explicitamente NÃO cobre, se relevante — evita ambiguidade]
```

### Ajustes conforme o caso

- **Documentando algo a ser construído (spec/requisito):** incluir todas as seções acima, com foco em deixar os critérios de aceite completos o bastante para servir de base pra implementação e teste.
- **Documentando um fluxo/sistema já existente:** manter a mesma estrutura, mas a seção "Regras de negócio" passa a ser o núcleo — é onde comportamentos, exceções e casos de borda observados no sistema real são registrados. A "User Story" nesse caso descreve o fluxo como ele funciona hoje, não uma proposta.
- Sempre incluir pelo menos um cenário de critério de aceite para o "caminho feliz" (fluxo principal sem erros) e, quando fizer sentido, cenários para casos de borda/erro.

## Perguntas a fazer quando faltar contexto

Antes de escrever, se o usuário não tiver dado informação suficiente, perguntar (no máximo o essencial, sem travar o processo):
- Quem são os usuários/personas envolvidos?
- Existe alguma regra de negócio ou exceção que não pode faltar?
- É uma proposta nova ou a documentação de algo que já existe?
