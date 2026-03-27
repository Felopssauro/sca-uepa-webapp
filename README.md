# SCA UEPA - Sistema Cronos de Alocação

<!--toc:start-->
- [SCA UEPA - Sistema Cronos de Alocação](#sca-uepa-sistema-cronos-de-alocação)
  - [Description](#description)
  - [Languages](#languages)
  - [English (en-us)](#english-en-us)
    - [Dependencies](#dependencies)
    - [Tools](#tools)
    - [Initial setup](#initial-setup)
    - [How to contribute to development](#how-to-contribute-to-development)
      - [Work Inside-Out](#work-inside-out)
    - [Essential commands](#essential-commands)
      - [Running other commands inside containers](#running-other-commands-inside-containers)
      - [Git](#git)
      - [Docker and Docker Compose](#docker-and-docker-compose)
        - [Building the environment](#building-the-environment)
        - [Handling containers and images](#handling-containers-and-images)
      - [Prisma](#prisma)
      - [Backend](#backend)
      - [Frontend](#frontend)
  - [Português (pt-br)](#português-pt-br)
<!--toc:end-->

## Description

SCA UEPA is a web app built with NestJS + Prisma for the backend and React + Vite for the user interface. The goal is to provide a simple and effective way to manage the allocation of rooms in the State University of Pará.

## Languages

- [English (en-us)](#english-en-us)
- [Português (pt-br)](#português-pt-br)

## English (en-us)

### Dependencies

To run this project it is recommend you have the following dependencies installed on your machine:

- `git`
- `docker`
- `docker-compose`

### Tools

- React + Vite for the user interface development.
- NestJS API with Prisma for the backend and structure of the database.
- Neon Database for hosting the cloud database server.
- Docker and Docker compose for multi-service dev/prod setups.
- `.gitmodules`: this repository uses git submodules; remember to initialize them before working.

### Initial setup

- Clone the repository:
  - `git clone --recursive https://github.com/Felopssauro/sca-uepa-webapp.git`
- Initialize submodules (if you cloned it without using `--recursive`):
  - `git submodule update --init --recursive`
- Update submodules to track changes of their respective `dev` branches:
  - `git submodule update --remote --merge`
- Provide environment variables inside a .env file for Docker Compose:
  - `DATABASE_URL`: set this to the link of a Neon Database Server.
  - `JWT_SECRET`
  - `VITE_API_BASE_URL`: this is the link that makes the connection of the frontend with the backend.
    - For localhost development, set this variable to: `http://localhost:3000`
  - You can also write a `COMPOSE_FILE` variable with the name of the docker compose file you will be using the most, so you don't have to manually pass the `-f` flag every time.
    - e.g: `COMPOSE_FILE=docker-compose.dev.yml`
  - Build the containers with docker compose:
    - For development:
      - `docker compose -f docker-compose.dev.yml up --build -d`

Now you have a complete setup for development with running containers.

You can access the development frontend at `http://localhost:5173`

The backend is accessible at `htpp://localhost:3000`

But you also need to know how to make contributions and keep track of your changes using git.

### How to contribute to development

After you have a running container with docker compose, you will probably want to make your contribution, so, to do this, follow these steps bellow:

#### Work Inside-Out

Never commit to this repository, without pushing your changes on the submodules repositories first.

Example:

- `cd sca-backend/`
- `git checkout dev` --> if you aren't on the correct branch
- `git add .`
- `git commit -m "feature: example`
- `git push origin dev`

After this you go back to the root folder and commit to this repository, to update the submodules pointers.

- `cd ..`
- `git add sca-backend` --> Never add the submodule name as folder, with a `/` at the end
- `git commit -m "example given`
- `git push origin dev`

### Essential commands

All commands are explained assuming you are running them from the root directory `sca-uepa-webapp/`

#### Running other commands inside containers

It is important to run all `npm` and `npx prisma` commands inside the containers, because this is where all the installed dependencies will be.

- format: `docker compose -f <docker-compose.name.yml> exec <name-of-the-container> npm test`
  - e.g.: `docker compose -f docker-compose.dev.yml exec backend npm test`

#### Git

- `git clone --recursive https://github.com/Felopssauro/sca-uepa-webapp`: clone the repository with the submodules.
- `git submodule update --init --recursive`: fetch the submodules if you cloned the repository without the `--recursive` flag.
- `git submodule update --remote --merge`: fetch changes from the submodules repositories `dev` branch and merge them on your development environment.
  - On the .gitmodules file the submodules are set to track their respective `dev` branches
- `git checkout dev`: change your work branch to dev.
  - Use this if, when inside a submodule folder, your branch is detached and says something like: `sca-backend/ on HEAD (75307ab)`. This means you are tracking the commit and not the branch, use this before commiting any change.
- `git pull origin dev`: fetch and merge the latest changes of this repository
- `git push origin dev`: push your commits to the dev branch on your remote repository.

#### Docker and Docker Compose

When running commands using docker compose, you will have to use `-f` flag to select the specific compose file you will be using on the command. You will to add this every time you run a docker compose command.

##### Building the environment

- For development:
  - `docker compose -f docker-compose.dev.yml up --build -d`
- For production (not tested yet):
  - `docker compose -f docker-compose.prod.yml up --build -d`

##### Handling containers and images

- `docker compose -f <docker-compose.name.yml> exec`: executes commands inside the container
- `docker compose -f <docker-compose.name.yml> stop`: stop containers built with compose file.
- `docker compose -f <docker-compose.name.yml> up`: start containers built with compose file.
- `docker compose -f <docker-compose.name.yml> rm -f`: force removes stopped containers built with compose file.
- `docker compose -f <docker-compose.name.yml> down -v`: removes the containers and
networks for that compose file and deletes named volumes
- `docker image ls`: list docker images
- `docker image rm <image-id-or-name>`: remove docker images

#### Prisma

All prisma commands happen inside the `backend` container, so be sure to follow the format above to run commands inside containers.

- `npx prisma db push`: Push the new structure to the Neon database
- `npx prisma generate`: Rebuild the client
- `npx prisma studio`: Initialize prisma studio to manually add/view rows using the browser interface of Prisma
  - Access prisma studio at `http://localhost:5555`

#### Backend

- `npm run start:dev`: Generate Prisma client and run NestJS in watch mode.
- `npm run build`: Build the NestJS app.
- `npm run start`: Start the app normally.
- `npm run start:prod`: Run the compiled app (`dist/main`).
- `npm run lint`: ESLint with `--fix`.
- `npm run format`: Prettier on `src/**/*.ts` and `test/**/*.ts`.
- `npm test`: Jest unit tests.
- `npm run test:e2e`: Jest e2e tests (`test/jest-e2e.json`).

#### Frontend

- `npm run dev`: Vite dev server.
- `npm run build`: Production build.
- `npm run lint`: ESLint.
- `npm run preview`: Preview production build.

<!-- ## Code organization and conventions -->
<!---->
<!-- ### Backend -->
<!---->
<!-- - NestJS modules are grouped by domain (e.g., `professor/`, `disciplina/`). -->
<!-- - Main module wiring is in `src/app.module.ts`. -->
<!-- - App bootstrap and CORS are in `src/main.ts`. -->
<!-- - Prisma schema and migrations live under `prisma/`. -->
<!---->
<!-- ### Frontend -->
<!---->
<!-- - Main flow and auth state live in `src/App.jsx` (auth uses `localStorage`). -->
<!-- - UI and flow components live under `src/components/`. -->
<!-- - Data/utilities/assets live under `src/data/`, `src/utils/`, `src/assets/`. -->
<!---->
<!-- ## Testing -->
<!---->
<!-- - Backend uses Jest with `*.spec.ts` under `src/` (see `sca-backend/package.json`). -->
<!-- - E2E tests are configured via `test/jest-e2e.json` (see backend scripts). -->
<!-- - Frontend has no explicit test scripts defined beyond linting. -->

## Português (pt-br)
