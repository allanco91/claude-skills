---
name: preferencia-de-codigo
description: SEMPRE aplique esta skill ao escrever, gerar, terminar de escrever, revisar, refatorar ou editar qualquer código em TypeScript, React ou Node.js — não é opcional, é o padrão obrigatório para qualquer código nessas stacks, mesmo que o usuário não mencione "estilo", "convenção" ou peça explicitamente para seguir um padrão. Aplique durante a escrita E faça uma checagem final antes de entregar o código, confirmando que todas as convenções foram seguidas. Cobre tipagem, estrutura de componentes, hooks, controle de fluxo, nomenclatura, constantes e assincronismo.
---

# Preferência de Código (TS + React + Node)

Skill para aplicar as convenções pessoais do usuário sempre que ele escrever ou revisar código em TypeScript, React ou Node.js.

## Quando usar

- Ao gerar código novo em TS/React/Node para o usuário
- Ao revisar, refatorar ou editar código existente nessas stacks
- Vale tanto para componentes React quanto para lógica de backend em Node
- Aplicar automaticamente, sem esperar o usuário pedir — este é o padrão de código dele, não uma opção pontual

## Checagem final obrigatória

Antes de considerar qualquer trecho de código pronto para entrega (seja escrito do zero, seja uma edição), revisar o código contra todas as convenções desta skill, uma a uma. Se algo não seguir o padrão, corrigir antes de entregar — não apontar a violação e deixar como está, a menos que o usuário tenha pedido especificamente para não aplicar a skill.

## Convenções

### Tipagem
- Preferir `type` em vez de `interface`
- Todo `type` deve começar com a letra `T` (ex: `TUserProps`, `TApiResponse`)
- Types devem ficar em um arquivo específico dedicado a eles (ex: `types.ts`), não misturados com a lógica
- NUNCA usar `any`. Se não houver como tipar corretamente, usar `unknown` (e tratar/validar o tipo antes de usar o valor)

### Loops
- Preferir `for...of` em vez de outros tipos de loop (`for` tradicional, `forEach`, etc.)

### Arrays
- Ao retornar/acessar um item específico de um array, dar preferência a `.at(n)` em vez de indexação com colchetes (`array[n]`) — ex: `items.at(0)` em vez de `items[0]`. `.at()` também aceita índices negativos (ex: `items.at(-1)` para o último item)

### Componentes React
- Criar componentes com `function`, nunca com `const` + arrow function
- Não desestruturar as props no parâmetro do componente — exceção: quando a prop tem valor default, aí desestruturar faz sentido
- Separar toda lógica de estado em hooks customizados, mantendo o componente com um render limpo (componente só cuida de apresentação)
- Nunca usar inline style — sob nenhuma circunstância
- Quando o estado é um objeto mais complexo (múltiplos campos relacionados que mudam juntos, ou transições de estado com lógica própria), dar preferência a `useReducer` em vez de múltiplos `useState` separados. Para estado simples (um valor isolado, booleano, string, número), `useState` continua sendo a escolha certa — a regra vale especificamente quando os campos formam um objeto coeso e há lógica de transição entre eles. Exemplo:

  Errado (múltiplos estados relacionados):
  ```ts
  function useCheckoutForm() {
    const [step, setStep] = useState(1);
    const [isSubmitting, setIsSubmitting] = useState(false);
    const [error, setError] = useState<string | null>(null);

    function goToNextStep() {
      setStep(step + 1);
      setError(null);
    }

    return { step, isSubmitting, error, goToNextStep };
  }
  ```

  Certo (estado complexo consolidado em useReducer):
  ```ts
  type TCheckoutState = {
    step: number;
    isSubmitting: boolean;
    error: string | null;
  };

  type TCheckoutAction =
    | { type: "next_step" }
    | { type: "submit_start" }
    | { type: "submit_error"; error: string };

  function checkoutReducer(state: TCheckoutState, action: TCheckoutAction): TCheckoutState {
    switch (action.type) {
      case "next_step":
        return { ...state, step: state.step + 1, error: null };
      case "submit_start":
        return { ...state, isSubmitting: true };
      case "submit_error":
        return { ...state, isSubmitting: false, error: action.error };
      default:
        throw new Error(`Unsupported action: ${JSON.stringify(action)}`);
    }
  }

  function useCheckoutForm() {
    const [state, dispatch] = useReducer(checkoutReducer, {
      step: 1,
      isSubmitting: false,
      error: null,
    });

    function goToNextStep() {
      dispatch({ type: "next_step" });
    }

    return { ...state, goToNextStep };
  }
  ```
- No `default` do `switch` dentro do reducer, nunca retornar o `state` silenciosamente — lançar um erro indicando que a action não é suportada (como no exemplo acima). Isso torna bugs visíveis em vez de mascará-los com um estado que não muda sem explicação. A mensagem do erro deve ser em inglês (ex: `Unsupported action: ...`) — mensagens de erro no código, de forma geral, ficam em inglês

### Funções
- Preferir declarar funções com `function` em vez de `const arrow function`

### Controle de fluxo
- Não usar if ternário, exceto quando for muito simples e não prejudicar a leitura
- Não usar ifs aninhados
- Preferir `return` dentro das condições em vez de `else` (early return / guard clauses)
- Em funções `void` (que não retornam valor, ex: handlers, setters), não escrever `return algumaChamadaVoid()` — isso "retorna void" desnecessariamente. Em vez disso, chamar a função e usar um `return;` vazio para sair:

  Errado:
  ```ts
  function toggleOpen() {
    if (isOpen) return setIsOpen(false);
    return setIsOpen(true);
  }
  ```

  Certo:
  ```ts
  function toggleOpen() {
    if (isOpen) {
      setIsOpen(false);
      return;
    }
    setIsOpen(true);
  }
  ```
- Quando houver várias condições comparando o mesmo campo/variável contra valores diferentes, dar preferência a `switch` em vez de uma cadeia de `if/else if` — fica mais legível. Exemplo:

  Errado:
  ```ts
  function getStatusLabel(status: TOrderStatus) {
    if (status === "pending") {
      return "Pendente";
    } else if (status === "paid") {
      return "Pago";
    } else if (status === "canceled") {
      return "Cancelado";
    } else {
      return "Desconhecido";
    }
  }
  ```

  Certo:
  ```ts
  function getStatusLabel(status: TOrderStatus) {
    switch (status) {
      case "pending":
        return "Pendente";
      case "paid":
        return "Pago";
      case "canceled":
        return "Cancelado";
      default:
        return "Desconhecido";
    }
  }
  ```
- O early return também se aplica dentro do `switch`: cada `case` deve dar `return` diretamente (como no exemplo acima), evitando `break` e variáveis intermediárias acumulando valor entre os `case`s

### Assincronismo
- SEMPRE usar `async/await` para métodos assíncronos, nunca encadeamento de `.then()`/`.catch()` — deixa o código mais legível e linear. Erros são tratados com `try/catch`. Exemplo:

  Errado:
  ```ts
  function fetchUser(id: string) {
    return api.get(`/users/${id}`)
      .then((response) => response.data)
      .catch((error) => {
        logger.error(error);
        throw error;
      });
  }
  ```

  Certo:
  ```ts
  async function fetchUser(id: string) {
    try {
      const response = await api.get(`/users/${id}`);
      return response.data;
    } catch (error) {
      logger.error(error);
      throw error;
    }
  }
  ```

### Nomenclatura
- Nomes de variáveis sempre claros e descritivos
- Nomes de predicados em callbacks (`.map`, `.filter`, etc.) também devem ser claros — ex: `users.map(user => user.name)`, não `users.map(u => u.name)`

### Constantes
- Nunca usar "magic numbers" (ou strings) — valores numéricos ou de texto soltos no meio da lógica, sem explicação do que representam. Extrair para uma constante com nome breve e explicativo, em `UPPER_SNAKE_CASE`. Exemplo:

  Errado:
  ```ts
  function applyDiscount(price: number) {
    if (price > 500) {
      return price * 0.9;
    }
    return price;
  }
  ```

  Certo:
  ```ts
  const FREE_SHIPPING_MIN_PRICE = 500;
  const DISCOUNT_RATE = 0.9;

  function applyDiscount(price: number) {
    if (price > FREE_SHIPPING_MIN_PRICE) {
      return price * DISCOUNT_RATE;
    }
    return price;
  }
  ```
- Constantes usadas em mais de um arquivo devem ficar centralizadas num arquivo próprio (ex: `constants.ts`), seguindo a mesma lógica de organização usada para `types.ts`. Constantes usadas só dentro de um único arquivo podem ficar declaradas no topo dele
- Valores como `0`, `1` ou `-1` usados em contextos óbvios (ex: incremento de loop, índice inicial) não precisam virar constante — a regra vale para valores cujo significado não é evidente pelo contexto

### Limpeza de código
- Remover imports não utilizados — nenhum import deve ficar no arquivo sem ser referenciado no código
- Variáveis e parâmetros não utilizados devem ser removidos sempre que possível. Quando não puder ser removido (ex: parâmetro exigido pela assinatura de uma função/interface, posição de um parâmetro que precisa ser mantida), prefixar com `_` para indicar descarte intencional (ex: `function handler(_event: Event, data: TData) { ... }`)
- NUNCA adicionar comentários no código (nem explicativos, nem de documentação de função). O código deve ser autoexplicativo e de fácil leitura — por meio de nomes claros, funções pequenas e bem definidas, e estrutura simples — a ponto de não precisar de nenhum comentário para ser entendido. Se sentir necessidade de comentar um trecho, é sinal de que o código precisa ser reescrito de forma mais clara, não de que falta um comentário. Única exceção: nos exemplos desta skill, comentários indicando apenas o nome/caminho do arquivo (ex: `// types.ts`) são usados só para separar blocos de código de arquivos diferentes num mesmo exemplo — isso não é um comentário de código real e não deve ser replicado como padrão de código em arquivos de verdade

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
