# Estado atual — Adapta Cliente

- task_id: F1-T01
- champion: Carlos
- spec: adapta-cliente/04_fase-atual/specs/spec-1-001-importacao-com-o-champion.md
- etapa: em_correcao
- autorizacao_implementacao: confirmada em 2026-09-25 12:01 -03:00 — “Ficha mínima já em F1-T01”; autorização do plano de migração, hook, tela, testes e publicação
- teste_humano: falhou em 2026-10-02 — a CEO informou que a tentativa de importar a planilha deu problema; logs de produção mostram validação HTTP 200 e confirmação HTTP 500 após 30,21 s; resultado persistido do lote ainda não confirmado
- verificacao_automatica: local isolada passou em 2026-10-02: node --check do hook e teste 8/8; esbuild transformou a tela TSX sem erro; reprodução do parser sobre a cópia local: 15.680 aceitas, 0 rejeitadas, SENHA_ALUNO não exibida. Isso não valida transação nem timeout do Skip Cloud. QA integrado oficial não executado.
- aprendizado: pendente — registrar causa reutilizável/ausência de sinal após fechar a correção
- ultima_acao: correção mínima (tolerar só uma célula excedente vazia; rejeitar excedente com valor; tornar registro de rejeições explícito na UI; código de erro correlacionável) permanece no working tree do Skip. Produção segue v0.0.4 (`d8d6d24`). `.skip.config.json` preexistente permanece intacta e pendente; não foi chamado finalize, commit ou publish. Nenhum dado da produção foi reenviado, criado, excluído ou revertido nesta rodada.
- proxima_acao: resolver com a CEO o caminho seguro para separar ou incluir `.skip.config.json` no QA oficial; até lá, não chamar o finalize do Skip nem reenviar o CSV. Depois do QA e publicação autorizados, conferir o histórico do lote e pedir novo teste humano na produção, mantendo os dados.
- atualizado_em: 2026-10-02T14:00:58-03:00
