### Capítulo 11: Construindo uma Aplicação de Todo List com Angular e Bootstrap

Criar uma aplicação de Todo List é um excelente projeto inicial para aprender a usar Angular e Bootstrap, explorando a criação de componentes, a utilização de estilos responsivos e a manipulação de dados com Angular. Neste artigo, vamos construir uma aplicação funcional de Todo List em Angular, estilizada com Bootstrap, para adicionar, marcar como concluídas e remover tarefas de uma lista.

### Pré-requisitos

Antes de começarmos, você precisará:
- Node.js e npm instalados.
- Angular CLI instalado globalmente. Se você ainda não possui o Angular CLI, pode instalá-lo com o comando:
  ```bash
  npm install -g @angular/cli
  ```

### 1. Criação do Projeto Angular

Primeiro, vamos criar um novo projeto Angular chamado `todo-list`:
```bash
ng new todo-list
cd todo-list
```

### 2. Instalação do Bootstrap

Vamos adicionar o Bootstrap ao nosso projeto para estilizar a aplicação. A instalação é feita pelo npm:
```bash
npm install bootstrap
```

Em seguida, precisamos incluir o Bootstrap no arquivo de configuração `angular.json`. Localize a seção `"styles"` e adicione o caminho do Bootstrap:
```json
"styles": [
  "node_modules/bootstrap/dist/css/bootstrap.min.css",
  "src/styles.css"
]
```

### 3. Criação de um Componente Todo

Vamos criar um componente chamado `todo`, que será responsável pela lógica e pelo layout da nossa lista de tarefas:
```bash
ng generate component todo
```

### 4. Implementando o Layout do Componente

Abra o arquivo `todo.component.html` e substitua seu conteúdo pelo código HTML abaixo. Ele cria uma interface para adicionar tarefas, marcá-las como concluídas e removê-las:

```html
<div class="container mt-5">
    <h2 class="text-center mb-4">Todo List</h2>
  
    <!-- Formulário para adicionar tarefas -->
    <div class="input-group mb-3">
      <input [(ngModel)]="newTaskTitle" type="text" class="form-control" placeholder="Task Title" />
    </div>
    <div class="input-group mb-3">
      <input [(ngModel)]="newTaskDescription" type="text" class="form-control" placeholder="Task Description" />
    </div>
    <div class="d-flex justify-content-between">
      <button (click)="addTask()" class="btn btn-primary">Add Task</button>
    </div>
  
    <!-- Lista de tarefas -->
    <ul class="list-group mt-3">
      <li *ngFor="let task of tasks; let i = index" class="list-group-item d-flex justify-content-between align-items-center">
        <div>
          <h5 [class.text-decoration-line-through]="task.isCompleted">{{ task.title }}</h5>
          <p [class.text-decoration-line-through]="task.isCompleted">{{ task.description }}</p>
          <small>Created At: {{ task.createdAt }}</small>
        </div>
        <div>
          <button (click)="toggleTask(i)" class="btn btn-sm btn-outline-success me-2">{{ task.isCompleted ? 'Undo' : 'Complete' }}</button>
          <button (click)="removeTask(i)" class="btn btn-sm btn-outline-danger">Remove</button>
        </div>
      </li>
    </ul>
  </div>
```

```css
li {
    border: 1px solid #ddd;
    margin-bottom: 10px;
  }
  
  h5.text-decoration-line-through {
    text-decoration: line-through;
  }
  
  p.text-decoration-line-through {
    text-decoration: line-through;
  }
```

- **Input de tarefas**: o campo de input e o botão "Add" permitem que o usuário adicione novas tarefas.
- **Listagem de tarefas**: cada tarefa possui um botão "Done" para marcar como concluída (ou "Undo" para desfazer), e um botão "Remove" para excluí-la.

### 5. Implementação da Lógica no TypeScript

Agora, vamos adicionar a lógica de manipulação das tarefas no arquivo `todo.component.ts`:

```typescript
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { CommonModule } from '@angular/common';

interface Task {
  title: string;
  description: string;
  isCompleted: boolean;
  createdAt: string;
}

@Component({
  selector: 'app-todo',
  standalone: true,
  imports: [FormsModule, CommonModule],
  templateUrl: './todo.component.html',
  styleUrl: './todo.component.css'
})
export class TodoComponent {
  tasks: Task[] = [];
  newTaskTitle: string = '';
  newTaskDescription: string = '';

  // Função para adicionar uma nova tarefa
  addTask() {
    if (this.newTaskTitle.trim() && this.newTaskDescription.trim()) {
      const newTask: Task = {
        title: this.newTaskTitle.trim(),
        description: this.newTaskDescription.trim(),
        isCompleted: false,
        createdAt: new Date().toLocaleString()  // Formato de data
      };
      this.tasks.push(newTask);
      this.newTaskTitle = '';
      this.newTaskDescription = '';
    }
  }

  // Função para alternar o estado de conclusão de uma tarefa
  toggleTask(index: number) {
    this.tasks[index].isCompleted = !this.tasks[index].isCompleted;
  }

  // Função para remover uma tarefa
  removeTask(index: number) {
    this.tasks.splice(index, 1);
  }
}
```

Aqui, definimos três funções principais:
- **addTask()**: adiciona uma nova tarefa à lista.
- **toggleTask()**: alterna o status de uma tarefa entre concluída e não concluída.
- **removeTask()**: remove uma tarefa da lista.

### 6. Configuração do FormsModule

Para usar o `ngModel` em nosso campo de input, precisamos importar o `FormsModule`. No arquivo `app.module.ts`, adicione o FormsModule aos imports:

```typescript
import { Component } from '@angular/core';
import { RouterOutlet } from '@angular/router';
import { TodoComponent } from './todo/todo.component';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [TodoComponent,RouterOutlet],
  templateUrl: './app.component.html',
  styleUrl: './app.component.css'
})
export class AppComponent {
  title = 'ng-todo-list';
}
```

### 7. Incluindo o Componente na Aplicação Principal

Para exibir nossa lista de tarefas na aplicação principal, adicione o componente `<app-todo>` no arquivo `app.component.html`:

```html
<app-todo></app-todo>
```

### 8. Executando a Aplicação

Agora estamos prontos para ver o resultado. No terminal, execute:
```bash
ng serve
```

Abra o navegador e vá para `http://localhost:4200`. Agora você deverá ver uma aplicação de Todo List onde é possível adicionar, marcar como concluídas e remover tarefas, tudo estilizado com o Bootstrap.

### Conclusão

Este projeto é um excelente exemplo de como usar Angular e Bootstrap para construir uma aplicação interativa e estilosa. Com ele, você aprendeu a:
- Criar componentes e manipular dados com Angular.
- Utilizar o Bootstrap para adicionar estilos.
- Gerenciar o estado da aplicação e as interações do usuário.

Essa estrutura pode ser expandida com mais funcionalidades, como a persistência de dados em um banco de dados ou a utilização de uma API, mas agora você já tem uma base sólida para desenvolver aplicações mais completas com Angular e Bootstrap.