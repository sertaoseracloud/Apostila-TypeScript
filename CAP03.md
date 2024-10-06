# Capítulo 3: Funções e Classes em TypeScript

## 3.1 Funções em TypeScript

As funções são um dos elementos mais importantes em qualquer linguagem de programação. Em TypeScript, as funções podem ser tipadas, o que aumenta a segurança do código e facilita a manutenção.

### 3.1.1 Tipos de Parâmetros

Você pode definir o tipo de cada parâmetro de uma função, o que garante que os valores recebidos serão do tipo correto.

```typescript
function greet(name: string): string {
    return `Hello, ${name}!`;
}

console.log(greet("Alice"));
```

Aqui, o parâmetro `name` é do tipo `string`, e a função retorna uma `string`.

### 3.1.2 Valores Padrão

Em TypeScript, é possível definir valores padrão para os parâmetros de uma função. Se o argumento não for passado, o valor padrão será usado.

```typescript
function greet(name: string = "Guest"): string {
    return `Hello, ${name}!`;
}

console.log(greet()); // Hello, Guest!
console.log(greet("Bob")); // Hello, Bob!
```

### 3.1.3 Parâmetros Opcionais

Os parâmetros opcionais são aqueles que podem ou não ser fornecidos ao chamar a função. Para indicar que um parâmetro é opcional, basta adicionar o símbolo `?` após o nome do parâmetro.

```typescript
function greet(name?: string): string {
    return `Hello, ${name ? name : "Guest"}!`;
}

console.log(greet()); // Hello, Guest!
console.log(greet("Alice")); // Hello, Alice!
```

Neste exemplo, o parâmetro `name` é opcional, e a função lida com ambos os casos, quando o parâmetro é fornecido e quando não é.

### 3.1.4 Funções Anônimas

Funções anônimas são funções que não possuem um nome. Elas são frequentemente usadas como argumentos para outras funções.

```typescript
const greet = function(name: string): string {
    return `Hello, ${name}!`;
};

console.log(greet("Alice"));
```

### 3.1.5 Arrow Functions

As **arrow functions** oferecem uma sintaxe mais concisa para escrever funções, além de manter o valor correto de `this` no contexto onde são criadas.

```typescript
const greet = (name: string): string => `Hello, ${name}!`;

console.log(greet("Alice"));
```

As arrow functions simplificam a sintaxe ao eliminar a necessidade da palavra-chave `function` e do `return`, no caso de funções de linha única.

## 3.2 Classes e Interfaces

### 3.2.1 Definição de Classes

As classes são blocos fundamentais para a criação de objetos em TypeScript. Elas permitem agrupar dados e comportamentos em uma estrutura unificada.

```typescript
class Person {
    name: string;
    age: number;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }

    greet(): string {
        return `Hello, my name is ${this.name} and I am ${this.age} years old.`;
    }
}

const person = new Person("Alice", 30);
console.log(person.greet());
```

### 3.2.2 Interfaces

As interfaces definem contratos para objetos e classes, descrevendo como os objetos devem se comportar.

```typescript
interface Animal {
    name: string;
    sound(): string;
}

class Dog implements Animal {
    name: string;

    constructor(name: string) {
        this.name = name;
    }

    sound(): string {
        return "Woof!";
    }
}

const dog = new Dog("Rex");
console.log(dog.sound());
```

Aqui, a classe `Dog` implementa a interface `Animal`, garantindo que a classe siga o contrato definido pela interface.

### 3.2.3 Herança

A **herança** permite que uma classe herde propriedades e métodos de outra classe, promovendo reutilização de código.

```typescript
class Animal {
    name: string;

    constructor(name: string) {
        this.name = name;
    }

    move(): string {
        return `${this.name} is moving.`;
    }
}

class Bird extends Animal {
    fly(): string {
        return `${this.name} is flying.`;
    }
}

const bird = new Bird("Parrot");
console.log(bird.move()); // Parrot is moving.
console.log(bird.fly());  // Parrot is flying.
```

### 3.2.4 Polimorfismo

O **polimorfismo** permite que objetos de diferentes classes herdem o mesmo comportamento, mas possam ter implementações específicas.

```typescript
class Animal {
    sound(): string {
        return "Some sound";
    }
}

class Dog extends Animal {
    sound(): string {
        return "Woof!";
    }
}

class Cat extends Animal {
    sound(): string {
        return "Meow!";
    }
}

function makeSound(animal: Animal) {
    console.log(animal.sound());
}

makeSound(new Dog()); // Woof!
makeSound(new Cat()); // Meow!
```

Aqui, diferentes subclasses (`Dog` e `Cat`) implementam o método `sound` de maneira distinta, mas ambas podem ser tratadas como um `Animal`.

### 3.2.5 Encapsulamento

O **encapsulamento** permite proteger os dados internos de uma classe, controlando o acesso a eles através de métodos específicos. Isso é feito usando os modificadores `private` e `protected`.

```typescript
class Person {
    private name: string;

    constructor(name: string) {
        this.name = name;
    }

    getName(): string {
        return this.name;
    }
}

const person = new Person("Alice");
console.log(person.getName()); // Alice
// console.log(person.name); // Erro: Propriedade privada
```

## 3.3 Exemplo Final: Sistema de Funcionários

Vamos construir um exemplo que combina funções, classes e interfaces para simular um sistema de gerenciamento de funcionários.

```typescript
interface Employee {
    id: number;
    name: string;
    getSalary(): number;
}

class FullTimeEmployee implements Employee {
    id: number;
    name: string;
    annualSalary: number;

    constructor(id: number, name: string, annualSalary: number) {
        this.id = id;
        this.name = name;
        this.annualSalary = annualSalary;
    }

    getSalary(): number {
        return this.annualSalary;
    }
}

class PartTimeEmployee implements Employee {
    id: number;
    name: string;
    hourlyRate: number;
    hoursWorked: number;

    constructor(id: number, name: string, hourlyRate: number, hoursWorked: number) {
        this.id = id;
        this.name = name;
        this.hourlyRate = hourlyRate;
        this.hoursWorked = hoursWorked;
    }

    getSalary(): number {
        return this.hourlyRate * this.hoursWorked;
    }
}

function printEmployeeSalary(employee: Employee): void {
    console.log(`${employee.name} receives a salary of $${employee.getSalary()}`);
}

const fullTimeEmployee = new FullTimeEmployee(1, "Alice", 60000);
const partTimeEmployee = new PartTimeEmployee(2, "Bob", 20, 1000);

printEmployeeSalary(fullTimeEmployee); // Alice receives a salary of $60000
printEmployeeSalary(partTimeEmployee); // Bob receives a salary of $20000
```

## 3.4 Perguntas para Reflexão

1. **O que são parâmetros opcionais em funções?**
2. **Qual é a principal diferença entre uma função anônima e uma arrow function?**
3. **Como você define uma classe em TypeScript?**
4. **O que é uma interface e como ela é usada em TypeScript?**
5. **O que é herança e como ela funciona em TypeScript?**
6. **Explique o conceito de polimorfismo com um exemplo.**
7. **Qual é a função do encapsulamento em classes?**
8. **Como você cria uma função com um valor padrão para um parâmetro?**
9. **O que acontece se você não passar um parâmetro opcional em uma função?**
10. **Quais são os benefícios de utilizar interfaces ao invés de classes?**
