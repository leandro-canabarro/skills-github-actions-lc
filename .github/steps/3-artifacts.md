# Passo 3: Workflow artifacts

Os testes rodam, mas os relatórios somem quando o `job` termina. Neste passo você
vai persistir o **relatório de cobertura** como um **workflow artifact**, para
baixá-lo e inspecioná-lo depois da execução.

## Objetivo

Atualizar `.github/workflows/ci.yml` para gerar a cobertura e publicá-la com
`actions/upload-artifact`.

## O que você vai fazer

1. Troque o comando de teste por `npm run test:coverage` (gera a pasta
   `coverage/` com relatórios `lcov`, `html` e `json-summary`).
2. Adicione um `step` com `actions/upload-artifact` para publicar `coverage/`.
3. (Opcional) Faça upload de um arquivo único — o relatório HTML do Playwright.

## Conteúdo sugerido

```yaml
      - name: Rodar testes com cobertura
        run: npm run test:coverage

      - name: Publicar relatório de cobertura
        uses: actions/upload-artifact@v4
        with:
          name: coverage-node-${{ matrix.node-version }}
          path: coverage/
          retention-days: 7
```

## Conceito: artifacts

- **Artifact**: arquivos gerados por um `job` que o GitHub armazena após a
  execução (relatórios, builds, binários).
- São ideais para _compartilhar dados entre jobs_ (baixando com
  `actions/download-artifact`) e para inspeção manual pela aba **Actions**.
- Use `name` único por combinação de `matrix` para evitar colisões.
- `retention-days` controla por quanto tempo o artifact fica disponível.

> [!TIP]
> Uma prática comum de CI/CD é **build once, reuse everywhere**: gere o build uma
> única vez, publique como artifact e reutilize-o nos jobs de deploy — sem
> rebuildar.

## Conclusão do passo

Faça commit e push **na branch `feature/actions`**:

```bash
git add .github/workflows/ci.yml
git commit -m "ci: publicar cobertura como artifact"
git push
```

A automação valida o uso de `actions/upload-artifact` e libera o Passo 4.
