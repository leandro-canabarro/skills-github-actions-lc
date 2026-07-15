# Passo 2: CI e testes com matrix

Agora você vai proteger o OctoMatch com **Continuous Integration (CI)**: uma
pipeline que instala as dependências e roda os testes automaticamente a cada
mudança, em várias versões do Node ao mesmo tempo usando `strategy.matrix`.

## Objetivo

Criar `.github/workflows/ci.yml` que roda os testes (Vitest) do OctoMatch em uma
`matrix` de versões do Node.

## O que você vai fazer

1. Crie o arquivo `.github/workflows/ci.yml`.
2. Dispare em `push` e `pull_request`.
3. Use `strategy.matrix` para testar em mais de uma versão do Node.
4. Instale dependências com `npm ci` e rode `npm test`.

## Conteúdo sugerido

```yaml
name: CI

on:
  push:
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [20, 22, 24]
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: npm

      - name: Instalar dependências
        run: npm ci

      - name: Rodar testes
        run: npm test
```

## Conceito: por que uma matrix?

Uma `strategy.matrix` cria um job por combinação de valores. Aqui, os testes
rodam em paralelo nas versões `20`, `22` e `24` do Node — garantindo que o
OctoMatch funcione em todas elas antes do merge. Isso é a base do CI: _feedback_
rápido e automático sobre a saúde do código.

> [!TIP]
> Depois, no seu repositório, use **branch protection rules** exigindo que o job
> de CI passe (`status check`) antes de permitir o merge na `main`.

## Conclusão do passo

Faça commit e push da pipeline **na branch `feature/actions`**:

```bash
git add .github/workflows/ci.yml
git commit -m "ci: pipeline de testes com matrix de Node"
git push
```

A automação valida a `matrix`, o `npm ci` e o `npm test`, e libera o Passo 3.
