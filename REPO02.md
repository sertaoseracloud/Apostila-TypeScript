# Reposição 2: Criando uma API com Node.js, Prisma e SQLite Usando Express

Neste tutorial, você aprenderá a criar uma API utilizando **Node.js**, **Express** e **Prisma**, com o banco de dados **SQLite**. Express simplifica a criação de rotas e tratamento de requisições, tornando o desenvolvimento mais ágil e organizado.

#### 1. Configuração Inicial

Primeiro, crie um novo projeto Node.js e instale as dependências necessárias:

```bash
npm init -y
npm install express 
npm install typescript prisma ts-node @types/node @types/express --save-dev
```

Depois, inicialize o TypeScript criando o `tsconfig.json`:

```bash
npx tsc --init
```

Em seguida, configure o Prisma e o SQLite criando o arquivo `prisma/schema.prisma`:
```bash
npx prisma init --datasource-provider sqlite
```

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "sqlite"
  url      = "file:./dev.db"
}

model User {
  id        Int      @id @default(autoincrement())
  name      String
  email     String   @unique
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

Agora, execute os seguintes comandos para criar o banco de dados e gerar o cliente Prisma:

```bash
npx prisma migrate dev --name init
```

#### 2. Criando a API com Express

Agora que o Prisma e SQLite estão configurados, podemos começar a trabalhar na API com Express.

Crie o arquivo `src/server.ts` com o seguinte código:

```ts
import express from 'express';
import { PrismaClient } from '@prisma/client';

const app = express();
const prisma = new PrismaClient();

app.use(express.json());

// Rota para obter todos os usuários
app.get('/users', async (req, res) => {
  const users = await prisma.user.findMany();
  res.status(200).json(users);
});

// Rota para criar um novo usuário
app.post('/users', async (req, res) => {
  const { name, email } = req.body;
  try {
    const newUser = await prisma.user.create({
      data: { name, email },
    });
    res.status(201).json(newUser);
  } catch (error) {
    res.status(400).json({ error: 'Erro ao criar usuário' });
  }
});

// Rota para atualizar um usuário
app.put('/users/:id', async (req, res) => {
  const { id } = req.params;
  const { name, email } = req.body;
  try {
    const updatedUser = await prisma.user.update({
      where: { id: Number(id) },
      data: { name, email },
    });
    res.status(200).json(updatedUser);
  } catch (error) {
    res.status(404).json({ error: 'Usuário não encontrado' });
  }
});

// Rota para deletar um usuário
app.delete('/users/:id', async (req, res) => {
  const { id } = req.params;
  try {
    await prisma.user.delete({ where: { id: Number(id) } });
    res.status(204).send();
  } catch (error) {
    res.status(404).json({ error: 'Usuário não encontrado' });
  }
});

// Inicializa o servidor
app.listen(3000, () => {
  console.log('Servidor rodando em http://localhost:3000');
});
```

Neste exemplo, usamos o **Express** para lidar com rotas e requisições. O middleware `express.json()` facilita o tratamento de dados no formato JSON. Implementamos as seguintes rotas:
- `GET /users`: Retorna todos os usuários.
- `POST /users`: Cria um novo usuário.
- `PUT /users/:id`: Atualiza um usuário existente.
- `DELETE /users/:id`: Deleta um usuário específico.

#### 3. Rodando o Projeto

Adicione o seguinte script no `package.json` para rodar o servidor:

```json
{
  "scripts": {
    "start": "ts-node src/server.ts"
  }
}
```

Agora, você pode iniciar o servidor com o comando:

```bash
npm start
```

Isso iniciará a API no endereço `http://localhost:3000`, pronta para receber requisições.

### Conclusão

Criar uma API com Node.js e Express é uma abordagem simples e eficiente para lidar com rotas e requisições HTTP. Usar o Prisma com SQLite torna a interação com o banco de dados mais fácil, e a estrutura modular permite que a API seja facilmente expandida com novos recursos.

---

### Perguntas para Entrevista

1. **Qual a função do `express.json()` neste projeto?**  

2. **Como o Prisma se conecta ao banco de dados SQLite neste projeto?**  

3. **Explique o que acontece quando uma requisição POST é feita para `/users`.**  

4. **Quais são as vantagens de usar o Express em comparação com o uso apenas do módulo HTTP nativo do Node.js?**  

5. **O que acontece se tentarmos atualizar um usuário inexistente na rota `PUT /users/:id`?**  

6. **Como você expandiria essa API para incluir autenticação de usuários?**  

7. **Quais são os benefícios de usar TypeScript neste projeto?**  

8. **Se você quisesse adicionar validações adicionais nos campos `name` e `email`, onde você faria isso no código?**  

9. **Por que optamos por usar SQLite como banco de dados nesse projeto?**  

10. **Como Prisma lida com migrações de banco de dados e por que isso é importante?**  

Essas perguntas ajudarão a avaliar o entendimento do candidato sobre o uso de Node.js, Express, Prisma e SQLite no desenvolvimento de APIs, além de medir sua capacidade de expandir e manter o projeto de forma eficiente.
