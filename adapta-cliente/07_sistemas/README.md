# 07_sistemas — Sistemas do cliente

Esta pasta organiza os repositórios de sistemas técnicos do cliente, referenciados como submodules do workspace `adapta-cliente`.

## Submodule registrado

| Campo | Valor |
|---|---|
| Repositório | https://github.com/carloskubrusly-hue/projeto-engajamento---ynova-b7vcf11r1 |
| Commit pinado (HEAD em 25/09/2026) | `eb17678a6e540ba2c06b8f6154076e16a7ac8ad1` |
| Destino | `07_sistemas/projeto-engajamento---ynova-b7vcf11r1` |
| Dono | Carlos Kubrusly (Champion) — conta `carloskubrusly-hue` |
| Contexto | Sistema do novo recorte (onboarding/engajamento) da Ynova — SPECs F1 em `04_fase-atual/specs/` |

## Como materializar o submodule

O gitlink ainda não está gravado na árvore do Git (o conector de publicação não cria entradas `mode 160000`). Com credenciais git no clone local, execute uma vez:

```bash
git submodule add https://github.com/carloskubrusly-hue/projeto-engajamento---ynova-b7vcf11r1.git 07_sistemas/projeto-engajamento---ynova-b7vcf11r1
git submodule status  # conferir o commit pinado
```

Isso cria `.gitmodules` e o gitlink; o commit resultante substitui este README como referência operacional.

## Nota de fronteira

O conteúdo do repo do cliente é de responsabilidade do Champion/da operação da Ynova. O workspace `adapta-cliente` referencia e acompanha; não publica nem altera código no repo do cliente sem autorização explícita.
