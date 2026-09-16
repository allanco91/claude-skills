---
name: documentacao-tecnica
description: Gera documentação técnica em markdown, com foco em runbooks (procedimentos operacionais) — passo a passo enxuto para executar, resolver ou operar algo. Use sempre que o usuário pedir para documentar um procedimento operacional, processo de deploy, rotina de manutenção, plano de resposta a incidente, ou qualquer "como fazer X" técnico que outra pessoa (ou ele mesmo no futuro) precise seguir. Produz documentação direta ao ponto, sem enrolação, priorizando passos executáveis.
---

# Documentação Técnica (Runbooks)

Skill para documentar procedimentos operacionais técnicos de forma enxuta e direta — o tipo de documento que alguém segue passo a passo sob pressão (ex: durante um incidente) ou pra executar uma rotina sem precisar adivinhar nada.

## Quando usar

- Usuário pede pra documentar um procedimento, processo ou rotina técnica
- Usuário descreve os passos de um deploy, rollback, manutenção, ou resposta a incidente
- Usuário pede um "runbook" ou "como fazer X" que precisa ficar registrado

## Princípios

- **Enxuto e direto ao ponto** — sem parágrafos longos de contexto, sem redundância. Se uma frase não ajuda quem está executando, corta.
- Passos numerados e executáveis — cada passo é uma ação concreta, não uma descrição vaga
- Comandos, se houver, em blocos de código prontos para copiar/colar
- Assumir que quem lê pode estar sob pressão (ex: incidente em produção) — sem rodeios

## Template

```markdown
# [Nome do procedimento]

**Objetivo:** [1 frase — o que esse runbook resolve/executa]
**Quando usar:** [gatilho — ex: "quando o deploy falhar no passo X", "toda sexta às 18h"]

## Pré-requisitos
- [acesso/ferramenta/permissão necessária]
- [...]

## Passos

1. [Ação concreta]
   ```bash
   comando-se-houver
   ```
2. [Ação concreta]
3. [Ação concreta]

## Verificação
[Como confirmar que deu certo — 1-2 linhas ou um comando de check]

## Se der errado
- **[sintoma/erro comum]:** [o que fazer]
- **[outro sintoma]:** [o que fazer]

## Rollback
[Passos para reverter, se aplicável — ou "N/A" se não houver rollback]
```

### Regras de formatação

- Nunca usar mais de 1-2 frases de introdução antes de ir para pré-requisitos/passos
- Cada passo começa com um verbo de ação (ex: "Rodar", "Verificar", "Reiniciar")
- Se um passo tiver mais de uma ação, quebrar em sub-passos em vez de um parágrafo
- Omitir seções do template que não se aplicam (ex: "Rollback" só entra se fizer sentido) em vez de preencher com "N/A" sempre

## Perguntas a fazer quando faltar contexto

- Quais são os pré-requisitos (acesso, ferramentas) pra executar isso?
- Existe um jeito de verificar se deu certo?
- O que fazer se algo der errado no meio do caminho?
