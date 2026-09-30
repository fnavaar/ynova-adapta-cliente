## 2026-09-30 — F1-T01 publicada; aguardando teste humano

- Skip project `Projeto Engajamento - Ynova` (id 61372): versão 0.0.4 (`d8d6d24`) passou setup, análise estática, build, integrações e testes do pipeline.
- Migração `1790364225_f1_t01_academic_import` aplicada; coleções `alunos`, `polos`, `cursos`, `matriculas`, `lotes_importacao`, `rejeicoes_importacao`, `divergencias_aluno` e `historico_lote` observadas no backend.
- Produção publicada em https://projeto-engajamento-ynova-4ce46.goskip.app; verificação sem autenticação encontrou tela “Acesso restrito”. Nenhuma conta do Champion foi testada.
- Suíte sintética da rota de importação executada localmente contra uma cópia do hook atual: 7/7 passaram (BOM/CSV citado, duplicidade, CPF/pessoa com matrículas distintas, checksum/data, esquema/largura, tamanho/data/nome de arquivo e linha vazia). Isso não valida a persistência/rollback no runtime do backend.
- Nenhum CSV real foi carregado. `SENHA_ALUNO` não deve ser importada. Primeiro teste de produção e recibo real continuam sob responsabilidade do Champion Carlos, conforme SPEC-1-001.
- `.skip.config.json` permaneceu pendente no working tree, não foi alterado por esta rodada e não entrou na versão 0.0.4 publicada.
- F1-T01 não concluída; manter em `aguardando_teste_humano`. F1-T02..T04 continuam bloqueadas.

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
