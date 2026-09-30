# Changelog — Ynova (workspace do cliente)

## 2026-09-30 — Consultor: informações reais aceitas e modelagem do banco aprovada

- Informações reais da planilha (45 colunas, 15.680 linhas, sha256 4779389f...) ACEITAS como base de modelagem (decisão do consultor, Navaar).
- Modelagem do banco APROVADA em `03_documentos/modelagem-banco-f1-t01.md` — 6 coleções (alunos por CPF, matriculas por CODIGO_ALUNO, polos, cursos, lotes_importacao, rejeicoes_importacao com ficha mínima de divergência CA-1-004) e regras derivadas dos achados reais.
- SENHA_ALUNO não é importada (credencial); N1..N4 vazias preservam null; CODIGO_INSCRICAO duplicado divergente vira `divergencia` com diff, sem sobrescrever.
- Disparos/envios: fase futura, muito provavelmente sem integração direta — apenas link de redirecionamento para o aluno. REGRA: não bloquear tasks da F1 por falta de configuração de envio/notificação/integração de mensagem.

## 2026-09-25 — Novo recorte onboarding como projeto canônico

- Escopo definitivo v2.0 aprovado pelo consultor (25/09): fronteira na entrada do aluno na base; comercial fora, sem gatilho de retorno; métrica = consolidação de todas as planilhas/históricos em sistema; Champion = Carlos; zero API; política de dados como estruturação (backend fechado).
- SPECs F1 do novo recorte publicadas como conjunto canônico e único (importação com o Champion na produção; ficha + termômetro com catálogo de eventos do Champion; RBAC server-side por função; prova ponta a ponta com aceite do Champion).
- Tasks F1-T01..T04 publicadas na Jornada (fase-format:2): F1-T01 única elegível (deadline 02/10/2026), demais bloqueadas por dependência.
- SPECs/tasks do recorte comercial (15/09) removidas da unidade ativa; histórico preservado no Git (commits 12d0ad4^).

## DÚVIDA: F1-T01 / CA-1-004 — 2026-09-25 (RESOLVIDA em 25/09)

A SPEC-1-001, CA-1-004, exige que divergências entre registros do mesmo aluno em lotes diferentes sejam sinalizadas "na ficha". A SPEC-1-002 e a sequência da Fase 1 colocam a construção da ficha em F1-T02, que depende da conclusão de F1-T01. **Decisão (25/09):** ficha mínima já em F1-T01 (registro de divergência com diff em rejeicoes_importacao, tipo=divergencia); a ficha completa segue na F1-T02. F1-T01 liberada.
