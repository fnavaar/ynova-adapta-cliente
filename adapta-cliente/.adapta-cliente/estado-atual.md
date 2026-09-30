# Estado atual — Adapta Cliente

- task_id: F1-T01
- champion: Carlos
- spec: adapta-cliente/04_fase-atual/specs/spec-1-001-importacao-com-o-champion.md
- etapa: implementando
- autorizacao_implementacao: confirmada em 2026-09-25 (Champion/CEO) e reafirmada pelo consultor (Navaar) em 2026-09-30 — informações reais da planilha ACEITAS como base e modelagem do banco APROVADA (03_documentos/modelagem-banco-f1-t01.md)
- teste_humano: pendente
- verificacao_automatica: pendente
- aprendizado: pendente
- ultima_acao: consultor validou a modelagem (6 coleções) com base nos dados reais (45 colunas, 15.680 linhas) e decidiu que disparos/envios são fase futura (link de redirecionamento, sem integração direta) e não podem bloquear a F1
- proxima_acao: implementar as 6 coleções conforme 03_documentos/modelagem-banco-f1-t01.md e a importação server-side (hook, RLS autenticado, rollback por lote, idempotência por lote); testes com fixtures sintéticas; SENHA_ALUNO não importada; solicitar teste humano de Carlos ao fim
- atualizado_em: 2026-09-30T17:25:00-03:00
