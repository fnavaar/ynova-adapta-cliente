# Fase 1 — Tarefas

<!-- fase-format:2 -->

- [ ] F1-T01 — Importar a primeira planilha real com o Champion e fechar o contrato de dados @Carlos !02/10/2026 #projeto
  > SPEC-1-001 · CA-1-001..006 · leva 1 · ELEGÍVEL (aguarda autorização de execução). O Carlos aponta a planilha real dentro da própria task; o sistema valida a estrutura, consolida por CPF/nº matrícula e devolve recibo reconciliado com rejeições explicadas. Prova: carga real importada, contagem reconciliada, reenvio idempotente, rollback de lote.
- [ ] F1-T02 — Construir ficha do aluno e termômetro com o catálogo de eventos do Champion @Carlos !09/10/2026 #projeto
  > SPEC-1-002 · CA-1-007..012 · leva 2 · BLOQUEADA (depende de F1-T01). Catálogo de eventos informado pelo Carlos na produção; termômetro explicável com data/hora e justificativa; evento ausente = dado indisponível. Prova: ficha real com estado calculado e histórico append-only.
- [ ] F1-T03 — Implementar RBAC server-side por função com isolamento por polo @Carlos !16/10/2026 #projeto
  > SPEC-1-003 · CA-1-013..018 · leva 3 · BLOQUEADA (depende de F1-T02). Backend fechado, papéis gestão/coordenação/polo, prova negativa de acesso cruzado em UI/URL/endpoint, revogação imediata, trilha de auditoria append-only. Prova: dois polos sintéticos com perfis distintos.
- [ ] F1-T04 — Executar a jornada ponta a ponta da fundação e registrar o aceite da Fase 1 @Carlos !23/10/2026 #projeto
  > SPEC-1-004 · CA-1-019..022 · leva 4 · BLOQUEADA (depende de F1-T01..T03). O próprio Champion executa: importar planilha real → recibo → ficha com termômetro → tentativa de acesso cruzado negada → aceite explícito. Prova: evidências registradas no changelog; sem aceite, a fase não fecha.
