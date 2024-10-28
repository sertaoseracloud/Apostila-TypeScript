# Capítulo 9: Gerenciando o Ciclo de Vida de Componentes com useEffect e TypeScript

O React introduziu os hooks para facilitar o gerenciamento do estado e dos efeitos colaterais em componentes funcionais. Um dos hooks mais importantes é o `useEffect`, que permite que você execute efeitos colaterais em componentes funcionais, funcionando de maneira semelhante aos métodos de ciclo de vida em componentes de classe. Neste capítulo, exploraremos como utilizar o `useEffect` para gerenciar o ciclo de vida de um componente em aplicações React com TypeScript.

## O Que é o useEffect?

O `useEffect` é um hook que aceita uma função que contém código a ser executado após a renderização do componente. Ele é usado para lidar com efeitos colaterais, como chamadas a APIs, manipulação de DOM, ou subscrições.

### Sintaxe Básica

```typescript
useEffect(() => {
  // Código do efeito
}, [dependencies]);
```

- **Função do efeito**: Código que você deseja executar.
- **Array de dependências**: Um array que contém valores dos quais o efeito depende. Se algum desses valores mudar, o efeito será re-executado.

## Exemplo Básico

Vamos começar com um exemplo simples onde usamos o `useEffect` para atualizar o título do documento com base no estado de um contador.

### Componente Contador com useEffect

```typescript
import React, { useState, useEffect } from 'react';

const Counter: React.FC = () => {
  const [count, setCount] = useState<number>(0);

  useEffect(() => {
    document.title = `Contador: ${count}`;
  }, [count]);

  return (
    <div>
      <p>Contador: {count}</p>
      <button onClick={() => setCount(count + 1)}>Incrementar</button>
      <button onClick={() => setCount(count - 1)}>Decrementar</button>
    </div>
  );
};

export default Counter;
```

Neste exemplo, toda vez que o `count` é atualizado, o título do documento é alterado para refletir o valor atual do contador.

## Efeitos Colaterais e o Ciclo de Vida

O `useEffect` permite que você imite os métodos de ciclo de vida de componentes de classe, como `componentDidMount`, `componentDidUpdate` e `componentWillUnmount`.

### 1. **componentDidMount**

O `componentDidMount` é chamado uma vez após o primeiro render. Para replicar esse comportamento com `useEffect`, você pode passar um array de dependências vazio.

```typescript
useEffect(() => {
  // Código que deve ser executado apenas uma vez
  console.log('Componente montado');
}, []);
```

### 2. **componentDidUpdate**

Se você quiser executar um efeito sempre que uma determinada variável mudar, basta incluí-la na lista de dependências.

```typescript
useEffect(() => {
  console.log('O contador mudou para: ', count);
}, [count]);
```

### 3. **componentWillUnmount**

Para lidar com a limpeza de efeitos, como cancelar assinaturas ou limpar temporizadores, você pode retornar uma função de limpeza do `useEffect`.

```typescript
useEffect(() => {
  const timer = setTimeout(() => {
    console.log('Timer executado!');
  }, 1000);

  return () => clearTimeout(timer); // Limpeza
}, []);
```

## Exemplo Completo: Chamadas a APIs

Vamos agora criar um exemplo mais complexo onde usamos `useEffect` para fazer uma chamada a uma API. O componente buscará dados de usuários e os exibirá.

### Componente de Lista de Usuários

```typescript
import React, { useState, useEffect } from 'react';

interface User {
  id: number;
  name: string;
  email: string;
}

const UserList: React.FC = () => {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const fetchUsers = async () => {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        if (!response.ok) {
          throw new Error('Erro ao buscar usuários');
        }
        const data = await response.json();
        setUsers(data);
      } catch (error) {
        setError(error.message);
      } finally {
        setLoading(false);
      }
    };

    fetchUsers();
  }, []); // Chamada apenas uma vez

  if (loading) return <p>Carregando...</p>;
  if (error) return <p>Erro: {error}</p>;

  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>
          {user.name} - {user.email}
        </li>
      ))}
    </ul>
  );
};

export default UserList;
```

### Explicação do Exemplo

1. **Estado**: Temos três estados: `users`, `loading` e `error`.
2. **Efeito**: O `useEffect` é usado para buscar os usuários assim que o componente é montado, usando uma função assíncrona chamada `fetchUsers`.
3. **Tratamento de Erros**: Se a chamada falhar, definimos o estado de `error` com a mensagem de erro.
4. **Renderização Condicional**: O componente renderiza mensagens de carregamento ou erro, dependendo do estado.

## Limpeza de Efeitos

Se o seu efeito tiver alguma lógica que precisa ser limpa ao desmontar o componente, você deve retornar uma função de limpeza. Isso é especialmente importante para evitar vazamentos de memória e comportamentos inesperados.

### Exemplo de Timer com Limpeza

```typescript
import React, { useState, useEffect } from 'react';

const Timer: React.FC = () => {
  const [seconds, setSeconds] = useState<number>(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds(prev => prev + 1);
    }, 1000);

    return () => clearInterval(interval); // Limpeza
  }, []);

  return <div>Segundos: {seconds}</div>;
};

export default Timer;
```

Neste exemplo, o intervalo é limpo quando o componente é desmontado, evitando que o contador continue a ser incrementado após a remoção do componente.

## Resumo das Considerações sobre useEffect

1. **Componente Montado**: Para simular `componentDidMount`, use um array vazio como dependência.
2. **Atualizações**: Para simular `componentDidUpdate`, inclua as variáveis necessárias no array de dependências.
3. **Desmontagem**: Use uma função de limpeza para simular `componentWillUnmount` e evitar vazamentos de memória.

## Conclusão

O `useEffect` é uma ferramenta poderosa para gerenciar efeitos colaterais em componentes funcionais no React. Ao entender como o `useEffect` se relaciona com os métodos de ciclo de vida, você pode criar componentes mais eficientes e organizados. Ao combinar o `useEffect` com TypeScript, você também garante um código mais seguro e fácil de manter.

Neste capítulo, abordamos como usar o `useEffect` para simular o ciclo de vida de um componente, gerenciar chamadas a APIs e realizar limpezas de efeitos. Com essas informações, você estará bem equipado para usar `useEffect` em seus projetos React com TypeScript!

