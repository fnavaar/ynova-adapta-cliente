# Índice — SPECs Fase 1 (recorte onboarding + acompanhamento de leads)

**Escopo:** `02-Escopo-Definitivo.md` v2.0 (25/09) · **Run:** `20260925T1330-adefa2d4-gerar-specs-f1`
**Estado:** conjunto canônico e único da Fase 1 por decisão do consultor (25/09); as SPECs do recorte comercial (15/09) foram substituídas e permanecem no histórico Git.

| SPEC | Título | Critérios | Task | Bloqueios |
|---|---|---|---|---|
| SPEC-1-001 | Importação de planilhas acadêmicas com o Champion na produção | CA-1-001..006 | F1-T01 | B1-001 (planilha real, na produção) |
| SPEC-1-002 | Ficha do aluno e termômetro de engajamento inicial | CA-1-007..012 | F1-T02 | B1-002 (catálogo de eventos, na produção) |
| SPEC-1-003 | Estruturação de acesso: backend fechado e acesso por função | CA-1-013..018 | F1-T03 | B1-003 (usuários/papéis, na produção) |
| SPEC-1-004 | Prova ponta a ponta da fundação com o Champion | CA-1-019..022 | F1-T04 | — (exige 001–003) |

**Sequência:** 001 → 002 → 003 → 004 (linear; uma task por vez, teste humano do Champion entre elas).

**Decisões preservadas:** fronteira pós-comercial (DH-01); zero API (DH-08); contrato de importação e catálogo de eventos nascem com o Champion na produção, sem gate documental prévio (DH-03/DH-04); política de dados como estruturação (DH-05); Champion = Carlos (DH-06).

**Ambiente:** Skip (GoSkip + SkipCloud) — regra geral do consultor (22/09).
