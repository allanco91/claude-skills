---
name: preferencia-de-codigo
description: Aplica as convenções de estilo pessoais do usuário para código TypeScript, React e Node.js — tanto ao gerar código novo quanto ao revisar/editar código existente. Use sempre que o usuário pedir para escrever, criar, refatorar, ajustar ou revisar código em TS, React ou Node, mesmo que ele não mencione "estilo" ou "convenção" explicitamente. Cobre tipagem, estrutura de componentes, hooks, controle de fluxo e nomenclatura.
---

# Preferência de Código (TS + React + Node)

Skill para aplicar as convenções pessoais do usuário sempre que ele escrever ou revisar código em TypeScript, React ou Node.js.

## Quando usar

- Ao gerar código novo em TS/React/Node para o usuário
- Ao revisar, refatorar ou editar código existente nessas stacks
- Vale tanto para componentes React quanto para lógica de backend em Node

## Convenções

### Tipagem
- Preferir `type` em vez de `interface`
- Todo `type` deve começar com a letra `T` (ex: `TUserProps`, `TApiResponse`)
- Types devem ficar em um arquivo específico dedicado a eles (ex: `types.ts`), não misturados com a lógica
- NUNCA usar `any`. Se não houver como tipar corretamente, usar `unknown` (e tratar/validar o tipo antes de usar o valor)

### Loops
- Preferir `for...of` em vez de outros tipos de loop (`for` tradicional, `forEach`, etc.)

### Componentes React
- Criar componentes com `function`, nunca com `const` + arrow function
- Não desestruturar as props no parâmetro do componente — exceção: quando a prop tem valor default, aí desestruturar faz sentido
- Separar toda lógica de estado em hooks customizados, mantendo o componente com um render limpo (componente só cuida de apresentação)
- Nunca usar inline style — sob nenhuma circunstância

### Funções
- Preferir declarar funções com `function` em vez de `const arrow function`

### Controle de fluxo
- Não usar if ternário, exceto quando for muito simples e não prejudicar a leitura
- Não usar ifs aninhados
- Preferir `return` dentro das condições em vez de `else` (early return / guard clauses)
- Em funções `void` (que não retornam valor, ex: handlers, setters), não escrever `return algumaChamadaVoid()` — isso "retorna void" desnecessariamente. Em vez disso, chamar a função e usar um `return;` vazio para sair:
  ```ts
  // Errado
  function toggleOpen() {
    if (isOpen) return setIsOpen(false);
    return setIsOpen(true);
  }

  // Certo
  function toggleOpen() {
    if (isOpen) {
      setIsOpen(false);
      return;
    }
    setIsOpen(true);
  }
  ```

### Nomenclatura
- Nomes de variáveis sempre claros e descritivos
- Nomes de predicados em callbacks (`.map`, `.filter`, etc.) também devem ser claros — ex: `users.map(user => user.name)`, não `users.map(u => u.name)`

## Exemplo de aplicação

**Antes (fora do padrão):**
```tsx
const UserCard = ({ name, age, theme = "light" }) => {
  const [isOpen, setIsOpen] = useState(false);

  const handleClick = () => {
    if (isOpen) {
      setIsOpen(false);
    } else {
      setIsOpen(true);
    }
  };

  return (
    <div style={{ padding: "8px" }} onClick={handleClick}>
      {name} - {isOpen ? "aberto" : "fechado"}
    </div>
  );
};
```

**Depois (seguindo a preferência do usuário):**
```tsx
// types.ts
export type TUserCardProps = {
  name: string;
  age: number;
  theme?: "light" | "dark";
};

// useUserCardState.ts
function useUserCardState() {
  const [isOpen, setIsOpen] = useState(false);

  function toggleOpen() {
    if (isOpen) {
      setIsOpen(false);
      return;
    }
    setIsOpen(true);
  }

  return { isOpen, toggleOpen };
}

// UserCard.tsx
function UserCard(props: TUserCardProps) {
  const { theme = "light" } = props;
  const { isOpen, toggleOpen } = useUserCardState();

  return (
    <div className="user-card" onClick={toggleOpen}>
      {props.name} - {isOpen ? "aberto" : "fechado"}
    </div>
  );
}
```

Note: as props só foram desestruturadas para `theme`, que tem valor default. `name` e `isOpen`/`toggleOpen` (que vêm do hook) permanecem acessados via `props.` ou retorno nomeado do hook.

## Ao revisar código existente

Ao encontrar código que viola essas convenções, apontar a violação específica e sugerir a correção seguindo o padrão acima — não é necessário reescrever o arquivo inteiro a menos que o usuário peça.
