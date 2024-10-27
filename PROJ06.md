### TODO List com React TypeScript e Bootstrap

Este artigo vai guiá-lo na criação de uma aplicação **TODO List** com **React TypeScript** usando o **ViteJS** para inicialização rápida e **Bootstrap** para estilização.

### Passo 1: Criar o Projeto com ViteJS

Primeiro, vamos iniciar o projeto com o Vite, uma ferramenta de build que oferece uma configuração simplificada para projetos React.

1. Abra o terminal e execute o comando abaixo para criar o projeto:
   ```bash
   npm create vite@latest my-todo-app --template react-ts
   ```
   Esse comando cria um projeto com React e TypeScript.

2. Navegue até a pasta do projeto e instale as dependências:
   ```bash
   cd my-todo-app
   npm install
   ```

### Passo 2: Instalar o Bootstrap

Para adicionar o Bootstrap, execute o comando abaixo:

```bash
npm install bootstrap
```

Agora, importe o CSS do Bootstrap em `main.tsx` para que possamos usar as classes do Bootstrap em todo o projeto.

```typescript
// src/main.tsx
import 'bootstrap/dist/css/bootstrap.min.css';
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

### Passo 3: Criar a Estrutura do Componente `App.tsx`

Vamos criar o arquivo `App.tsx`, que será o componente principal do projeto. Esse componente vai conter toda a lógica da nossa TODO List, incluindo funções para adicionar, remover e marcar tarefas como concluídas.

1. Crie o arquivo `App.tsx` na pasta `src` e adicione o seguinte código:

```typescript
// src/App.tsx
import React, { useState } from 'react';

interface Todo {
  id: number;
  task: string;
  completed: boolean;
}

const App: React.FC = () => {
  const [todos, setTodos] = useState<Todo[]>([]);
  const [task, setTask] = useState<string>("");

  // Função para adicionar um item
  const addTodo = () => {
    if (task.trim()) {
      const newTodo: Todo = {
        id: Date.now(),
        task: task,
        completed: false,
      };
      setTodos([...todos, newTodo]);
      setTask("");
    }
  };

  // Função para remover um item
  const removeTodo = (id: number) => {
    setTodos(todos.filter(todo => todo.id !== id));
  };

  // Função para alternar a conclusão de uma tarefa
  const toggleComplete = (id: number) => {
    setTodos(
      todos.map(todo =>
        todo.id === id ? { ...todo, completed: !todo.completed } : todo
      )
    );
  };

  return (
    <div className="container mt-5">
      <h2 className="text-center">Todo List</h2>

      <div className="input-group mb-3">
        <input
          type="text"
          className="form-control"
          placeholder="Adicionar nova tarefa"
          value={task}
          onChange={(e) => setTask(e.target.value)}
        />
        <button className="btn btn-primary" onClick={addTodo}>Adicionar</button>
      </div>

      <ul className="list-group">
        {todos.map((todo) => (
          <li key={todo.id} className="list-group-item d-flex justify-content-between align-items-center">
            <div>
              <input
                type="checkbox"
                className="form-check-input me-2"
                checked={todo.completed}
                onChange={() => toggleComplete(todo.id)}
              />
              <span style={{ textDecoration: todo.completed ? "line-through" : "none" }}>
                {todo.task}
              </span>
            </div>
            <button className="btn btn-danger btn-sm" onClick={() => removeTodo(todo.id)}>
              Remover
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default App;
```

### Passo 4: Executar o Projeto

Agora que a estrutura está pronta, é hora de executar o projeto:

1. No terminal, execute:
   ```bash
   npm run dev
   ```

2. Abra seu navegador e acesse `http://localhost:3000`. Você deverá ver a interface da TODO List funcionando.

### Explicação do Código

1. **Estrutura de Estado**:
   - Utilizamos dois estados principais:
     - `todos`: Array de tarefas a serem gerenciadas, onde cada tarefa é um objeto com `id`, `task` e `completed`.
     - `task`: Armazena o valor do input para a nova tarefa.

2. **Funções**:
   - `addTodo`: Cria um novo item e adiciona à lista `todos`.
   - `removeTodo`: Filtra a lista para remover a tarefa com o `id` correspondente.
   - `toggleComplete`: Alterna o estado de conclusão de uma tarefa específica.

3. **Estilização com Bootstrap**:
   - Usamos classes do Bootstrap para o layout, como `container`, `input-group` e `btn`, tornando o visual mais organizado e responsivo.

Com esses passos, você tem uma aplicação TODO List simples e prática, estilizada com Bootstrap e desenvolvida em React TypeScript!