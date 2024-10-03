### Criando um Jogo de Pedra, Papel e Tesoura com HTML, CSS e TypeScript

Neste artigo, vamos aprender a criar um simples jogo de Pedra, Papel e Tesoura usando HTML, CSS e TypeScript. Este projeto é perfeito para iniciantes que desejam praticar suas habilidades de programação web e entender como integrar diferentes tecnologias. Vamos passar por cada parte do projeto, explicando o que estamos fazendo e por quê.

#### Estrutura do Projeto

Antes de começarmos a codificar, vamos definir a estrutura dos arquivos que precisamos para nosso projeto. Você precisará criar três arquivos:

1. **`index.html`**: O arquivo HTML que conterá a estrutura da nossa página web.
2. **`styles.css`**: O arquivo CSS que estilizará nossa página, tornando-a visualmente agradável.
3. **`script.ts`**: O arquivo TypeScript que conterá a lógica do jogo.

Vamos começar!

### 1. Criando o Arquivo `index.html`

O arquivo HTML é a base da nossa página. Aqui, vamos estruturar a interface do usuário. Abra um editor de texto e crie um arquivo chamado `index.html`. Cole o seguinte código:

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="styles.css">
    <title>Pedra, Papel e Tesoura</title>
</head>
<body>
    <h1>Pedra, Papel e Tesoura</h1>
    <div class="choices">
        <button id="rock">Pedra</button>
        <button id="paper">Papel</button>
        <button id="scissors">Tesoura</button>
    </div>
    <div id="result"></div>
    <script src="dist/script.js"></script>
</body>
</html>
```

**Explicação do Código:**
- **Estrutura da Página**: Usamos a tag `<head>` para incluir informações sobre a página, como o título e a folha de estilo (CSS).
- **Botões**: Criamos três botões para que o jogador escolha entre Pedra, Papel ou Tesoura.
- **Resultado**: Criamos uma área (`<div>`) onde o resultado do jogo será exibido.

### 2. Estilizando com `styles.css`

Agora, vamos adicionar um pouco de estilo à nossa página. Crie um arquivo chamado `styles.css` e adicione o seguinte código:

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

.choices {
    margin-bottom: 20px;
}

button {
    margin: 0 10px;
    padding: 10px 20px;
    font-size: 18px;
    cursor: pointer;
}

#result {
    font-size: 24px;
    margin-top: 20px;
}
```

**Explicação do Código:**
- **Estilização**: Usamos CSS para definir a fonte, alinhar os elementos e ajustar as margens e preenchimentos para que a página tenha uma aparência limpa e amigável.
- **Botões**: Estilizamos os botões para que sejam grandes o suficiente para serem facilmente clicados.

### 3. Implementando a Lógica do Jogo em `script.ts`

Agora vem a parte mais interessante: a lógica do jogo! Crie um arquivo chamado `script.ts` e adicione o seguinte código:

```typescript
// Definindo as opções do jogo
const choices = ['pedra', 'papel', 'tesoura'];

// Obtendo referências aos botões e ao elemento de resultado
const rockButton = document.getElementById('rock') as HTMLButtonElement;
const paperButton = document.getElementById('paper') as HTMLButtonElement;
const scissorsButton = document.getElementById('scissors') as HTMLButtonElement;
const resultDisplay = document.getElementById('result') as HTMLDivElement;

// Função para determinar o vencedor
function determineWinner(playerChoice: string, computerChoice: string): string {
    if (playerChoice === computerChoice) {
        return "Empate!";
    } else if (
        (playerChoice === 'pedra' && computerChoice === 'tesoura') ||
        (playerChoice === 'papel' && computerChoice === 'pedra') ||
        (playerChoice === 'tesoura' && computerChoice === 'papel')
    ) {
        return "Você ganhou!";
    } else {
        return "Você perdeu!";
    }
}

// Função para jogar
function playGame(playerChoice: string) {
    const computerChoice = choices[Math.floor(Math.random() * choices.length)];
    const result = determineWinner(playerChoice, computerChoice);
    resultDisplay.textContent = `Você escolheu ${playerChoice}. A máquina escolheu ${computerChoice}. ${result}`;
}

// Adicionando eventos de clique aos botões
rockButton.addEventListener('click', () => playGame('pedra'));
paperButton.addEventListener('click', () => playGame('papel'));
scissorsButton.addEventListener('click', () => playGame('tesoura'));
```

**Explicação do Código:**
- **Opções do Jogo**: Definimos as opções disponíveis (`pedra`, `papel` e `tesoura`) em um array.
- **Referências**: Pegamos referências aos botões e ao espaço onde exibiremos o resultado.
- **Função `determineWinner`**: Esta função compara a escolha do jogador com a escolha da máquina e determina o resultado. Se ambos escolherem a mesma opção, é um empate. Se o jogador ganhar, retorna "Você ganhou!", caso contrário, retorna "Você perdeu!".
- **Função `playGame`**: Esta função é chamada quando o jogador clica em um botão. Ela escolhe aleatoriamente a opção da máquina, chama a função `determineWinner` e atualiza o resultado na página.
- **Eventos de Clique**: Adicionamos um ouvinte de eventos a cada botão. Quando o jogador clica em um botão, a função `playGame` é chamada com a escolha correspondente.

### 4. Compilando o TypeScript

Para que seu código TypeScript funcione no navegador, precisamos compilar o arquivo `script.ts` em JavaScript. Você pode fazer isso usando o TypeScript Compiler (tsc). Se ainda não tiver o TypeScript instalado, instale-o com o seguinte comando:

```bash
npm install -g typescript
```

Em seguida, execute o seguinte comando para compilar o código:

```bash
tsc script.ts --outDir dist
```

Isso gerará um arquivo `script.js` dentro da pasta `dist`.

### Conclusão

Parabéns! Você acabou de criar um jogo simples de Pedra, Papel e Tesoura usando HTML, CSS e TypeScript. Este projeto é uma ótima maneira de praticar suas habilidades de programação web e entender como diferentes tecnologias funcionam juntas.

Você pode expandir este projeto adicionando mais recursos, como:
- Um placar para contar vitórias e derrotas.
- A possibilidade de jogar várias rodadas.
- Melhores animações ou transições.

Continue praticando e explorando novas ideias!

Claro! Aqui estão algumas perguntas que os iniciantes podem responder para refletir sobre o código criado e aprofundar sua compreensão do projeto:

### Perguntas para Reflexão

1. **Compreensão do Fluxo do Jogo:**
   - Como o fluxo do jogo se desenrola desde que o jogador faz uma escolha até que o resultado é exibido?
   - O que acontece quando o jogador clica em um dos botões?

2. **Lógica de Determinação do Vencedor:**
   - Como a função `determineWinner` decide se o jogador ganhou, perdeu ou empatou?
   - Que condições precisam ser atendidas para que cada resultado ocorra?

3. **Uso de Funções:**
   - Como as funções estão organizadas no código? Elas estão desempenhando suas responsabilidades de forma clara?
   - O que você poderia fazer para melhorar a clareza ou a eficiência do código?

4. **Manipulação do DOM:**
   - Como o código utiliza a manipulação do DOM para atualizar o conteúdo da página?
   - Que métodos foram utilizados para selecionar elementos e alterar seu conteúdo?

5. **Eventos de Clique:**
   - Como os eventos de clique são configurados para os botões? O que acontece quando um botão é clicado?
   - O que você poderia adicionar para melhorar a interatividade do jogo?

6. **Aleatoriedade:**
   - Como a escolha da máquina é feita de forma aleatória? Você entende a lógica por trás de `Math.random()` e `Math.floor()`?
   - Que outras maneiras você poderia implementar para gerar uma escolha aleatória?

7. **Reflexão Pessoal:**
    - O que você aprendeu ao criar este projeto?
    - Como você se sente em relação ao uso de HTML, CSS e TypeScript juntos? Existe alguma parte do processo que você achou desafiadora?

