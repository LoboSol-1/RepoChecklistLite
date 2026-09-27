# RepoChecklist Lite

Uma Action simples que eu fiz pra conferir se meus repositórios têm os arquivos básicos no lugar — README, LICENSE, CONTRIBUTING e por aí vai. Ela roda em Pull Request e deixa um checklist comentado.

[![GitHub release](https://img.shields.io/github/v/release/LoboSol-1/RepoChecklistLite?style=flat-square)](https://github.com/LoboSol-1/RepoChecklistLite/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](LICENSE)
[![Actions Status](https://github.com/LoboSol-1/RepoChecklistLite/actions/workflows/test.yml/badge.svg)](https://github.com/LoboSol-1/RepoChecklistLite/actions/workflows/test.yml)

## Por que isso existe

Cansei de abrir PR e perceber depois que faltava `SECURITY.md` ou `.gitignore`. Fiz essa Action pra mim mesmo, pra automatizar essa checagem chata. Se servir pra mais alguém, ótimo.

## O que ela faz

- Olha o repositório e vê quais arquivos essenciais estão faltando.
- Roda em `pull_request` e também dá pra disparar na mão com `workflow_dispatch`.
- Deixa (ou atualiza) um comentário no PR com o resultado.
- Lê uma lista de arquivos de um `.repochecklist.yml` se você quiser customizar.
- Usa só o `GITHUB_TOKEN`, sem serviço externo, sem telemetria, sem nada disso.

## Como funciona por dentro

Bate na API REST do GitHub, lista o conteúdo do repositório, compara com a lista esperada e monta um comentário em Markdown. É isso. Não tem mágica.

## Como usar

Cria o arquivo `.github/workflows/repochecklist.yml` no seu repositório:

```yaml
name: RepoChecklist Lite

on:
  pull_request:
    types: [opened, synchronize, reopened]
  workflow_dispatch:

permissions:
  contents: read
  pull-requests: write

jobs:
  checklist:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Rodar RepoChecklist Lite
        uses: LoboSol-1/RepoChecklistLite@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
