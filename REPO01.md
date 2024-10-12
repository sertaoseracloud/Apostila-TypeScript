# Reposição 1: Criando uma API com Node.js, Prisma e SQLite (Sem Express)

Se você deseja aprender a criar uma API do zero utilizando apenas Node.js e Prisma, sem depender de frameworks como Express, este artigo é para você. Vamos usar SQLite como banco de dados e TypeScript para garantir um código mais seguro e legível.

#### 1. Configuração Inicial

Primeiro, crie um novo projeto Node.js e instale as dependências necessárias:

```bash
npm init -y
npm install prisma typescript ts-node @types/node --save-dev
```

Configure o TypeScript inicializando o `tsconfig.json`:

```bash
npx tsc --init
```

Agora, crie o arquivo `prisma/schema.prisma` para configurar o Prisma e definir o modelo de usuário:

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

Em seguida, execute os comandos abaixo para configurar o banco de dados SQLite e gerar o cliente Prisma:

```bash
npx prisma migrate dev --name init
```

#### 2. Criando o Servidor HTTP

Agora, crie um servidor simples usando Node.js com a API HTTP nativa. Não utilizaremos Express ou outros frameworks para manter a simplicidade.

Crie o arquivo `src/server.ts` com o seguinte conteúdo:

```ts
import { createServer, IncomingMessage, ServerResponse } from 'node:http';
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

const requestHandler = async (req: IncomingMessage, res: ServerResponse) => {
  if (req.method === 'GET' && req.url === '/users') {
    const users = await prisma.user.findMany();
    res.writeHead(200, { 'Content-Type': 'application/json' });
    return res.end(JSON.stringify(users));
  } 
  if (req.method === 'POST' && req.url === '/users') {
    let body = '';
     for await (const chunk of req) {
        body += chunk;
      }

      const { name, email } = JSON.parse(body);
      const newUser = await prisma.user.create({
        data: { name, email },
      });
      res.writeHead(201, { 'Content-Type': 'application/json' });
      return res.end(JSON.stringify(newUser));
  } 
    res.writeHead(404, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify({ message: 'Rota não encontrada' }));
  }
};

const server = createServer(requestHandler);

server.listen(3000, () => {
  console.log('Servidor rodando em http://localhost:3000');
});
```

Aqui utilizamos o módulo `http` do Node.js para lidar com as requisições. A função `requestHandler` lida com rotas `GET` e `POST`. No `GET`, retornamos todos os usuários do banco de dados, e no `POST`, criamos um novo usuário.

#### 3. Executando o Projeto

Para garantir que o TypeScript e Prisma funcionem corretamente, adicione o seguinte script ao `package.json`:

```json
{
  "scripts": {
    "start": "ts-node src/server.ts"
  }
}
```

Agora você pode iniciar o servidor:

```bash
npm start
```

### Conclusão

Esse exemplo simples mostrou como criar uma API sem o uso de frameworks, utilizando Node.js, TypeScript, Prisma e SQLite. Apesar de ser um projeto básico, é possível expandi-lo para implementar outras funcionalidades ou adicionar frameworks no futuro.

---

### Perguntas para Entrevista

1. **Como você configuraria um projeto para utilizar TypeScript no Node.js?**  

2. **O que é Prisma e como ele interage com o banco de dados SQLite?**  

3. **No modelo Prisma, qual a função do campo `@updatedAt`?**  

4. **Qual a diferença entre as funções `req.on('data')` e `req.on('end')` no código do servidor HTTP?**  

5. **Por que usamos o `JSON.stringify` na resposta da API?**  

6. **Quais seriam as vantagens de utilizar Express nesse projeto, se decidíssemos por adicioná-lo?**  

7. **Como você executaria migrações no Prisma?**  

8. **Se você precisasse adicionar validações nos dados do usuário, onde isso seria feito no código?**  

9. **Como você trataria erros no código atual para melhorar a robustez da API?**  

10. **Como você expandiria essa API para incluir autenticação de usuários?**  


Essas perguntas servem para avaliar o entendimento técnico do candidato sobre os conceitos abordados no artigo, bem como sua capacidade de expandir e melhorar o projeto.
