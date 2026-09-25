# Estado atual — Adapta Cliente

- task_id: F1-T01
- champion: Carlos
- spec: adapta-cliente/04_fase-atual/specs/spec-1-001-importacao-com-o-champion.md
- etapa: bloqueada
- autorizacao_implementacao: ausente
- teste_humano: pendente
- verificacao_automatica: pendente — análise da planilha base concluída e plano de modelagem apresentado em 2026-09-25 (45 colunas, 15.680 linhas, sha256 4779389f5a99ef6305e604030fd4e192bd80706f8d71c606adf2929783f63256; CPF 100% presente com 11 dígitos; 260 CPFs com >1 CODIGO_ALUNO; 108 CODIGO_INSCRICAO duplicados divergentes; 274 linhas extras de CPF repetido no lote; 12 colunas N1..N4 vazias; SENHA_ALUNO em 5,4% — recomendação de não importar). Modelo proposto: alunos (chave CPF), matriculas (chave CODIGO_ALUNO), polos, cursos, lotes_importacao, rejeicoes_importacao; RLS autenticado; importação server-side via hook com rollback por lote. Implementação e publicação aguardam autorização em mensagem nova
- aprendizado: pendente
- ultima_acao: plano de modelagem (6 coleções) apresentado à CEO em 2026-09-25; CA-1-004 segue sem decisão (divergência entre lotes: recibo vs ficha da F1-T02)
- proxima_acao: aguardar autorização de implementação e decisão do escopo de CA-1-004
- atualizado_em: 2026-09-25T11:54:55-03:00