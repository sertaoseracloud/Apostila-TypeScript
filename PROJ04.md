# Jogo da Cobra com Orientação a Objetos em TypeScript

Neste Projeto, vamos desenvolver um projeto prático: o clássico jogo da cobra, utilizando **TypeScript** e aplicando conceitos de **orientação a objetos** (OO). O objetivo é ajudar os alunos a entenderem como a orientação a objetos pode ser aplicada em um projeto real, organizando o código em classes e objetos, e manipulando elementos do **DOM** para renderizar o jogo no navegador.

## 1. Estrutura Básica do Projeto

Antes de começar, vamos organizar a estrutura do projeto. Você precisará dos seguintes arquivos:

- **index.html**: para a interface do jogo.
- **styles.css**: para estilização básica.
- **game.ts**: onde o código TypeScript do jogo será escrito.
- **tsconfig.json**: configuração do TypeScript.

### 1.1 index.html

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jogo da Cobra</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Jogo da Cobra</h1>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    <script src="game.js"></script>
</body>
</html>
```

### 1.2 styles.css

```css
body {
    text-align: center;
    font-family: Arial, sans-serif;
}

canvas {
    background-color: #000;
    border: 2px solid #ccc;
}
```

### 1.3 tsconfig.json

```json
{
  "compilerOptions": {
    "target": "ES5",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

## 2. Classes e Lógica do Jogo

Agora vamos implementar o código do jogo da cobra em TypeScript utilizando a **orientação a objetos**. Vamos criar classes para representar a cobra, o jogo, e a comida.

### 2.1 Classe Snake (Cobra)

A classe **Snake** será responsável por armazenar a posição e o movimento da cobra.

```typescript
class Snake {
    body: { x: number, y: number }[]; // Segmentos da cobra
    direction: { x: number, y: number }; // Direção do movimento

    constructor() {
        this.body = [{ x: 10, y: 10 }]; // Cobra começa com um segmento
        this.direction = { x: 1, y: 0 }; // Movendo para a direita
    }

    move() {
        const head = { x: this.body[0].x + this.direction.x, y: this.body[0].y + this.direction.y };
        this.body.unshift(head); // Adiciona a nova posição da cabeça
        this.body.pop(); // Remove o último segmento
    }

    changeDirection(newDirection: { x: number, y: number }) {
        this.direction = newDirection;
    }

    grow() {
        const tail = this.body[this.body.length - 1];
        this.body.push({ x: tail.x, y: tail.y }); // Adiciona um novo segmento à cobra
    }
}
```

### 2.2 Classe Food (Comida)

A classe **Food** será responsável por gerar e exibir a comida em uma posição aleatória no tabuleiro.

```typescript
class Food {
    position: { x: number, y: number };

    constructor() {
        this.position = this.randomPosition();
    }

    randomPosition() {
        const x = Math.floor(Math.random() * 20);
        const y = Math.floor(Math.random() * 20);
        return { x, y };
    }

    draw(context: CanvasRenderingContext2D) {
        context.fillStyle = "red";
        context.fillRect(this.position.x * 20, this.position.y * 20, 20, 20);
    }
}
```

### 2.3 Classe Game (Jogo)

A classe **Game** será responsável pela lógica do jogo, incluindo a renderização, detecção de colisões e controle do estado geral.

```typescript
class Game {
    canvas: HTMLCanvasElement;
    context: CanvasRenderingContext2D;
    snake: Snake;
    food: Food;
    gridSize: number;

    constructor(canvasId: string) {
        this.canvas = document.getElementById(canvasId) as HTMLCanvasElement;
        this.context = this.canvas.getContext("2d")!;
        this.snake = new Snake();
        this.food = new Food();
        this.gridSize = 20;

        document.addEventListener("keydown", this.handleKey.bind(this)); // Ligações de eventos de teclado
    }

    start() {
        setInterval(() => {
            this.update();
            this.draw();
        }, 100);
    }

    update() {
        this.snake.move();
        // Verifica colisão com comida
        if (this.snake.body[0].x === this.food.position.x && this.snake.body[0].y === this.food.position.y) {
            this.snake.grow();
            this.food = new Food(); // Nova comida aparece
        }
    }

    draw() {
        this.context.clearRect(0, 0, this.canvas.width, this.canvas.height); // Limpa o canvas

        // Desenha a cobra
        this.snake.body.forEach(segment => {
            this.context.fillStyle = "lime";
            this.context.fillRect(segment.x * this.gridSize, segment.y * this.gridSize, this.gridSize, this.gridSize);
        });

        // Desenha a comida
        this.food.draw(this.context);
    }

    handleKey(event: KeyboardEvent) {
        switch (event.key) {
            case "ArrowUp":
                this.snake.changeDirection({ x: 0, y: -1 });
                break;
            case "ArrowDown":
                this.snake.changeDirection({ x: 0, y: 1 });
                break;
            case "ArrowLeft":
                this.snake.changeDirection({ x: -1, y: 0 });
                break;
            case "ArrowRight":
                this.snake.changeDirection({ x: 1, y: 0 });
                break;
        }
    }
}
```

### 2.4 Inicializando o Jogo

Agora, vamos instanciar o jogo e começar a execução.

```typescript
const game = new Game("gameCanvas");
game.start();
```

### 3. Compilando o Código

Após escrever o código TypeScript, compile-o para JavaScript utilizando o compilador **tsc**. No terminal, execute:

```bash
tsc
```

Isso criará o arquivo JavaScript necessário para rodar o jogo.

### 4. Perguntas para Reflexão

1. O que é orientação a objetos e como ela é aplicada em TypeScript?
2. Como você define uma classe em TypeScript?
3. Qual a função do método `constructor` em uma classe?
4. Como funcionam as instâncias de objetos em TypeScript?
5. Como você poderia modificar o jogo para adicionar dificuldade crescente?
6. O que é o encapsulamento e como ele foi usado nas classes do jogo?
7. Como funciona o método `move()` da classe **Snake**?
8. Como você faria para implementar colisões com as bordas do canvas?
9. Quais são as vantagens de utilizar tipagem estática em projetos como este?
10. Como você modularizaria este código utilizando **módulos** em TypeScript?

