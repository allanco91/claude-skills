---
name: qualidade-de-codigo
description: "SEMPRE aplique esta skill ao terminar de escrever, gerar ou editar qualquer código de tamanho não trivial — analisa qualidade estrutural do código: princípios SOLID, complexidade ciclomática, acoplamento, duplicação/reaproveitamento, cobertura e qualidade de testes, e performance/escalabilidade. É complementar às skills de code-review (revisão de bugs/segurança/legibilidade) e preferencia-de-codigo (convenções de estilo) — esta skill foca em qualidade estrutural e de design, não em estilo ou bugs pontuais. Roda automaticamente, sem precisar ser pedida, sempre que uma quantidade relevante de código for produzida ou alterada."
---

# Qualidade de Código

Skill para avaliar a qualidade estrutural do código produzido ou alterado — complementar ao `code-review` (bugs/segurança/legibilidade) e à `preferencia-de-codigo` (convenções de estilo). Esta skill olha para o design do código: princípios, complexidade, duplicação, testes e performance.

## Quando usar

- Automaticamente, ao terminar de escrever, gerar ou editar uma quantidade relevante de código (uma função isolada e trivial não precisa passar por essa análise completa — funções/módulos com lógica não trivial, sim)
- Quando o usuário pedir explicitamente uma análise de qualidade de código

## Como executar: delegar quando possível

Se houver um subagent `quality-reviewer` disponível (ambiente Claude Code, com suporte a subagents), delegar a análise a ele em vez de fazer inline — o `quality-reviewer` nunca viu a implementação sendo escrita, o que dá uma segunda opinião sem o viés de quem acabou de codar. Passar a ele os arquivos/trechos relevantes e aguardar o relatório.

Se não houver subagents disponíveis no ambiente (ex: Claude.ai, Cowork), aplicar a análise abaixo diretamente, inline, como fallback.

## Categorias analisadas

### 1. Princípios (SOLID / complexidade / acoplamento)
- **Responsabilidade única**: a função/classe/módulo faz uma coisa só? Se uma função tem múltiplas responsabilidades misturadas, sinalizar
- **Complexidade ciclomática**: contar caminhos de execução (ifs, loops, switches, operadores lógicos combinados). Funções com muitos caminhos (aproximadamente mais de 8-10) são candidatas a quebra em funções menores
- **Acoplamento**: o código depende de detalhes internos de outros módulos, em vez de depender de interfaces/contratos? Módulos muito amarrados uns aos outros dificultam mudança isolada
- **Profundidade de aninhamento**: mesmo com early return (já coberto pela skill de preferência de código), verificar se a lógica em si não ficou excessivamente ramificada

### 2. Duplicação e reaproveitamento
- Trechos de lógica repetidos (mesmo que com pequenas variações) que poderiam virar uma função/hook/utilitário compartilhado
- Padrões copiados e colados entre componentes/módulos que indicam abstração faltando
- Cuidado para não sugerir abstração prematura: duplicação de 2 ocorrências simples nem sempre justifica extração — julgar pelo contexto

### 3. Testes
- O código novo/alterado tem testes cobrindo o caminho principal?
- Casos de borda relevantes (valores nulos/vazios, limites, erros) estão cobertos?
- Os testes existentes testam comportamento (o quê) e não implementação (como) — testes muito acoplados a detalhes internos quebram fácil em refatorações
- Se não houver testes e o código for não trivial, sinalizar a ausência como ponto de atenção

### 4. Performance e escalabilidade
- Operações custosas dentro de loops (ex: chamada a API, query a banco, cálculo pesado repetido sem necessidade)
- Estruturas de dados inadequadas para o volume esperado (ex: busca linear onde um `Map`/`Set` resolveria em O(1))
- Em React especificamente: re-renders desnecessários, cálculos pesados sem memoização (`useMemo`/`useCallback`) quando justificado, listas grandes sem virtualização
- Não otimizar prematuramente: sinalizar apenas gargalos reais ou prováveis dado o contexto (volume de dados, frequência de chamada), não qualquer possível micro-otimização

## Formato de saída

Sempre duas partes: uma nota/classificação geral no topo, seguida do detalhamento por categoria.

```markdown
## Qualidade de código: [Boa / Atenção / Crítica]

[1-2 frases resumindo o veredito geral]

### Princípios (SOLID / complexidade / acoplamento)
- [achado] — [por quê importa] — [sugestão]

### Duplicação e reaproveitamento
- [achado] — [sugestão]

### Testes
- [achado] — [o que falta cobrir]

### Performance e escalabilidade
- [achado] — [impacto esperado] — [sugestão]
```

- A classificação geral (`Boa / Atenção / Crítica`) reflete a severidade combinada dos achados: **Crítica** se algo compromete corretude/performance de forma significativa; **Atenção** se há melhorias estruturais relevantes mas nada urgente; **Boa** se o código está sólido, com no máximo pontos menores
- Omitir uma categoria inteira se não houver achado relevante nela — não preencher com "nada a apontar"
- Se nenhuma categoria tiver achados, retornar só a nota geral "Boa" com uma frase curta, sem a estrutura completa

## Após a análise: perguntar quais melhorias aplicar

Se a classificação for **Atenção** ou **Crítica** (há achados de melhoria), não aplicar nada automaticamente e não perguntar de forma genérica ("quer que eu aplique?"). Em vez disso, listar os achados de forma numerada/identificável e perguntar quais o usuário quer que sejam aplicados — já que mudanças estruturais (extrair função, paralelizar chamadas, adicionar testes) têm custo e risco diferentes entre si, e o usuário pode querer aplicar só parte delas. Exemplo de como encerrar:

> Quais dessas melhorias você quer que eu aplique? (pode escolher mais de uma, ou "todas")
> 1. Extrair a lógica de busca de dados do pedido para uma função separada
> 2. Paralelizar as chamadas de API com `Promise.all`
> 3. Adicionar testes para os casos citados

Aplicar apenas os itens que o usuário selecionar, seguindo também as convenções da skill `preferencia-de-codigo`. Se a classificação for **Boa**, não é necessário perguntar nada — só informar o veredito.

## Diferença em relação às outras skills

- **`code-review`**: revisa bugs, segurança, legibilidade e testes de forma mais ampla, geralmente sob pedido explícito de revisão de um diff/PR
- **`preferencia-de-codigo`**: aplica convenções de estilo pessoais (nomenclatura, tipagem, estrutura sintática) — é sobre "como o código é escrito"
- **`qualidade-de-codigo`** (esta skill): é sobre "como o código é desenhado" — estrutura, responsabilidades, testes e performance. Roda automaticamente após a preferência de código já ter sido aplicada, como uma segunda camada de análise
