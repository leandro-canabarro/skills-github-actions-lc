# Passo 5: AI in Actions com GitHub Models

No passo final você vai adicionar **inteligência** à automação usando o
**GitHub Models** direto no workflow, com a action `actions/ai-inference`. O
objetivo é fazer uma **triagem automática de issues**: quando uma issue é aberta,
a AI resume e classifica o conteúdo e comenta o resultado.

## Objetivo

Criar `.github/workflows/ai-triage.yml` que, ao abrir uma issue, envia o título e
o corpo para um modelo do GitHub Models e comenta a análise na própria issue.

## O que você vai fazer

1. Crie `.github/workflows/ai-triage.yml`.
2. Dispare no `event` `issues` com `types: [opened]`.
3. Conceda a permissão `models: read` (necessária para o GitHub Models).
4. Use `actions/ai-inference` com um `prompt` dinâmico e comente o resultado.

## Conteúdo sugerido

```yaml
name: AI Issue Triage

on:
  issues:
    types: [opened]

permissions:
  issues: write
  models: read

jobs:
  triage:
    runs-on: ubuntu-latest
    steps:
      - name: Analisar a issue com GitHub Models
        id: inference
        uses: actions/ai-inference@v1
        with:
          model: openai/gpt-4o-mini
          system-prompt: |
            Você é um assistente de triagem. Responda em português do Brasil.
            Gere um resumo curto e sugira até 3 labels adequadas.
          prompt: |
            Título: ${{ github.event.issue.title }}
            Corpo: ${{ github.event.issue.body }}

      - name: Comentar a análise na issue
        uses: actions/github-script@v7
        env:
          AI_RESPONSE: ${{ steps.inference.outputs.response }}
        with:
          script: |
            github.rest.issues.createComment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.issue.number,
              body: ['### 🤖 Triagem automática', '', process.env.AI_RESPONSE].join('\n')
            })
```

## Conceito: GitHub Models em workflows

- **GitHub Models** expõe modelos de linguagem que você consome direto no
  workflow — sem gerenciar chaves de API externas.
- A permissão `models: read` é obrigatória e o `GITHUB_TOKEN` já autentica a
  chamada.
- Você monta **prompts dinâmicos** com dados do `event` (issue, PR, commit) e
  combina a saída da AI com outras actions (comentar, rotular, abrir PR).

> [!NOTE]
> O GitHub Models precisa estar habilitado para a sua conta/organização. Se não
> estiver disponível, o conceito e o YAML seguem válidos como referência.

## Módulo bônus (opcional)

Se sobrar tempo, explore como **empacotar sua própria action**:

- [Write JavaScript Actions](https://github.com/skills/write-javascript-actions) —
  crie uma action JavaScript com `action.yml` e a use no workflow.
- [Create AI Powered Actions](https://github.com/skills/create-ai-powered-actions) —
  combine GitHub Models e schemas Zod dentro de uma action customizada.

## Conclusão do passo

Faça commit e push **na branch `feature/actions`**:

```bash
git add .github/workflows/ai-triage.yml
git commit -m "ci: triagem de issues com GitHub Models"
git push
```

A automação valida o `actions/ai-inference` e a permissão `models: read`,
publica a revisão final e **encerra o exercício**. Para finalizar, abra um Pull
Request de `feature/actions` para a `main` e faça o merge. 🎉
