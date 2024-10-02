# Capítulo 2: Sintaxe e Tipagem em TypeScript

## 2.1 Introdução à Sintaxe do TypeScript

TypeScript é uma linguagem que expande o JavaScript adicionando tipagem estática. Essa tipagem permite aos desenvolvedores especificar os tipos de variáveis, funções e objetos, resultando em código mais seguro e fácil de manter. A sintaxe do TypeScript é semelhante à do JavaScript, mas com recursos adicionais que ajudam a trabalhar com tipos de dados.

- **Para assistir**: Aqui vai alguns videos para complementar o conhecimento:

[Módulo B - Variáveis Primitivas - Cursos em Video](https://www.youtube.com/watch?v=Vbabsye7mWo&list=PLHz_AreHm4dlsK3Nr9GVvXCbpQyHQl1o1&index=9)
[Type x Interface - Mateus Durães](https://www.youtube.com/watch?v=s9qgTlpYDuA)

## 2.2 Tipos Básicos

TypeScript oferece vários tipos básicos que podem ser utilizados para definir variáveis e funções:

### 2.2.1 Boolean
Representa um valor verdadeiro (`true`) ou falso (`false`).

```typescript
let isCompleted: boolean = true;
```

### 2.2.2 Number
Representa números, incluindo inteiros e números de ponto flutuante.

```typescript
let age: number = 30;
let temperature: number = 36.6;
```

### 2.2.3 String
Representa sequências de caracteres.

```typescript
let name: string = "Alice";
```

### 2.2.4 Array
Permite armazenar múltiplos valores do mesmo tipo.

```typescript
let scores: number[] = [90, 80, 70];
let fruits: Array<string> = ["apple", "banana", "cherry"];
```

### 2.2.5 Tuple
Permite armazenar um conjunto fixo de elementos de diferentes tipos.

```typescript
let user: [string, number] = ["Alice", 30];
```

### 2.2.6 Enum
Um tipo que permite definir um conjunto de constantes nomeadas.

```typescript
enum Color {
    Red,
    Green,
    Blue
}

let favoriteColor: Color = Color.Green;
```

## 2.3 Tipos Avançados

Além dos tipos básicos, TypeScript também oferece tipos avançados:

### 2.3.1 Any
Permite definir uma variável que pode ter qualquer tipo. Deve ser usado com cautela, pois elimina a verificação de tipos.

```typescript
let variable: any = "Hello";
variable = 10; // Válido, pois é 'any'
```

### 2.3.2 Void
Usado em funções que não retornam um valor.

```typescript
function logMessage(message: string): void {
    console.log(message);
}
```

### 2.3.3 Never
Usado para funções que nunca retornam (por exemplo, lançam uma exceção ou entram em um loop infinito).

```typescript
function throwError(message: string): never {
    throw new Error(message);
}
```

### 2.3.4 Unknown
Semelhante ao tipo `any`, mas mais seguro, pois requer verificação de tipo antes de uso.

```typescript
let value: unknown = "Hello";
// value.toUpperCase(); // Erro, pois não se sabe se é uma string
if (typeof value === "string") {
    console.log(value.toUpperCase()); // Agora é seguro
}
```

## 2.4 Union e Intersection Types

### 2.4.1 Union Types
Permitem que uma variável aceite múltiplos tipos.

```typescript
let id: string | number;
id = "123"; // Válido
id = 123;   // Válido
```

### 2.4.2 Intersection Types
Permitem combinar múltiplos tipos em um único tipo.

```typescript
interface Person {
    name: string;
}

interface Employee {
    employeeId: number;
}

type Worker = Person & Employee;

const worker: Worker = {
    name: "Alice",
    employeeId: 1
};
```

## 2.5 Type Assertions e Type Inference

### 2.5.1 Type Assertions
Permitem que você "afirme" o tipo de uma variável, informando ao compilador que você sabe melhor.

```typescript
let someValue: unknown = "Hello, World!";
let strLength: number = (someValue as string).length; // Usando 'as'
```

### 2.5.2 Type Inference
O TypeScript é capaz de inferir o tipo de uma variável automaticamente com base no valor atribuído.

```typescript
let message = "Hello, World!"; // TypeScript infere que 'message' é do tipo string
```

## 2.6 Exemplo Final: Aplicação Prática

Vamos criar um pequeno exemplo que demonstra a utilização de diferentes tipos em uma função que calcula o IMC (Índice de Massa Corporal).

```typescript
function calculateBMI(weight: number, height: number): string {
    const bmi: number = weight / (height * height);
    
    if (bmi < 18.5) {
        return "Abaixo do peso";
    } else if (bmi >= 18.5 && bmi < 24.9) {
        return "Peso normal";
    } else if (bmi >= 25 && bmi < 29.9) {
        return "Sobrepeso";
    } else {
        return "Obesidade";
    }
}

const weight: number = 70; // em kg
const height: number = 1.75; // em metros
const result: string = calculateBMI(weight, height);
console.log(`Seu IMC é: ${result}`);
```

Neste exemplo, a função `calculateBMI` utiliza tipos básicos (`number` e `string`) e retorna uma string informando a categoria do IMC calculado.

## 2.7 Perguntas para Reflexão

Aqui estão 10 perguntas que podem ajudar a aprofundar a compreensão sobre a sintaxe e tipagem em TypeScript:

1. **Quais são os principais tipos básicos disponíveis no TypeScript?**
2. **Como você utilizaria o tipo `enum` em um projeto real?**
3. **Qual a diferença entre os tipos `any` e `unknown`?**
4. **Como as tuples diferem dos arrays em TypeScript?**
5. **Quando você usaria `void` em uma função?**
6. **O que é uma Union Type e como ela pode ser útil?**
7. **Como você utilizaria a Intersection Type para combinar propriedades de diferentes interfaces?**
8. **Qual é a importância da inferência de tipo no TypeScript?**
9. **Como você utilizaria type assertions na prática?**
10. **De que forma a tipagem estática do TypeScript pode melhorar a qualidade do código em comparação com o JavaScript?**
