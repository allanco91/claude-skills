---
name: planejamento-de-tarefa
description: "Quebra uma tarefa de desenvolvimento em um plano técnico estruturado antes de começar a codar — contexto, regras, testes, alterações de layout e riscos. Use sempre que o usuário pedir para planejar, quebrar em passos, ou organizar uma tarefa/feature/bug antes de implementar, mesmo que ele não peça explicitamente um 'plano' (ex: 'vou começar a mexer em X, me ajuda a organizar' ou 'antes de codar, quero entender os passos')."
---

# Planejamento de Tarefa

Skill para estruturar o plano técnico de uma tarefa antes de começar a implementação — evita começar a codar sem ter mapeado regras, riscos e o que precisa ser testado.

## Quando usar

- Usuário pede pra planejar, organizar ou quebrar uma tarefa/feature/bug em passos
- Usuário descreve algo que vai implementar e pede ajuda para estruturar antes de começar
- Usuário está prestes a começar uma tarefa nova e quer clareza antes de codar

## Formato de saída

```markdown
# Plano: [nome da tarefa]

## Contexto
[O que é a tarefa, por que está sendo feita, e onde ela se encaixa (bug, feature, refactor)]

## Regras
- [Regra de negócio ou comportamento que a implementação precisa respeitar]
- [...]

## Passos técnicos
1. [Passo concreto de implementação]
2. [Passo concreto de implementação]
3. [...]

## Testes
- [O que precisa ser testado — cenário principal]
- [Casos de borda relevantes]

## Alterações no layout
[Se houver mudança visual/UI, descrever o que muda. Omitir essa seção inteira se a tarefa não envolve layout/UI]

[Incluir também uma proposta visual simples (wireframe/mockup) da mudança, além da descrição em texto]

## Riscos
- [O que pode dar errado, quebrar, ou gerar efeito colateral em outras partes do sistema]
- [Dependências externas ou de outras partes do código que podem impactar a tarefa]
```

## Diretrizes

- **Contexto** deve ser curto (2-4 frases) — só o suficiente pra situar por que a tarefa existe
- **Regras** são o comportamento esperado/negócio — não confundir com passos técnicos
- **Passos técnicos** devem ser concretos e ordenados, algo que dá pra seguir sequencialmente ao codar
- **Testes** cobrem tanto o caminho principal quanto casos de borda que a implementação precisa considerar
- **Alterações no layout** só aparece quando a tarefa envolve UI/UX — se for uma tarefa puramente de backend/lógica, omitir essa seção. Quando aparecer, sempre acompanhar a descrição textual de uma proposta visual da mudança: usar a ferramenta de visualização disponível para gerar um mockup/wireframe; se ela não estiver disponível, representar com um wireframe em ASCII/texto estruturado
- **Riscos** deve ser honesto sobre o que pode quebrar — não é uma lista genérica, é específica pra essa tarefa

## Perguntas a fazer quando faltar contexto

Antes de montar o plano, se faltar informação, perguntar o essencial:
- Isso envolve mudança de UI/layout ou é só lógica/backend?
- Existe alguma regra de negócio específica que não pode ser esquecida?
- Essa tarefa mexe em algo que outras partes do sistema dependem?
