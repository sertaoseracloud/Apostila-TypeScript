### Capítulo 7: Introdução ao ReactJS

O ReactJS é uma biblioteca JavaScript criada pelo Facebook para construir interfaces de usuário (UI) interativas e dinâmicas. Ele usa o conceito de **componentização**, onde a interface é dividida em partes menores e reutilizáveis chamadas componentes. Cada componente tem seu próprio ciclo de vida e pode ser atualizado dinamicamente, proporcionando uma experiência de desenvolvimento eficiente e uma aplicação reativa.

### Como o React Funciona

O React utiliza o **Virtual DOM**, uma representação leve do DOM real. Quando o estado de um componente muda, o React cria uma nova versão do Virtual DOM, identifica as diferenças e aplica as alterações no DOM real, resultando em um desempenho mais eficiente.

### O que é JSX e TSX?

**JSX** (JavaScript XML) é uma sintaxe que permite escrever HTML dentro do JavaScript, tornando o código mais intuitivo e legível. Ele é convertido em JavaScript puro durante a compilação. **TSX** é a versão para **TypeScript**, que adiciona tipagem estática ao JSX, permitindo maior segurança e controle sobre o código.

```javascript
const Component = () => <h1>Hello, World!</h1>;
```

### Criando o Primeiro Projeto com ViteJS

**Vite** é uma ferramenta para inicializar e construir projetos React rapidamente. Para começar:

1. **Instale o Vite**:
   ```bash
   npm create vite@latest my-react-app
   cd my-react-app
   npm install
   npm run dev
   ```

2. **Escolha o template React com TypeScript** ao criar o projeto.

3. Acesse `http://localhost:3000` no navegador para ver sua aplicação.

### Hooks Essenciais: `useState` e `useEffect`

#### `useState`

O `useState` é um hook que permite gerenciar o estado local de um componente. Ele retorna um par de valores: o estado atual e uma função para atualizar esse estado.

```javascript
import { useState } from 'react';

const Counter = () => {
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>Incrementar</button>
    </div>
  );
};
```

#### `useEffect`

O `useEffect` permite executar efeitos colaterais em componentes, como chamadas de API ou manipulação do DOM, e aceita uma lista de dependências que controla quando o efeito deve ser reexecutado.

```javascript
import { useEffect, useState } from 'react';

const DataFetcher = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch('https://api.example.com/data')
      .then(response => response.json())
      .then(data => setData(data));
  }, []);

  return <div>{data ? <p>{data}</p> : <p>Carregando...</p>}</div>;
};
```

### Componentização e Parâmetros (Props)

A **componentização** permite dividir a interface em blocos reutilizáveis, chamados componentes, que podem receber **propriedades** (props). As props são parâmetros passados para componentes, que podem exibir conteúdo variável de acordo com as props recebidas.

```javascript
const Greeting = ({ name }) => <h1>Olá, {name}!</h1>;

const App = () => (
  <div>
    <Greeting name="João" />
    <Greeting name="Maria" />
  </div>
);
```

### Composição de Componentes

Podemos compor componentes para criar uma estrutura modular. Por exemplo, o componente `UserProfile` pode reunir `Avatar` e `UserInfo`, cada um responsável por uma parte da interface.

```javascript
const Avatar = ({ url }) => <img src={url} alt="Avatar" />;

const UserInfo = ({ name, bio }) => (
  <div>
    <h2>{name}</h2>
    <p>{bio}</p>
  </div>
);

const UserProfile = ({ user }) => (
  <div className="user-profile">
    <Avatar url={user.avatar} />
    <UserInfo name={user.name} bio={user.bio} />
  </div>
);

const App = () => {
  const user = { name: "Alice", bio: "Desenvolvedora Frontend", avatar: "https://exemplo.com/avatar.jpg" };
  return <UserProfile user={user} />;
};
```

### Conclusão

Com React, JSX e TSX, e os hooks `useState` e `useEffect`, você tem uma base sólida para criar interfaces modernas. A componentização e as props ajudam a construir UIs escaláveis e reutilizáveis. Com ViteJS, a criação de projetos React fica ainda mais ágil.