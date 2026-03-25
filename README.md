# SCA UEPA - Sistema Cronos de Alocação

Aplicação web feita para gerenciar a alocação de salas de aula de uma universidade.

## Contribuindo

### Pré-requisitos

- Docker
- Docker Compose

### Configuração inicial

1. Inicialize os submódulos:

   ```bash
   git submodule update --init --recursive
   ```

2. Defina as variáveis de ambiente usadas pelo compose:
   - `DATABASE_URL`
   - `JWT_SECRET`
   - `VITE_API_BASE_URL`

### Desenvolvimento com Docker

- Ambiente de desenvolvimento:

  ```bash
  docker compose -f docker-compose.dev.yml up --build
  ```

- Ambiente de produção (build local):

  ```bash
  docker compose -f docker-compose.prod.yml up --build
  ```

### Estrutura do projeto

- `cronos-backend/`: API NestJS com Prisma.
- `cronos-frontend/`: React + Vite.
- `docker-compose.dev.yml`: serviços para desenvolvimento.
- `docker-compose.prod.yml`: serviços para build/execução em produção.

### Padrões gerais

- Backend organizado por módulos NestJS (ver `src/app.module.ts`).
- Prisma está em `cronos-backend/prisma/schema.prisma`.
- Frontend organizado em `src/components/`.
