### Capítulo 10: Criando uma Todo List com Vue.js

Neste artigo, vamos criar uma aplicação simples de lista de tarefas ("Todo List") usando Vue.js. Esta aplicação terá funcionalidades para adicionar, marcar como concluída e remover tarefas. Vue.js é uma framework JavaScript progressiva que facilita a criação de interfaces interativas com componentes reativos.

#### Estrutura do Projeto

1. **HTML (Template)**: Contém a estrutura visual da aplicação, como o campo de entrada de texto e a lista de tarefas.
2. **JavaScript (Script)**: Define a lógica para adicionar e remover tarefas, além de atualizar o status de conclusão.
3. **CSS (Style)**: Adiciona um estilo básico para destacar tarefas concluídas.

### Código Completo

#### Template

```html
<template>
  <div id="app" class="container mt-5">
    <h1 class="text-center mb-4">Todo List</h1>
    
    <!-- Formulário de entrada -->
    <div class="input-group mb-3">
      <input
        type="text"
        v-model="newTodo"
        @keyup.enter="addTodo"
        placeholder="Adicione uma nova tarefa"
        class="form-control"
      />
      <button @click="addTodo" class="btn btn-primary">Adicionar</button>
    </div>

    <!-- Lista de tarefas -->
    <ul class="list-group">
      <li
        v-for="todo in todos"
        :key="todo.id"
        class="list-group-item d-flex justify-content-between align-items-center"
      >
        <div>
          <input
            type="checkbox"
            v-model="todo.completed"
            class="form-check-input me-2"
          />
          <span :class="{ 'text-decoration-line-through': todo.completed }">
            {{ todo.text }}
          </span>
        </div>
        <button @click="removeTodo(todo.id)" class="btn btn-danger btn-sm">
          Remover
        </button>
      </li>
    </ul>
  </div>
</template>
```

#### Script

Aqui, usamos o `data` para armazenar a lista de tarefas e `methods` para definir as funções `addTodo` e `removeTodo`.

```javascript
<script>
export default {
  data() {
    return {
      newTodo: "",
      todos: []
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim()) {
        // Adiciona uma nova tarefa com um ID único
        this.todos.push({ id: Date.now(), text: this.newTodo, completed: false });
        this.newTodo = "";
      }
    },
    removeTodo(id) {
      // Filtra a tarefa com base no ID
      this.todos = this.todos.filter(todo => todo.id !== id);
    }
  }
};
</script>
```

#### Estilos

Definimos o estilo `.text-decoration-line-through` para riscar o texto das tarefas concluídas.

```css
<style scoped>
.text-decoration-line-through {
  text-decoration: line-through;
}
</style>
```

### Explicação das Funcionalidades

- **Adicionar Tarefa**: No método `addTodo`, verificamos se o campo de entrada não está vazio e adicionamos a nova tarefa ao array `todos`.
- **Remover Tarefa**: No método `removeTodo`, removemos uma tarefa específica usando seu `id`.
- **Marcar como Concluída**: Utilizamos `v-model` para sincronizar a propriedade `completed` da tarefa com o checkbox, aplicando a classe `.text-decoration-line-through` quando a tarefa é marcada como concluída.

### Conclusão

Este exemplo demonstra como o Vue.js torna fácil a criação de uma interface reativa e interativa com código claro e organizado.

### Perguntas para reflexão

1. Como posso otimizar a organização do código?  
2. Como gerenciar o estado da aplicação de forma mais eficiente?  
3. Como posso armazenar as tarefas localmente?  
4. Como implementar validações mais robustas?  
5. Quais melhorias de acessibilidade podem ser feitas?  
6. Como posso melhorar a experiência do usuário com animações?  
7. Como implementar testes para garantir a funcionalidade?  
8. Que outras funcionalidades poderiam ser agregadas?  
9. Como escalar este projeto?  
10. Que outras tecnologias ou frameworks poderiam ser integrados?