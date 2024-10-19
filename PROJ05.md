# Projeto de Batalha Estilo Pokémon com TypeScript

Neste capítulo, vamos desenvolver um projeto prático de uma tela de batalha estilo **Pokémon**, utilizando **TypeScript**. O objetivo deste projeto é aplicar conceitos de orientação a objetos e tipagem estática para gerenciar a lógica de batalha, onde cada personagem (Pokémon) terá quatro golpes disponíveis. Este projeto vai ajudar os alunos a entenderem como organizar um sistema de batalha em turnos com TypeScript e HTML.

## 1. Estrutura Básica do Projeto

Organize o projeto com os seguintes arquivos:

- **index.html**: para a interface da batalha.
- **styles.css**: para estilizar a tela.
- **battle.ts**: onde o código TypeScript da batalha será escrito.
- **tsconfig.json**: configuração do TypeScript.

### 1.1 index.html

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Batalha Pokémon</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Batalha Pokémon</h1>
    <div id="battlefield">
        <div id="player-pokemon">
            <h2>Seu Pokémon</h2>
            <p id="player-name">Pikachu</p>
            <p id="player-hp">HP: 100</p>
            <div id="player-moves"></div>
        </div>

        <div id="enemy-pokemon">
            <h2>Pokémon Inimigo</h2>
            <p id="enemy-name">Charmander</p>
            <p id="enemy-hp">HP: 100</p>
        </div>
    </div>
    <script src="battle.js"></script>
</body>
</html>
```

### 1.2 styles.css

```css
body {
    text-align: center;
    font-family: Arial, sans-serif;
}

#battlefield {
    display: flex;
    justify-content: space-around;
    margin-top: 20px;
}

#player-pokemon, #enemy-pokemon {
    width: 40%;
    border: 1px solid #ccc;
    padding: 10px;
    background-color: #f9f9f9;
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

## 2. Classes e Lógica de Batalha

Agora, vamos criar classes que representarão os Pokémon, seus ataques, e a lógica de batalha.

### 2.1 Classe Move (Golpe)

A classe **Move** representará cada golpe que um Pokémon pode usar.

```typescript
class Move {
    name: string;
    damage: number;

    constructor(name: string, damage: number) {
        this.name = name;
        this.damage = damage;
    }

    use() {
        return this.damage;
    }
}
```

### 2.2 Classe Pokemon

A classe **Pokemon** armazenará informações sobre o Pokémon, como nome, HP (vida) e seus golpes.

```typescript
class Pokemon {
    name: string;
    hp: number;
    moves: Move[];

    constructor(name: string, hp: number, moves: Move[]) {
        this.name = name;
        this.hp = hp;
        this.moves = moves;
    }

    attack(move: Move, target: Pokemon) {
        target.hp -= move.use();
    }

    isFainted() {
        return this.hp <= 0;
    }
}
```

### 2.3 Classe Battle (Batalha)

A classe **Battle** controlará o fluxo do jogo, alternando entre os turnos dos jogadores e aplicando os golpes.

```typescript
class Battle {
    player: Pokemon;
    enemy: Pokemon;

    constructor(player: Pokemon, enemy: Pokemon) {
        this.player = player;
        this.enemy = enemy;
    }

    playerAttack(moveIndex: number) {
        const move = this.player.moves[moveIndex];
        this.player.attack(move, this.enemy);

        if (this.enemy.isFainted()) {
            alert(`${this.enemy.name} foi derrotado!`);
        } else {
            this.enemyTurn();
        }
    }

    enemyTurn() {
        const randomMove = Math.floor(Math.random() * this.enemy.moves.length);
        const move = this.enemy.moves[randomMove];
        this.enemy.attack(move, this.player);

        if (this.player.isFainted()) {
            alert(`${this.player.name} foi derrotado!`);
        }
    }
}
```

## 3. Inicializando a Batalha

Agora, vamos inicializar a batalha e permitir que o jogador escolha seus golpes.

```typescript
// Criando os golpes
const thunderbolt = new Move("Thunderbolt", 25);
const quickAttack = new Move("Quick Attack", 10);
const ironTail = new Move("Iron Tail", 15);
const electroBall = new Move("Electro Ball", 20);

const ember = new Move("Ember", 20);
const scratch = new Move("Scratch", 10)
const growl = new Move("Growl", 0)
const flamethrower = new Move("Flamethrower", 30)

// Criando os Pokémon
const pikachu = new Pokemon("Pikachu", 100, [thunderbolt, quickAttack, ironTail, electroBall]);
const charmander = new Pokemon("Charmander", 100, [ember, scratch, growl, flamethrower]);

// Inicializando a batalha
const battle = new Battle(pikachu, charmander);

// Renderizando golpes no DOM
const playerMovesDiv = document.getElementById("player-moves")!;
pikachu.moves.forEach((move, index) => {
    const moveButton = document.createElement("button");
    moveButton.textContent = move.name;
    moveButton.addEventListener("click", () => battle.playerAttack(index));
    playerMovesDiv.appendChild(moveButton);
});
```

## 4. Compilando o Código

Execute o compilador **tsc** para gerar o código JavaScript e visualizar o projeto no navegador.

```bash
tsc
```

## 5. Perguntas para Reflexão

1. Como o conceito de herança poderia ser aplicado no jogo da batalha?
2. Quais são as vantagens de usar classes para representar Pokémon e golpes?
3. Como você poderia modificar o jogo para suportar mais Pokémon?
4. Como o encapsulamento foi aplicado nas classes do projeto?
5. Qual o papel da tipagem estática no desenvolvimento deste jogo?
6. Como você controlaria a ordem de turnos em uma batalha entre múltiplos Pokémon?
7. Como garantir que o Pokémon derrotado não possa mais atacar?
8. Como você modularizaria o código utilizando importação e exportação de módulos?
9. Como funcionam as classes anônimas em TypeScript e como elas poderiam ser usadas neste contexto?
10. Como a implementação de eventos no DOM auxilia na interação do jogador com a aplicação?

