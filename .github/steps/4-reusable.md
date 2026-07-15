# Passo 4: Reusable workflows

Sua pipeline está crescendo. Neste passo você vai extrair os _quality checks_
(lint + testes) para um **reusable workflow** e chamá-lo de outros workflows —
eliminando duplicação e centralizando a manutenção.

## Objetivo

Criar um reusable workflow (`on: workflow_call`) com os quality checks e chamá-lo
a partir de um workflow _caller_.

## O que você vai fazer

1. Crie `.github/workflows/quality.yml` com `on: workflow_call`.
2. Mova os steps de lint e teste para dentro dele.
3. Crie (ou ajuste) um workflow _caller_ que use o reusable workflow com
   `uses: ./.github/workflows/quality.yml`.

## Reusable workflow (`quality.yml`)

```yaml
name: Quality Checks

on:
  workflow_call:
    inputs:
      node-version:
        type: string
        default: "24"

jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ inputs.node-version }}
          cache: npm
      - run: npm ci
      - run: npm run lint
      - run: npm run test:coverage
```

## Workflow caller (`.github/workflows/main.yml`)

```yaml
name: Main

on:
  push:
  pull_request:

jobs:
  call-quality:
    uses: ./.github/workflows/quality.yml
    with:
      node-version: "24"
```

## Conceito: permissions em caller e reusable

- Um reusable workflow **herda** o contexto de quem o chama, mas as `permissions`
  do `GITHUB_TOKEN` são definidas pelo _caller_ e **nunca são ampliadas** pelo
  reusable.
- Passe dados via `inputs` e `secrets` (com `secrets: inherit` quando fizer
  sentido).
- Reusable workflows podem ser aninhados (até 4 níveis), permitindo compor
  pipelines complexas a partir de blocos pequenos e testáveis.

> [!TIP]
> Publique reusable workflows em um repositório central para compartilhá-los
> entre vários projetos da sua organização.

## Conclusão do passo

Faça commit e push **na branch `feature/actions`**:

```bash
git add .github/workflows/quality.yml .github/workflows/main.yml
git commit -m "ci: extrair quality checks para reusable workflow"
git push
```

A automação valida o `workflow_call` e a chamada local, e libera o Passo 5.
