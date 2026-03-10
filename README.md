# 🏋️ Bootcamp Treinos API

<div align="center">

**API RESTful para gerenciamento de treinos com Personal Trainer de IA integrado**

![Node.js](https://img.shields.io/badge/Node.js-24-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Fastify](https://img.shields.io/badge/Fastify-5.7-000000?style=for-the-badge&logo=fastify&logoColor=white)
![Prisma](https://img.shields.io/badge/Prisma-7.4-2D3748?style=for-the-badge&logo=prisma&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)

</div>

---

## 📋 Sobre o Projeto

O **Bootcamp Treinos API** é uma API REST completa para gerenciamento de planos de treino, construída com foco em boas práticas de engenharia de software. O diferencial do projeto é a integração com **IA Generativa (GPT-4o-mini)** que atua como personal trainer virtual, criando planos de treino personalizados via chat com o usuário.

### ✨ Destaques

- 🤖 **Personal Trainer com IA** — Chatbot inteligente que monta planos de treino personalizados usando GPT-4o-mini com **tool calling**
- 🔐 **Autenticação robusta** — Login social com Google OAuth via Better Auth
- 📊 **Dashboard de estatísticas** — Métricas de treino com filtros por período
- 📚 **Documentação interativa** — Swagger/OpenAPI auto-gerado com UI Scalar
- 🐳 **Docker multi-stage build** — Imagem otimizada para produção
- ✅ **Validação end-to-end** — Schemas Zod integrados ao Fastify como type-provider

---

## 🏗️ Arquitetura

```
src/
├── index.ts              # Bootstrap do servidor Fastify
├── lib/
│   ├── auth.ts           # Configuração do Better Auth (Google OAuth)
│   ├── db.ts             # Cliente Prisma (PostgreSQL)
│   └── env.ts            # Variáveis de ambiente com validação Zod
├── routes/
│   ├── ai.ts             # POST /ai — Chat com o personal trainer IA
│   ├── home.ts           # GET /home/:date — Dados da home
│   ├── me.ts             # GET /me — Perfil do usuário
│   ├── stats.ts          # GET /stats — Estatísticas de treino
│   └── workout-plan.ts   # CRUD completo de planos de treino
├── usecases/
│   ├── CreateWorkoutPlan.ts
│   ├── GetHomeData.ts
│   ├── GetStats.ts
│   ├── GetWorkoutDay.ts
│   ├── GetWorkoutPlan.ts
│   ├── ListWorkoutPlans.ts
│   ├── StartWorkoutSession.ts
│   ├── UpdateWorkoutSession.ts
│   ├── GetUserTrainData.ts
│   └── UpsertUserTrainData.ts
├── schemas/              # Schemas Zod para validação de request/response
└── errors/               # Classes de erro customizadas
```

O projeto segue uma arquitetura em camadas com separação clara de responsabilidades:

| Camada | Responsabilidade |
|--------|-----------------|
| **Routes** | Recebe requisições HTTP, valida input/output, gerencia autenticação |
| **Use Cases** | Regras de negócio isoladas e testáveis |
| **Schemas** | Contratos de dados (Zod) usados para validação e tipagem |
| **Lib** | Configurações de infraestrutura (DB, Auth, Env) |

---

## 🚀 Tech Stack

| Tecnologia | Uso |
|---|---|
| **[Fastify](https://fastify.dev/)** | Framework HTTP de alta performance |
| **[TypeScript](https://www.typescriptlang.org/)** | Tipagem estática e segurança em tempo de compilação |
| **[Prisma](https://www.prisma.io/)** | ORM type-safe com migrations automáticas |
| **[PostgreSQL](https://www.postgresql.org/)** | Banco de dados relacional |
| **[Better Auth](https://www.better-auth.com/)** | Autenticação com Google OAuth e gerenciamento de sessões |
| **[Vercel AI SDK](https://sdk.vercel.ai/)** | Integração com modelos de IA (GPT-4o-mini) via streaming |
| **[Zod](https://zod.dev/)** | Validação de schemas e type-provider para o Fastify |
| **[Scalar](https://scalar.com/)** | Interface moderna para documentação OpenAPI |
| **[Docker](https://www.docker.com/)** | Containerização com multi-stage build |
| **[pnpm](https://pnpm.io/)** | Gerenciador de pacotes rápido e eficiente |
| **[ESLint](https://eslint.org/) + [Prettier](https://prettier.io/)** | Linting e formatação de código |

---

## 🤖 Personal Trainer com IA

A feature principal do projeto é o **chat com IA** que atua como personal trainer:

```
POST /ai
```

O chatbot utiliza **GPT-4o-mini** com **tool calling** para:

1. **Coletar dados do usuário** — Peso, altura, idade e percentual de gordura
2. **Montar planos de treino** — Com base no objetivo, dias disponíveis e restrições
3. **Escolher a divisão ideal** — Full Body, ABC, Upper/Lower, PPL, PPLUL
4. **Persistir no banco** — O plano criado pela IA é salvo automaticamente via Prisma

### Tools disponíveis para a IA:

| Tool | Descrição |
|------|-----------|
| `getUserTrainData` | Busca dados físicos do usuário |
| `updateUserTrainData` | Atualiza peso, altura, idade e BF% |
| `getWorkoutPlans` | Lista planos de treino existentes |
| `createWorkoutPlan` | Cria um plano completo de 7 dias |

> A IA gera planos respeitando princípios de periodização, agrupamento muscular sinérgico e progressão adequada.

---

## 📡 Endpoints da API

### 🏠 Home
| Método | Rota | Descrição |
|--------|------|-----------|
| `GET` | `/home/:date` | Dados da home page por data |

### 👤 Usuário
| Método | Rota | Descrição |
|--------|------|-----------|
| `GET` | `/me` | Dados do perfil autenticado |

### 📊 Estatísticas
| Método | Rota | Descrição |
|--------|------|-----------|
| `GET` | `/stats?from=&to=` | Estatísticas de treino com filtro de período |

### 🏋️ Planos de Treino
| Método | Rota | Descrição |
|--------|------|-----------|
| `GET` | `/workout-plans` | Listar planos de treino |
| `POST` | `/workout-plans` | Criar plano de treino |
| `GET` | `/workout-plans/:id` | Detalhes de um plano |
| `GET` | `/workout-plans/:id/days/:dayId` | Detalhes de um dia de treino |
| `POST` | `/workout-plans/:id/days/:dayId/sessions` | Iniciar sessão de treino |
| `PATCH` | `/workout-plans/:id/days/:dayId/sessions/:sessionId` | Atualizar sessão (completar) |

### 🤖 IA
| Método | Rota | Descrição |
|--------|------|-----------|
| `POST` | `/ai` | Chat com o personal trainer virtual |

### 🔐 Autenticação
| Método | Rota | Descrição |
|--------|------|-----------|
| `GET/POST` | `/api/auth/*` | Rotas de autenticação (Better Auth) |

> 📚 Acesse a **documentação interativa completa** em `http://localhost:8080/docs`

---

## 🗃️ Modelo de Dados

```mermaid
erDiagram
    User ||--o{ WorkoutPlan : "has many"
    User ||--o{ Session : "has many"
    User ||--o{ Account : "has many"
    WorkoutPlan ||--o{ WorkoutDay : "has many"
    WorkoutDay ||--o{ WorkoutExercise : "has many"
    WorkoutDay ||--o{ WorkoutSession : "has many"

    User {
        string id PK
        string name
        string email UK
        int weightInGrams
        int heightInCentimeters
        int age
        int bodyFatPercentage
    }

    WorkoutPlan {
        uuid id PK
        string name
        string userId FK
        boolean isActive
    }

    WorkoutDay {
        uuid id PK
        string name
        enum weekDay
        boolean isRest
        int estimatedDurationInSeconds
        string coverImageUrl
    }

    WorkoutExercise {
        uuid id PK
        string name
        int order
        int sets
        int reps
        int restTimeInSeconds
    }

    WorkoutSession {
        uuid id PK
        datetime startedAt
        datetime completedAt
    }
```

---

## ⚡ Quick Start

### Pré-requisitos

- [Node.js 24+](https://nodejs.org/)
- [pnpm](https://pnpm.io/)
- [Docker](https://www.docker.com/) (para o PostgreSQL)

### 1. Clone o repositório

```bash
git clone https://github.com/ascef182/bootcamp-treinos-api.git
cd bootcamp-treinos-api
```

### 2. Instale as dependências

```bash
pnpm install
```

### 3. Configure as variáveis de ambiente

```bash
cp .env.example .env
```

Edite o arquivo `.env` com suas credenciais:

```env
PORT=8080

DATABASE_URL="postgresql://postgres:password@localhost:5432/bootcamp-treinos-api"

BETTER_AUTH_SECRET="sua-chave-secreta"
API_BASE_URL="http://localhost:8080"

GOOGLE_CLIENT_ID="seu-google-client-id"
GOOGLE_CLIENT_SECRET="seu-google-client-secret"

GOOGLE_GENERATIVE_AI_API_KEY="sua-chave-gemini"
OPENAI_API_KEY="sua-chave-openai"

WEB_APP_BASE_URL="http://localhost:3000"
```

### 4. Suba o banco de dados

```bash
docker compose up -d
```

### 5. Execute as migrations

```bash
pnpm exec prisma migrate dev
```

### 6. Inicie o servidor

```bash
pnpm run dev
```

O servidor estará disponível em `http://localhost:8080` e a documentação em `http://localhost:8080/docs` 🚀

---

## 🐳 Docker

O projeto utiliza **multi-stage build** para criar uma imagem otimizada:

```dockerfile
# Estágio 1: Base com pnpm
FROM node:24-slim AS base

# Estágio 2: Instalação de dependências
FROM base AS deps

# Estágio 3: Build do TypeScript
FROM deps AS build

# Estágio 4: Imagem de produção (somente deps de produção + dist)
FROM base AS production
```

### Executar com Docker

```bash
# Build da imagem
docker build -t bootcamp-treinos-api .

# Executar (passando variáveis de ambiente)
docker run --env-file .env -p 8080:8080 bootcamp-treinos-api
```

---

## 🧪 Scripts Disponíveis

| Script | Comando | Descrição |
|--------|---------|-----------|
| **Dev** | `pnpm run dev` | Servidor de desenvolvimento com hot-reload (tsx --watch) |
| **Build** | `pnpm run build` | Gera o Prisma Client + compila TypeScript |

---

## 📁 Variáveis de Ambiente

| Variável | Obrigatória | Descrição |
|----------|:-----------:|-----------|
| `PORT` | Não | Porta do servidor (padrão: 8081) |
| `DATABASE_URL` | ✅ | Connection string do PostgreSQL |
| `BETTER_AUTH_SECRET` | ✅ | Secret para assinatura de tokens |
| `API_BASE_URL` | Não | URL base da API (padrão: http://localhost:8081) |
| `GOOGLE_CLIENT_ID` | ✅ | Client ID do Google OAuth |
| `GOOGLE_CLIENT_SECRET` | ✅ | Client Secret do Google OAuth |
| `GOOGLE_GENERATIVE_AI_API_KEY` | ✅ | API Key do Google Gemini |
| `OPENAI_API_KEY` | Não | API Key da OpenAI |
| `WEB_APP_BASE_URL` | ✅ | URL do frontend (CORS) |
| `NODE_ENV` | Não | Ambiente (development/production/test) |

---

## 🤝 Contribuindo

1. Faça um fork do projeto
2. Crie uma branch para sua feature (`git checkout -b feature/minha-feature`)
3. Commit suas mudanças (`git commit -m 'feat: minha nova feature'`)
4. Push para a branch (`git push origin feature/minha-feature`)
5. Abra um Pull Request

---

## 📝 Licença

Este projeto está sob a licença ISC. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

---

<div align="center">

Feito com 💪 por [ascef182](https://github.com/ascef182)

</div>
