# Setup do ambiente

Olá @{{login}}! Bem-vindo ao treinamento **GitHub Actions do zero ao CI/CD**.

Ao longo de ~2 horas você vai automatizar o **OctoMatch**, um jogo da memória feito em React + Vite, passando por todos os pilares do GitHub
Actions: `workflow`, `job`, `step`, `runner`, testes em CI, `matrix`,
`artifacts`, `reusable workflows` e `GitHub Models`.

## O que você vai fazer neste setup

1. Abrir o projeto em um GitHub Codespace (ou clonar localmente).
2. Instalar as dependências e rodar o jogo.
3. Rodar os testes automatizados.
4. Criar a branch de trabalho do exercício.

## Passo a passo

1. No seu repositório, clique em **Code > Codespaces > Create codespace on main**
   ou use o atalho abaixo.

   [![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/{{full_repo_name}}?quickstart=1)

2. Aguarde o ambiente subir. As extensões e o `Node.js` já vêm configurados.

3. Instale as dependências e suba o jogo em modo de desenvolvimento:

   ```bash
   npm install
   npm run dev
   ```

   Abra a porta `5173` encaminhada e jogue algumas rodadas do OctoMatch. 🐙

4. Rode a suíte de testes (Vitest) e verifique a cobertura:

   ```bash
   npm test
   npm run test:coverage
   ```

5. (Opcional) Rode os testes end-to-end com Playwright:

   ```bash
   npx playwright install --with-deps chromium
   npm run test:e2e
   ```

6. Crie e publique a branch de trabalho. Você vai desenvolver **todos os passos**
   nela e integrá-la à `main` por Pull Request ao final:

   ```bash
   git checkout -b feature/actions
   git push -u origin feature/actions
   ```

## Conceitos-chave

- **workflow**: arquivo YAML em `.github/workflows/` que descreve uma automação.
- **event**: o gatilho (`on:`) que dispara o workflow (`push`, `pull_request`, ...).
- **job**: conjunto de steps executado em um `runner`.
- **runner**: a máquina que executa o job (ex.: `ubuntu-latest`).
- **step**: uma ação individual (`uses:` uma action ou `run:` um comando).

## Conclusão do passo

O setup não exige validação automática. Com o jogo rodando, os testes passando
e a branch `feature/actions` publicada, siga para o **Passo 1: Primeiro
workflow**, publicado logo abaixo nesta issue.
