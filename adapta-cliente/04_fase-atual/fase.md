# Fase 1 — Tarefas

<!-- fase-format:2 -->

- [ ] F1-T01 — Importar a primeira planilha real com o Champion e fechar o contrato de dados @Carlos !02/10/2026 #projeto
  > SPEC-1-001 · CA-1-001..006 · **EM CORREÇÃO**. Produção permanece na v0.0.4 (`d8d6d24`). Em 2026-10-02, a confirmação de importação respondeu HTTP 500 após 30,21 s; resultado do lote não confirmado. Uma cópia local tem uma célula vazia excedente em cada linha, mas seu hash difere do enviado: hipótese, não causa confirmada. Correção de parser/UI e regressão preparadas no working tree; teste local passou para essa cópia (15.680 aceitas, 0 rejeitadas), sem prova da persistência. QA oficial não executado nem publicado, pois `.skip.config.json` já estava como alteração pendente e o finalize Skip inclui a working tree. A CEO determinou preservar todos os dados no único ambiente de produção: não reimportar, excluir, limpar, mover ou reverter. Próxima prova: resolver isolamento seguro do arquivo preexistente; QA; conferir estado do lote antes de novo envio; testar correção em produção preservando os dados. CA-1-006 segue pendente.
- [ ] F1-T02 — Construir ficha do aluno e termômetro com o catálogo de eventos do Champion @Carlos !09/10/2026 #projeto
  > SPEC-1-002 · CA-1-007..012 · leva 2 · BLOQUEADA (depende da conclusão e do teste humano aprovado de F1-T01). Catálogo de eventos informado pelo Carlos na produção; termômetro explicável com data/hora e justificativa; evento ausente = dado indisponível. Prova: ficha real com estado calculado e histórico append-only.
- [ ] F1-T03 — Implementar RBAC server-side por função com isolamento por polo @Carlos !16/10/2026 #projeto
  > SPEC-1-003 · CA-1-013..018 · leva 3 · BLOQUEADA (depende de F1-T02). Backend fechado, papéis gestão/coordenação/polo, prova negativa de acesso cruzado em UI/URL/endpoint, revogação imediata, trilha de auditoria append-only. Prova: dois polos sintéticos com perfis distintos.
- [ ] F1-T04 — Executar a jornada ponta a ponta da fundação e registrar o aceite da Fase 1 @Carlos !23/10/2026 #projeto
  > SPEC-1-004 · CA-1-019..022 · leva 4 · BLOQUEADA (depende de F1-T01..T03). O próprio Champion executa: importar planilha real → recibo → ficha com termômetro → tentativa de acesso cruzado negada → aceite explícito. Prova: evidências registradas no changelog; sem aceite, a fase não fecha.
