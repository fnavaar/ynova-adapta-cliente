# Changelog — Ynova (workspace do cliente)

## 2026-09-25 — Novo recorte onboarding como projeto canônico

- Escopo definitivo v2.0 aprovado pelo consultor (25/09): fronteira na entrada do aluno na base; comercial fora, sem gatilho de retorno; métrica = consolidação de todas as planilhas/históricos em sistema; Champion = Carlos; zero API; política de dados como estruturação (backend fechado).
- SPECs F1 do novo recorte publicadas como conjunto canônico e único (importação com o Champion na produção; ficha + termômetro com catálogo de eventos do Champion; RBAC server-side por função; prova ponta a ponta com aceite do Champion).
- Tasks F1-T01..T04 publicadas na Jornada (fase-format:2): F1-T01 única elegível (deadline 02/10/2026), demais bloqueadas por dependência.
- SPECs/tasks do recorte comercial (15/09) removidas da unidade ativa; histórico preservado no Git (commits 12d0ad4^).

## DÚVIDA: F1-T01 / CA-1-004 — 2026-09-25

A SPEC-1-001, CA-1-004, exige que divergências entre registros do mesmo aluno em lotes diferentes sejam sinalizadas “na ficha”. A SPEC-1-002 e a sequência da Fase 1 colocam a construção da ficha em F1-T02, que depende da conclusão de F1-T01. Qual é o comportamento/tela esperado em F1-T01 para atender CA-1-004 sem antecipar escopo da F1-T02? F1-T01 permanece bloqueada até decisão do autor/consultor das SPECs; não implementar interpretação local.
