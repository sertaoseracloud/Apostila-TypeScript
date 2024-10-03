### Criando um Jogo Nexus com Cores

Neste artigo, vamos aprender a criar um simples jogo chamado Nexus, onde os jogadores devem clicar nas cores corretas em uma sequência aleatória. O jogo usa HTML, CSS e TypeScript, e é uma excelente maneira para iniciantes praticarem suas habilidades de programação. Vamos detalhar cada parte do projeto, explicando o que cada seção do código faz.

#### Estrutura do Projeto

Para este projeto, você precisará criar três arquivos:

1. **`index.html`**: O arquivo HTML que conterá a estrutura da página web.
2. **`styles.css`**: O arquivo CSS que estilizará a aparência da página.
3. **`script.ts`**: O arquivo TypeScript que conterá a lógica do jogo.

### 1. Criando o Arquivo `index.html`

O arquivo HTML é a base da nossa página. Crie um arquivo chamado `index.html` e adicione o seguinte código:

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="styles.css">
    <title>Jogo Nexus</title>
</head>
<body>
    <h1>Jogo Nexus</h1>
    <div class="container">
        <div class="color" id="red"></div>
        <div class="color" id="blue"></div>
        <div class="color" id="yellow"></div>
        <div class="color" id="green"></div>
    </div>
    <button id="start">Iniciar Jogo</button>
    <div id="message"></div>
    <script src="dist/script.js"></script>
</body>
</html>
```

**Explicação do Código:**
- **Estrutura da Página**: A seção `<head>` inclui informações como título e folha de estilo (CSS).
- **Cores**: Criamos quatro divs para representar as cores vermelho, azul, amarelo e verde.
- **Botão de Início**: Um botão para iniciar o jogo.
- **Área de Mensagem**: Uma div onde exibiremos mensagens ao jogador.

### 2. Estilizando com `styles.css`

Agora, vamos adicionar estilo à nossa página. Crie um arquivo chamado `styles.css` e adicione o seguinte código:

```css
body {
    font-family: Arial, sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 0;
    padding: 20px;
}

h1 {
    margin-bottom: 20px;
}

.container {
    display: flex;
    margin-bottom: 20px;
}

.color {
    width: 100px;
    height: 100px;
    margin: 10px;
    opacity: 0.5;
    cursor: pointer;
    transition: opacity 0.3s;
}

#red {
    background-color: red;
}

#blue {
    background-color: blue;
}

#yellow {
    background-color: yellow;
}

#green {
    background-color: green;
}

.color:hover {
    opacity: 1;
}

button {
    padding: 10px 20px;
    font-size: 18px;
    cursor: pointer;
}

#message {
    font-size: 24px;
    margin-top: 20px;
}
```

**Explicação do Código:**
- **Estilização**: Definimos a fonte, alinhamos os elementos e ajustamos as dimensões e cores dos quadrados que representam as cores do jogo.
- **Interatividade**: Adicionamos um efeito de opacidade nos quadrados para que fiquem mais visíveis ao passar o mouse.

### 3. Implementando a Lógica do Jogo em `script.ts`

Agora, vamos implementar a lógica do jogo. Crie um arquivo chamado `script.ts` e adicione o seguinte código:

```typescript
// Definindo as cores
const colors = ['red', 'blue', 'yellow', 'green'];

// Obtendo referências aos elementos da página
const startButton = document.getElementById('start') as HTMLButtonElement;
const messageDisplay = document.getElementById('message') as HTMLDivElement;
const colorDivs = colors.map(color => document.getElementById(color) as HTMLDivElement);

// Variáveis do jogo
let sequence: string[] = [];
let userInput: string[] = [];
let level: number = 0;

// Função para gerar a sequência aleatória
function generateSequence() {
    const randomColor = colors[Math.floor(Math.random() * colors.length)];
    sequence.push(randomColor);
    level++;
    displaySequence();
}

// Função para exibir a sequência para o jogador
function displaySequence() {
    messageDisplay.textContent = `Nível ${level}`;
    let index = 0;
    const interval = setInterval(() => {
        if (index < sequence.length) {
            highlightColor(sequence[index]);
            index++;
        } else {
            clearInterval(interval);
            enableUserInput();
        }
    }, 1000);
}

// Função para destacar a cor
function highlightColor(color: string) {
    const colorDiv = document.getElementById(color) as HTMLDivElement;
    colorDiv.style.opacity = '1';
    setTimeout(() => {
        colorDiv.style.opacity = '0.5';
    }, 500);
}

// Função para habilitar a entrada do usuário
function enableUserInput() {
    colorDivs.forEach(div => {
        div.addEventListener('click', handleUserClick);
    });
}

// Função para lidar com a entrada do usuário
function handleUserClick(event: MouseEvent) {
    const color = (event.target as HTMLDivElement).id;
    userInput.push(color);
    checkInput();
}

// Função para verificar a entrada do usuário
function checkInput() {
    const lastIndex = userInput.length - 1;
    if (userInput[lastIndex] !== sequence[lastIndex]) {
        messageDisplay.textContent = 'Você perdeu! Tente novamente.';
        resetGame();
    } else if (userInput.length === sequence.length) {
        messageDisplay.textContent = 'Correto! Proximo nível...';
        userInput = [];
        setTimeout(generateSequence, 1000);
    }
}

// Função para reiniciar o jogo
function resetGame() {
    sequence = [];
    userInput = [];
    level = 0;
    messageDisplay.textContent = 'Clique em "Iniciar Jogo" para começar.';
}

// Adicionando evento de clique ao botão de início
startButton.addEventListener('click', () => {
    resetGame();
    generateSequence();
});
```

**Explicação do Código:**
- **Cores**: Definimos um array com as cores do jogo.
- **Referências**: Obtemos referências ao botão de início e às divs de cor.
- **Sequência e Entrada do Usuário**: Mantemos o controle da sequência de cores e da entrada do usuário.
- **Gerar Sequência**: A função `generateSequence` adiciona uma cor aleatória à sequência e aumenta o nível.
- **Exibir Sequência**: A função `displaySequence` mostra a sequência para o jogador destacando cada cor em intervalos.
- **Destacar Cor**: A função `highlightColor` altera a opacidade da cor ao ser destacada.
- **Entrada do Usuário**: A função `handleUserClick` registra a entrada do usuário e verifica se está correta.
- **Verificar Entrada**: A função `checkInput` compara a entrada do usuário com a sequência. Se estiver correta, gera uma nova sequência; se não, exibe uma mensagem de erro.
- **Reiniciar o Jogo**: A função `resetGame` reinicia o jogo.

### 4. Compilando o TypeScript

Para que seu código TypeScript funcione no navegador, precisamos compilar o arquivo `script.ts` em JavaScript. Se você ainda não tem o TypeScript instalado, instale-o usando o seguinte comando:

```bash
npm install -g typescript
```

Em seguida, execute o seguinte comando para compilar o código:

```bash
tsc script.ts --outDir dist
```

Isso gerará um arquivo `script.js` dentro da pasta `dist`.

### Conclusão

Parabéns! Você criou um jogo Nexus simples onde os jogadores devem seguir uma sequência de cores. Este projeto é uma ótima maneira de praticar suas habilidades de programação web e entender como HTML, CSS e TypeScript funcionam juntos.

### Perguntas para Reflexão

1. **Compreensão do Fluxo do Jogo:**
   - Como o fluxo do jogo se desenrola desde o início até a verificação da entrada do usuário?

2. **Lógica da Sequência:**
   - Como a sequência de cores é gerada e exibida ao jogador?

3. **Uso de Funções:**
   - As funções estão organizadas de forma clara? O que você faria diferente?

4. **Manipulação do DOM:**
   - Como o código utiliza a manipulação do DOM para atualizar a interface?

5. **Interatividade:**
   - Como os eventos de clique são configurados e gerenciados?

6. **Debugging:**
   - Você encontrou erros? Como você os resolveu?

7. **Extensibilidade:**
   - Que recursos adicionais você gostaria de adicionar ao jogo? Como você abordaria isso?
