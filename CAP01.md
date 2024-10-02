# Capítulo 1: Introdução ao TypeScript e Configuração do Ambiente

## 1.1 Visão Geral do TypeScript e Suas Vantagens

O **TypeScript** é uma linguagem de programação criada pela Microsoft, que estende o JavaScript adicionando **tipagem estática**. Enquanto o JavaScript permite a execução de código sem verificar erros de tipo, o TypeScript detecta esses erros **antes da execução**, na fase de compilação. Isso traz benefícios como maior **segurança**, **previsibilidade** e **facilidade de manutenção** no desenvolvimento de software.

- **Para assistir**: Aqui vai alguns videos para complementar o conhecimento:

[TypeScript em 15 minutos direto ao ponto! - LuizTools](https://www.youtube.com/watch?v=g0hkeyMb45U)

### Vantagens do TypeScript:
- **Tipagem Estática**: Erros de tipos são detectados antes da execução.
- **Autocompletar aprimorado**: Editores como o Visual Studio Code oferecem sugestões de código mais inteligentes e precisas.
- **Organização em projetos grandes**: Em sistemas complexos, a tipagem facilita a compreensão e a manutenção do código.
- **Compatibilidade com JavaScript**: Todo código JavaScript válido também é válido em TypeScript.
- **Adoção crescente**: Grandes empresas como Google e Microsoft utilizam TypeScript em projetos complexos.

## 1.2 Configuração do Ambiente de Desenvolvimento

### Passo 1: Instalando o Node.js
Antes de usar o TypeScript, é necessário instalar o **Node.js**, que serve como ambiente de execução para JavaScript e fornece o **npm** (Node Package Manager), utilizado para gerenciar pacotes, incluindo o TypeScript.

1. Acesse o [site oficial do Node.js](https://nodejs.org/).
2. Baixe a versão recomendada para o seu sistema operacional.
3. Siga as instruções de instalação.

### Passo 2: Instalando o TypeScript
Com o Node.js instalado, você pode instalar o TypeScript globalmente usando o npm:

```bash
npm install -g typescript
```

Esse comando instala o compilador TypeScript, permitindo o uso do comando `tsc` para compilar seus arquivos.

### Passo 3: Verificando a Instalação
Para verificar se o TypeScript foi instalado corretamente, execute o comando:

```bash
tsc --version
```

Isso exibirá a versão do TypeScript instalada.

## 1.3 Compilador TypeScript (tsc) e Configuração de Projetos

O **compilador TypeScript**, `tsc`, converte arquivos `.ts` (TypeScript) em `.js` (JavaScript), que podem ser executados em qualquer ambiente que suporte JavaScript.

### Criando um Projeto TypeScript

1. **Crie uma pasta para o projeto:**
   ```bash
   mkdir meu-projeto-ts
   cd meu-projeto-ts
   ```

2. **Inicie um projeto TypeScript:**
   ```bash
   tsc --init
   ```
   Isso gerará um arquivo `tsconfig.json`, que define as configurações de compilação do TypeScript, como o alvo da versão de JavaScript e o diretório de saída dos arquivos compilados.

### Configurações Comuns no `tsconfig.json`:
- **"target"**: Define para qual versão do JavaScript o TypeScript será compilado. Exemplo: `"target": "ES6"`.
- **"outDir"**: Define a pasta onde os arquivos compilados serão salvos. Exemplo: `"outDir": "./dist"`.
- **"strict"**: Ativa verificações estritas de tipos. Exemplo: `"strict": true`.

### Exemplo de Projeto: "Hello, World!"

Vamos criar um exemplo básico para demonstrar como configurar e executar um código TypeScript.

#### Passo 1: Criar o Arquivo TypeScript
Na pasta do projeto, crie um arquivo chamado `index.ts`:

Adicione o seguinte código ao arquivo `index.ts`:

```typescript
const message: string = "Hello, World!";
console.log(message);
```

#### Passo 2: Compilar o Arquivo
Para compilar o arquivo TypeScript para JavaScript, execute o comando:

```bash
tsc
```

Isso vai gerar um arquivo `index.js`, que contém o código JavaScript compilado.

#### Passo 3: Executar o Arquivo JavaScript
Agora, execute o arquivo compilado com o Node.js:

```bash
node index.js
```

Isso vai exibir o seguinte no console:

```
Hello, World!
```

## 1.4 Perguntas para Reflexão

Aqui estão algumas perguntas que podem ajudar a aprofundar a compreensão sobre **TypeScript** e **configuração de ambiente**:

1. **O que é TypeScript e quais são suas principais vantagens em relação ao JavaScript?**
2. **Explique o que é a tipagem estática no TypeScript e como ela melhora a qualidade do código.**
3. **Quais os passos necessários para configurar um ambiente de desenvolvimento TypeScript?**
4. **O que é o Node.js e qual é a sua importância na configuração do TypeScript?**
5. **Qual o comando utilizado para instalar o TypeScript globalmente e qual sua função?**
6. **Explique o que faz o compilador `tsc` e como ele converte arquivos TypeScript em JavaScript.**
7. **Para que serve o arquivo `tsconfig.json` em um projeto TypeScript e quais são suas principais configurações?**
8. **Qual é a função da propriedade `"strict"` no arquivo `tsconfig.json`?**
9. **Como você configuraria um projeto TypeScript para compilar os arquivos em uma pasta específica?**
10. **Quais problemas o TypeScript pode ajudar a detectar durante a fase de desenvolvimento que o JavaScript não detectaria?**
