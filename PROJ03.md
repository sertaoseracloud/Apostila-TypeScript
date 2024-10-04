### Construindo um Simulador de Semáforo com HTML, CSS e TypeScript

Neste projeto, vamos criar um simulador de semáforo utilizando **HTML**, **CSS** e **TypeScript**. Esse projeto simples é ideal para iniciantes e ajudará você a praticar conceitos importantes, como a manipulação do DOM, o uso de `enum` no TypeScript, e o funcionamento de temporizadores com `setInterval`. Ao final, você terá um semáforo funcional que alterna automaticamente entre as cores vermelho, amarelo e verde, simulando o comportamento de um sinal de trânsito real.

---

### O Que Vamos Aprender?

- Como organizar o código utilizando **enum** no TypeScript.
- Como manipular o DOM com JavaScript/TypeScript.
- Como criar ciclos de tempo com o método `setInterval`.
- A importância do CSS para controlar o estado visual de componentes interativos.

---

### 1. Estrutura do Projeto

O projeto será composto por três arquivos:

1. **HTML**: Estrutura do semáforo.
2. **CSS**: Estilização das luzes e da página.
3. **TypeScript**: Lógica para alternar as luzes automaticamente.

#### **1.1 Estrutura HTML (index.html)**

Vamos começar com o arquivo `index.html`. Ele define a estrutura da página e inclui as divs que representam as luzes do semáforo.

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulador de Semáforo</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Simulador de Semáforo</h1>
    <div class="traffic-light">
        <div class="light" id="red"></div>
        <div class="light" id="yellow"></div>
        <div class="light" id="green"></div>
    </div>
    <script src="dist/script.js"></script>
</body>
</html>
```

**Explicação:**
- O **h1** exibe o título da página.
- O container `traffic-light` possui três divs, cada uma representando uma cor do semáforo: vermelho, amarelo e verde.
- O arquivo JavaScript compilado a partir do TypeScript será referenciado no final da página.

---

### 2. Estilizando o Semáforo com CSS

O próximo passo é criar o estilo visual do semáforo. No arquivo `styles.css`, vamos definir o layout e as cores.

```css
body {
    font-family: Arial, sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    background-color: #f4f4f4;
}

h1 {
    margin-bottom: 20px;
}

.traffic-light {
    display: flex;
    flex-direction: column;
    width: 80px;
    background-color: black;
    padding: 20px;
    border-radius: 10px;
}

.light {
    width: 50px;
    height: 50px;
    margin: 10px 0;
    border-radius: 50%;
    background-color: grey;
    opacity: 0.3;
}

#red {
    background-color: red;
}

#yellow {
    background-color: yellow;
}

#green {
    background-color: green;
}

.active {
    opacity: 1;
}
```

**Explicação:**
- Definimos as luzes como círculos (`border-radius: 50%`), que ficam cinzas por padrão com opacidade reduzida (`opacity: 0.3`).
- Quando uma luz está ativa (classe `active`), a opacidade aumenta, destacando a cor correspondente.

---

### 3. Criando a Lógica com TypeScript

Agora, vamos escrever a lógica do semáforo em **TypeScript**. O ciclo do semáforo alternará automaticamente entre vermelho, amarelo e verde.

```typescript
// Enum que define os estados do semáforo
enum TrafficLightState {
    Red = 'red',
    Yellow = 'yellow',
    Green = 'green'
}

// Obtendo as divs de luz
const redLight = document.getElementById('red') as HTMLDivElement;
const yellowLight = document.getElementById('yellow') as HTMLDivElement;
const greenLight = document.getElementById('green') as HTMLDivElement;

// Variável para armazenar o estado atual do semáforo
let currentState: TrafficLightState = TrafficLightState.Red;

// Função para alternar entre as cores do semáforo
function changeLight() {
    // Remover a classe 'active' de todas as luzes
    redLight.classList.remove('active');
    yellowLight.classList.remove('active');
    greenLight.classList.remove('active');

    // Acender a luz correta com base no estado atual
    switch (currentState) {
        case TrafficLightState.Red:
            redLight.classList.add('active');
            currentState = TrafficLightState.Green;
            break;
        case TrafficLightState.Yellow:
            yellowLight.classList.add('active');
            currentState = TrafficLightState.Red;
            break;
        case TrafficLightState.Green:
            greenLight.classList.add('active');
            currentState = TrafficLightState.Yellow;
            break;
    }
}

// Função que inicia o ciclo do semáforo
function startTrafficLight() {
    setInterval(changeLight, 3000); // Alterna as cores a cada 3 segundos
}

// Iniciar o ciclo do semáforo assim que a página carregar
window.onload = startTrafficLight;
```

**Explicação do Código:**

- **Enum**: Criamos um `enum` chamado `TrafficLightState` para representar os três estados do semáforo: **vermelho**, **amarelo** e **verde**.
- **Função `changeLight`**: Essa função alterna entre as cores do semáforo. Removemos a classe `active` de todas as luzes e a adicionamos à luz correspondente ao estado atual.
- **Função `startTrafficLight`**: Utilizamos `setInterval` para chamar a função `changeLight` a cada 3 segundos, simulando o ciclo do semáforo.
- **Inicialização**: Quando a página carrega, a função `startTrafficLight` é chamada, e o semáforo começa a alternar automaticamente entre as cores.

---

### 4. Compilando o TypeScript

Para que o TypeScript funcione no navegador, precisamos compilá-lo em JavaScript. Veja como fazer isso:

1. Instale o TypeScript, se ainda não tiver:
    ```bash
    npm install -g typescript
    ```

2. Compile o arquivo `script.ts` para JavaScript:
    ```bash
    tsc script.ts --outDir dist
    ```

Isso gera o arquivo `script.js` na pasta `dist`, que será usado no HTML.

---

### Reflexão e Perguntas para Iniciantes

Agora que você construiu um simulador de semáforo, é hora de refletir sobre o que aprendeu. Aqui estão algumas perguntas para guiar sua reflexão:

1. **Como o `enum` ajuda a organizar os estados do semáforo?**
  
2. **Por que o `setInterval` é útil neste projeto?**

3. **O que acontece se mudarmos o tempo do `setInterval`?**

4. **Como o uso das classes `active` e do CSS ajuda a alterar a aparência das luzes sem modificar o HTML diretamente?**

5. **Como você poderia expandir este projeto?**

---

### Conclusão

Construímos um simulador de semáforo usando HTML, CSS e TypeScript. Vimos como utilizar `enum` para organizar os estados do semáforo, `setInterval` para alternar entre eles, e o poder do CSS para controlar a aparência das luzes. Este projeto é uma ótima forma de começar a entender como componentes interativos funcionam na web.

Agora, desafie-se a modificar o projeto e adicionar suas próprias funcionalidades!