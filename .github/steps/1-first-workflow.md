# Passo 1: Seu primeiro workflow

Neste passo você cria o seu primeiro **workflow** do GitHub Actions e entende a
anatomia de um `event`, um `job`, um `runner` e um `step`.

## Objetivo

Criar um workflow que roda em cada `push` e comenta uma saudação em toda Pull
Request aberta contra a `main`.

## O que você vai fazer

1. Na branch `feature/actions`, crie o arquivo
   `.github/workflows/hello-world.yml`.
2. Configure os `events` `push` e `pull_request`.
3. Adicione um `job` que roda em `ubuntu-latest`.
4. Adicione um `step` com `run` (um comando de shell) e um `step` que comenta na
   Pull Request.

## Conteúdo sugerido

```yaml
name: Hello World

on:
  push:
  pull_request:
    types: [opened]

permissions:
  pull-requests: write

jobs:
  say-hello:
    runs-on: ubuntu-latest
    steps:
      - name: Saudação no log
        run: echo "Olá! Este é o meu primeiro workflow do GitHub Actions."

      - name: Comentar na Pull Request
        if: github.event_name == 'pull_request'
        uses: actions/github-script@v7
        with:
          script: |
            github.rest.issues.createComment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.issue.number,
              body: '👋 Workflow disparado automaticamente pelo GitHub Actions!'
            })
```

## Conceito: anatomia do workflow

| Elemento    | O que é                                                        |
| ----------- | -------------------------------------------------------------- |
| `on`        | os `events` que disparam o workflow                            |
| `jobs`      | grupos de trabalho executados (em paralelo por padrão)         |
| `runs-on`   | o `runner` que executa o job                                   |
| `steps`     | as tarefas do job, na ordem                                    |
| `uses`      | executa uma `action` reutilizável                              |
| `run`       | executa um comando de shell no runner                          |
| `permissions` | os escopos do `GITHUB_TOKEN` disponíveis para o workflow     |

## Conclusão do passo

Faça commit e push do workflow **na branch `feature/actions`**:

```bash
git add .github/workflows/hello-world.yml
git commit -m "ci: primeiro workflow hello world"
git push
```

A automação valida a presença do workflow (com `job` e `runs-on`) e libera o
Passo 2. Acompanhe a aba **Actions** do repositório para ver seu workflow rodar.
