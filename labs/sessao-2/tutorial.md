# Lab Sessão 2 - Helm avançado

## Objetivo

Aplicar práticas de maturidade em chart com contrato, validação, reuso e operação segura.

## Escopo

- adicionar `values.schema.json`
- usar `_helpers.tpl` e `include`
- criar teste em `templates/tests`
- simular erro de configuração
- executar upgrade e rollback controlado

## Sequência sugerida

1. Criar uma regra de schema que exija `image.repository`, `image.tag` e `service.port`.
2. Rodar `helm lint` com configuração válida.
3. Quebrar propositalmente uma entrada do `values.yaml`.
4. Confirmar que `helm lint` ou `helm template` falha antes de chegar ao cluster.
5. Mover nomes/labels repetidos para `_helpers.tpl`.
6. Aplicar `include` nos manifests.
7. Adicionar um teste simples em `templates/tests`.
8. Rodar `helm test` após o deploy.
9. Simular release ruim e validar rollback.

## Próxima implementação

Este laboratório será implementado a partir do chart base validado na Sessão 1, mantendo a mesma aplicação para facilitar comparação entre o uso básico e o uso avançado.
