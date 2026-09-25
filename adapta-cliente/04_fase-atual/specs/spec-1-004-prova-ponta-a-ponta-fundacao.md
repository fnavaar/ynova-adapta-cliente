# SPEC-1-004 — Prova ponta a ponta da fundação com o Champion

**Fase:** 1 (Fundação) · **Recorte:** onboarding + acompanhamento de leads (escopo definitivo v2.0)
**Fonte:** escopo v2.0 §4-F1 (aceite binário); decisão DH-06 (Champion = Carlos).
**Depende de:** SPEC-1-001..003.

## Contexto

A Fase 1 fecha com o Champion executando a jornada da fundação inteira com dado real: importar planilha → recibo reconciliado → ficha com termômetro → acesso isolado por função. É o aceite binário da fase e a prova de que o contrato de dados nasceu simples de verdade.

## Resultado observável

O Carlos executa, ele mesmo, o roteiro completo em uma sessão: aponta a planilha real, confere o recibo, abre a ficha de um aluno com termômetro explicável, tenta (e é impedido de) acessar aluno de outro polo, e aprova ou recusa explicitamente a Fase 1.

## Critérios de aceite (CA-1-019..022)

- **CA-1-019:** carga real do Champion importada na sessão com recibo reconciliado (linhas = importadas + rejeitadas).
- **CA-1-020:** ficha de um aluno real exibida com termômetro explicável e eventos do catálogo que ele informou na produção.
- **CA-1-021:** prova negativa de isolamento executada pelo Champion (tentativa de acesso cruzado negada).
- **CA-1-022:** aceite explícito registrado do Champion (aprova ou recusa com motivo); sem aceite, a fase não fecha — silêncio não aprova.

## Limites e dependências

- Exige as três SPECs anteriores entregues e testadas.
- Nenhum dado de produção além da planilha que o próprio Champion apontar.

## Regras

1. O roteiro é executado pelo Champion, não pela consultoria.
2. Falha em qualquer ponto mantém a fase aberta e roteia para debug.
3. Evidências da sessão (recibo, telas, aceite) são registradas no sistema e no changelog.

## TDD

- **RED:** a jornada completa não existe hoje — planilhas manuais sem sistema.
- **GREEN:** sessão ponta a ponta com o Champion usando planilha real; todos os 4 critérios demonstrados.
- **REGRESSÃO:** reexecutar o roteiro após qualquer correção da fase antes do aceite final.

## Instruções Ethos

Superfícies: roteiro de teste documentado na task, registro de evidências (recibo + aceite) no changelog do plano. A prova é executada pelo Champion; a consultoria só acompanha e registra.

## Tasks vinculadas

| Task | Título | Critérios | Estado |
|---|---|---|---|
| F1-T04 | Executar a jornada ponta a ponta da fundação com o Champion e registrar o aceite da Fase 1 | CA-1-019..022 | Bloqueada (depende de F1-T01..T03) |
