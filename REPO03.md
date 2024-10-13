# Reposição 3: Criando uma API com NestJS, Prisma e SQLite

Neste tutorial, vamos explorar como criar uma API simples usando **NestJS**, **Prisma**, e **SQLite**. O **NestJS** é um framework moderno e modular para construir aplicativos Node.js eficientes e escaláveis, enquanto o **Prisma** facilita o acesso ao banco de dados. Utilizaremos **SQLite** como banco de dados, por sua simplicidade para desenvolvimento local.

#### 1. Configuração Inicial

Comece criando um novo projeto NestJS:

```bash
npm i -g @nestjs/cli
nest new user-api
```

Após o projeto ser criado, instale as dependências do Prisma:

```bash
npm install --save-dev prisma
```

Inicialize o Prisma e crie o arquivo `prisma/schema.prisma`:

```bash
npx prisma init --datasource-provider sqlite
```

Atualize o arquivo `prisma/schema.prisma` com a configuração do banco de dados **SQLite** e o modelo de usuário:


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

Após configurar o schema, rode as migrações e gere o cliente Prisma:

```bash
npx prisma migrate dev --name init
```

#### 2. Configurando o Prisma no NestJS

Vamos integrar o Prisma ao NestJS. Crie um módulo `PrismaModule` e um serviço `PrismaService` para encapsular a lógica de acesso ao banco de dados.

- **src/prisma/prisma.module.ts**:
- ```bash
nest generate module prisma
```

```ts
import { Module } from '@nestjs/common';
import { PrismaService } from './prisma.service';

@Module({
  providers: [PrismaService],
  exports: [PrismaService],
})
export class PrismaModule {}
```

- **src/prisma/prisma.service.ts**:
- ```bash
nest generate module prisma
```
```ts
import { Injectable, OnModuleInit, OnModuleDestroy } from '@nestjs/common';
import { PrismaClient } from '@prisma/client';

@Injectable()
export class PrismaService extends PrismaClient
  implements OnModuleInit {
  async onModuleInit() {
    await this.$connect();
  }
}
```

Aqui, o `PrismaService` é um serviço que conecta e desconecta do banco de dados conforme o ciclo de vida do módulo.

#### 3. Criando o CRUD de Usuários
- ```bash
nest generate resource user
```

Agora vamos criar o módulo `UsersModule`, o `UsersService`, e o `UsersController` para implementar o CRUD da API.

- **src/users/users.module.ts**:

```ts
import { Module } from '@nestjs/common';
import { UsersService } from './users.service';
import { UsersController } from './users.controller';
import { PrismaModule } from '../prisma/prisma.module';

@Module({
  imports: [PrismaModule],
  controllers: [UsersController],
  providers: [UsersService],
})
export class UsersModule {}
```

- **src/users/dto/create-user.dto.ts**:
```ts
export class CreateUserDto {
  id: number;
  name: string;
  email: string;
}
```

- **src/users/users.service.ts**:

```ts
import { Injectable } from '@nestjs/common';
import { CreateUserDto } from './dto/create-user.dto';
import { UpdateUserDto } from './dto/update-user.dto';
import { PrismaService } from 'src/prisma/prisma.service';

@Injectable()
export class UserService {
  constructor(private readonly prisma: PrismaService) {}
  async create(createUserDto: CreateUserDto) {
    const { name, email } = createUserDto;
    return await this.prisma.user.create({
      data: { name, email },
    });
  }

  async findAll() {
    return await this.prisma.user.findMany();
  }

  async findOne(id: number) {
    return await this.prisma.user.findUnique({
      where: { id },
    });
  }

  async update(id: number, updateUserDto: UpdateUserDto) {
    const { name, email } = updateUserDto;
    return await this.prisma.user.update({
      where: { id },
      data: { name, email },
    });
  }

  async remove(id: number) {
    await this.prisma.user.delete({
      where: { id },
    });
  }
}

```

- **src/users/users.controller.ts**:

```ts
import {
  Controller,
  Get,
  Post,
  Body,
  Patch,
  Param,
  Delete,
} from '@nestjs/common';
import { UserService } from './user.service';
import { CreateUserDto } from './dto/create-user.dto';
import { UpdateUserDto } from './dto/update-user.dto';

@Controller('users')
export class UserController {
  constructor(private readonly userService: UserService) {}

  @Post()
  create(@Body() createUserDto: CreateUserDto) {
    return this.userService.create(createUserDto);
  }

  @Get()
  findAll() {
    return this.userService.findAll();
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return this.userService.findOne(+id);
  }

  @Patch(':id')
  update(@Param('id') id: string, @Body() updateUserDto: UpdateUserDto) {
    return this.userService.update(+id, updateUserDto);
  }

  @Delete(':id')
  remove(@Param('id') id: string) {
    return this.userService.remove(+id);
  }
}

```

Nesse código, implementamos as rotas:
- `GET /users`: Retorna todos os usuários.
- `POST /users`: Cria um novo usuário.
- `PUT /users/:id`: Atualiza os dados de um usuário existente.
- `DELETE /users/:id`: Deleta um usuário.

#### 4. Configurando o Servidor

Certifique-se de que o `AppModule` está importando o `UsersModule`:

- **src/app.module.ts**:

```ts
import { Module } from '@nestjs/common';
import { UsersModule } from './users/users.module';
import { PrismaModule } from './prisma/prisma.module';

@Module({
  imports: [UsersModule, PrismaModule],
})
export class AppModule {}
```

Agora, você pode rodar o servidor NestJS com o seguinte comando:

```bash
npm run start
```

O servidor será iniciado em `http://localhost:3000`.

### Conclusão

Neste tutorial, vimos como criar uma API simples com NestJS, Prisma e SQLite. O NestJS oferece uma estrutura modular que facilita a criação de aplicativos escaláveis, enquanto o Prisma torna o acesso ao banco de dados mais eficiente e seguro. Agora, você tem uma API funcional que pode ser expandida e personalizada conforme as necessidades do projeto.

---

### Perguntas para Entrevista

1. **Qual a vantagem de usar o Prisma com NestJS em comparação a outros ORMs?**  

2. **Como o NestJS facilita a separação de responsabilidades no desenvolvimento de APIs?**  

3. **Explique como funciona o ciclo de vida do Prisma dentro do NestJS.**  

4. **Quais são os principais benefícios do uso de SQLite para este projeto?**  

5. **Qual a importância das validações no `UsersService` e onde você adicionaria essas validações?**  

6. **Como o NestJS implementa injeção de dependência e quais as vantagens disso?**  


