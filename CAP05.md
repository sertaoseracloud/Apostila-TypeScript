# Capítulo 5: Manipulação de DOM com TypeScript

Manipular o Document Object Model (DOM) é uma parte fundamental do desenvolvimento web. Em TypeScript, essa manipulação é feita de maneira muito parecida com o JavaScript tradicional, mas com a vantagem de usar tipagem estática para garantir maior segurança e robustez no código.

## 1. Acessando e Manipulando Elementos do DOM

Para acessar elementos do DOM, você pode usar funções como `getElementById`, `querySelector`, e `getElementsByClassName`. No entanto, em TypeScript, é necessário tipar corretamente os elementos para evitar erros em tempo de compilação.

### Exemplo Básico: Manipulação de DOM

```typescript
const heading = document.getElementById("main-heading") as HTMLHeadingElement;
heading.textContent = "Bem-vindo ao TypeScript!";
```

Aqui, usamos `as HTMLHeadingElement` para especificar o tipo do elemento, garantindo que o TypeScript saiba que estamos lidando com um elemento de cabeçalho.

## 2. Tipagem de Eventos e Listeners

Os eventos em TypeScript podem ser tipados para garantir que estamos lidando corretamente com o tipo de evento esperado. Isso reduz erros comuns, como tentar acessar propriedades que não existem no evento.

### 2.1 Exemplo de Eventos

Aqui está um exemplo de como tipar um evento de clique em TypeScript:

```typescript
const button = document.getElementById("myButton") as HTMLButtonElement;

button.addEventListener("click", (event: MouseEvent) => {
    console.log("Botão clicado!", event.clientX, event.clientY);
});
```

No exemplo acima, usamos o tipo `MouseEvent` para tipar o evento de clique. Isso garante que o TypeScript saiba que estamos lidando com um evento de mouse, permitindo acessar propriedades como `clientX` e `clientY`.

### 2.2 Eventos de Entrada de Texto

Para eventos como `input`, o evento também deve ser tipado corretamente:

```typescript
const input = document.getElementById("myInput") as HTMLInputElement;

input.addEventListener("input", (event: Event) => {
    const target = event.target as HTMLInputElement;
    console.log(target.value);
});
```

Neste caso, usamos `Event` como o tipo do evento e, em seguida, utilizamos **type assertion** para garantir que o `event.target` é um `HTMLInputElement`.

## 3. Integração com Bibliotecas JavaScript Populares

TypeScript é totalmente compatível com bibliotecas JavaScript populares, como **jQuery**, **React**, **Vue**, entre outras. Ao usar essas bibliotecas com TypeScript, você pode se beneficiar de tipagem estática, que ajuda a reduzir erros de tempo de execução e facilita o autocompletar em IDEs.

### 3.1 jQuery com TypeScript

Para usar jQuery com TypeScript, você precisa instalar os tipos correspondentes à biblioteca:

```bash
npm install --save @types/jquery
```

Exemplo de uso com jQuery:

```typescript
import * as $ from "jquery";

$(document).ready(() => {
    $("#myButton").on("click", () => {
        alert("Botão clicado!");
    });
});
```

Neste exemplo, estamos importando o jQuery e utilizando tipagem estática fornecida pelos tipos instalados.

### 3.2 React com TypeScript

No caso de React, o suporte a TypeScript é nativo e pode ser integrado diretamente ao seu projeto:

```tsx
type ButtonProps = {
    text: string;
};

const MyButton: React.FC<ButtonProps> = ({ text }) => {
    return <button>{text}</button>;
};
```

Aqui, estamos criando um componente React com TypeScript, utilizando o tipo `ButtonProps` para definir as propriedades do componente.

## 4. Exemplo Final: Aplicação de Lista de Tarefas

Neste exemplo, vamos combinar manipulação de DOM e eventos em TypeScript para criar uma aplicação simples de lista de tarefas.

### HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lista de Tarefas</title>
</head>
<body>
    <h1>Lista de Tarefas</h1>
    <input id="taskInput" type="text" placeholder="Nova tarefa">
    <button id="addTaskButton">Adicionar Tarefa</button>
    <ul id="taskList"></ul>

    <script src="app.js"></script>
</body>
</html>
```

### TypeScript

```typescript
const taskInput = document.getElementById("taskInput") as HTMLInputElement;
const addTaskButton = document.getElementById("addTaskButton") as HTMLButtonElement;
const taskList = document.getElementById("taskList") as HTMLUListElement;

addTaskButton.addEventListener("click", () => {
    const task = taskInput.value;
    if (task) {
        const li = document.createElement("li");
        li.textContent = task;
        taskList.appendChild(li);
        taskInput.value = ""; // Limpa o campo de entrada
    }
});
```

Neste exemplo, o usuário pode adicionar novas tarefas à lista, manipulando o DOM e eventos de forma tipada.

## 5. Perguntas para Reflexão

1. O que é o DOM e como ele é manipulado em TypeScript?
2. Qual a diferença entre a manipulação de DOM em JavaScript e TypeScript?
3. Como você tiparia um evento de clique no TypeScript?
4. Qual o benefício de usar `as` para afirmar o tipo de um elemento do DOM?
5. Como a tipagem estática pode ajudar na manipulação de eventos?
6. O que é um `MouseEvent` e como ele é usado?
7. Como você pode integrar jQuery em um projeto TypeScript?
8. Qual a vantagem de utilizar bibliotecas JavaScript populares com TypeScript?
9. Quais são as melhores práticas para manipular o DOM com TypeScript?
10. Como as interfaces de TypeScript podem ser usadas para melhorar o código em bibliotecas como React?

