# Capítulo 4: Módulos e Namespaces em TypeScript

## 4.1 Módulos em TypeScript

Módulos são uma maneira de organizar e reutilizar código em TypeScript, facilitando a manutenção e a escalabilidade dos projetos. Eles permitem que você separe seu código em diferentes arquivos e componentes, tornando-o mais modular.

### 4.1.1 Exportação de Componentes

Para compartilhar código entre diferentes arquivos, você pode usar a palavra-chave `export` para tornar funções, classes, interfaces ou variáveis disponíveis em outros módulos.

```typescript
// mathFunctions.ts
export function add(x: number, y: number): number {
    return x + y;
}

export function subtract(x: number, y: number): number {
    return x - y;
}
```

Aqui, as funções `add` e `subtract` foram exportadas, tornando-as acessíveis em outros arquivos.

### 4.1.2 Importação de Componentes

Uma vez que os componentes foram exportados, você pode importá-los em outros arquivos para usá-los.

```typescript
// app.ts
import { add, subtract } from './mathFunctions';

console.log(add(5, 3)); // 8
console.log(subtract(5, 3)); // 2
```

Neste exemplo, as funções `add` e `subtract` foram importadas do módulo `mathFunctions` e usadas no arquivo `app.ts`.

### 4.1.3 Exportação Padrão (Default Export)

Você também pode usar `export default` para exportar um único valor ou objeto de um módulo.

```typescript
// person.ts
export default class Person {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
}

// app.ts
import Person from './person';

const person = new Person("Alice");
console.log(person.name); // Alice
```

O `export default` facilita a importação, permitindo que o componente seja importado sem chaves.

## 4.2 Namespaces

Namespaces fornecem uma maneira de organizar o código dentro de um único arquivo ou módulo, agrupando código relacionado e evitando conflitos de nome. Eles são úteis quando você deseja encapsular código em um escopo lógico.

### 4.2.1 Definição de Namespaces

Você pode definir namespaces usando a palavra-chave `namespace`. Todo o código dentro do namespace está encapsulado e acessível somente por meio do namespace.

```typescript
namespace Geometry {
    export function calculateArea(radius: number): number {
        return Math.PI * radius * radius;
    }

    export function calculateCircumference(radius: number): number {
        return 2 * Math.PI * radius;
    }
}

console.log(Geometry.calculateArea(5)); // 78.53981633974483
console.log(Geometry.calculateCircumference(5)); // 31.41592653589793
```

Neste exemplo, as funções `calculateArea` e `calculateCircumference` estão encapsuladas no namespace `Geometry`.

### 4.2.2 Encapsulamento de Código com Namespaces

Namespaces ajudam a evitar conflitos de nome em grandes projetos. Componentes dentro de um namespace só podem ser acessados pelo nome do namespace.

```typescript
namespace Shapes {
    export class Circle {
        constructor(public radius: number) {}

        area(): number {
            return Math.PI * this.radius * this.radius;
        }
    }
}

const circle = new Shapes.Circle(10);
console.log(circle.area()); // 314.1592653589793
```

## 4.3 Ferramentas de Build: Webpack e Babel

Ferramentas como **Webpack** e **Babel** são amplamente utilizadas em projetos TypeScript para compilar, empacotar e otimizar o código.

### 4.3.1 Webpack

**Webpack** é uma ferramenta que empacota vários arquivos JavaScript e suas dependências em um único arquivo. Ele também pode processar arquivos TypeScript.

Para usar o Webpack com TypeScript, você precisa instalar o Webpack, o `ts-loader` e o TypeScript:

```bash
npm install --save-dev webpack webpack-cli ts-loader typescript
```

Exemplo de configuração (`webpack.config.js`):

```javascript
module.exports = {
    entry: './src/index.ts',
    module: {
        rules: [
            {
                test: /\.ts$/,
                use: 'ts-loader',
                exclude: /node_modules/,
            },
        ],
    },
    resolve: {
        extensions: ['.ts', '.js'],
    },
    output: {
        filename: 'bundle.js',
        path: __dirname + '/dist',
    },
};
```

### 4.3.2 Babel

**Babel** é um compilador que permite escrever código JavaScript moderno, que pode ser convertido para versões mais antigas da linguagem para maior compatibilidade.

Para usar Babel com TypeScript:

```bash
npm install --save-dev @babel/core @babel/preset-env @babel/preset-typescript babel-loader
```

Exemplo de configuração (`babel.config.js`):

```javascript
module.exports = {
    presets: [
        '@babel/preset-env',
        '@babel/preset-typescript',
    ],
};
```

## 4.4 Manipulação de DOM e Eventos

Manipular o DOM e lidar com eventos são tarefas essenciais no desenvolvimento web. Em TypeScript, você pode fazer isso de forma semelhante ao JavaScript, mas com o suporte de tipagem.

### 4.4.1 Manipulação de DOM

Vamos ver como acessar e manipular elementos HTML com TypeScript.

```typescript
const button = document.getElementById("myButton") as HTMLButtonElement;
button.addEventListener("click", () => {
    const input = document.getElementById("myInput") as HTMLInputElement;
    alert(`Hello, ${input.value}!`);
});
```

Aqui, estamos manipulando um botão e um campo de entrada. O `as HTMLButtonElement` e `as HTMLInputElement` são exemplos de **type assertions**, garantindo que o TypeScript saiba o tipo correto do elemento.

### 4.4.2 Eventos

Além de manipular o DOM, você pode lidar com eventos, como cliques ou digitação de texto, de maneira segura com TypeScript.

```typescript
const input = document.getElementById("myInput") as HTMLInputElement;
input.addEventListener("input", (event: Event) => {
    const target = event.target as HTMLInputElement;
    console.log(`Input value: ${target.value}`);
});
```

Aqui, estamos capturando o evento `input`, e `event.target` é utilizado para acessar o valor atual do campo de entrada.

## 4.5 Exemplo Final: App de Contador com Módulos e Manipulação de DOM

Neste exemplo, vamos combinar tudo que aprendemos em um simples app de contador.

### `counter.ts` (Módulo de Contador)

```typescript
export class Counter {
    private count: number = 0;

    increment(): void {
        this.count++;
    }

    decrement(): void {
        this.count--;
    }

    getCount(): number {
        return this.count;
    }
}
```

### `app.ts` (Manipulação de DOM)

```typescript
import { Counter } from './counter';

const counter = new Counter();

const incrementButton = document.getElementById("increment") as HTMLButtonElement;
const decrementButton = document.getElementById("decrement") as HTMLButtonElement;
const display = document.getElementById("display") as HTMLElement;

function updateDisplay(): void {
    display.textContent = counter.getCount().toString();
}

incrementButton.addEventListener("click", () => {
    counter.increment();
    updateDisplay();
});

decrementButton.addEventListener("click", () => {
    counter.decrement();
    updateDisplay();
});

updateDisplay();
```

## 4.6 Perguntas para Reflexão

1. **O que é um módulo em TypeScript e como ele é utilizado?**
2. **Qual a diferença entre `export` e `export default`?**
3. **Como namespaces ajudam a organizar o código?**
4. **Como você pode evitar conflitos de nome em um projeto grande?**
5. **O que é o Webpack e como ele auxilia no desenvolvimento?**
6. **Qual o propósito do Babel em projetos TypeScript?**
7. **Como você manipula o DOM com TypeScript?**
8. **Como eventos são tratados em TypeScript?**
9. **O que são type assertions e por que são importantes na manipulação de DOM?**
10. **Quais são os benefícios de usar ferramentas de build como Webpack e Babel em projetos TypeScript?**
