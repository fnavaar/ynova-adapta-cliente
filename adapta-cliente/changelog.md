## 2026-10-02 — DEBUG F1-T01: verificação local isolada

- Reproduzi o comportamento do parser localmente na cópia disponível: a versão anterior rejeitava todas as 15.680 linhas por uma célula vazia final excedente; com tolerância restrita a exatamente uma célula vazia no fim, foram 15.680 aceitas e 0 rejeitadas. Célula extra não vazia segue rejeitada. Nenhuma escrita de produção ocorreu.
- Verificação isolada da cópia atual: `node --check` do hook, `node --test` da suíte (8/8) e transformação esbuild da tela TSX passaram. Essa suíte cobre o parser, não a gravação/transação longa no runtime do Skip Cloud.
- Produção segue na v0.0.4 (`d8d6d24`). A tentativa humana anterior continua HTTP 500 em 30,21 s; o estado do lote não foi consultado por falta de forma segura/read-only. Não foi feito reenvio, limpeza, exclusão, rollback, commit nem publish nesta etapa.
- Causa permanece provável, não confirmada: o hash da cópia local difere do hash da confirmação registrada. A causa do 500 e a integridade do resultado anterior precisam ser confirmadas antes de novo envio.
- `.skip.config.json` é uma alteração preexistente no Skip working tree junto com o patch. O finalize oficial agrega as alterações pendentes; foi mantida intacta. QA oficial e publicação pendentes enquanto não houver caminho autorizado sem incorporar ou descartar essa alteração.
- F1-T01 continua `em_correcao`; não iniciar teste humano na produção até passar QA e publicar a correção.

## 2026-10-02 — DEBUG F1-T01: confirmação de importação HTTP 500

- A CEO relatou que a tentativa de importar a planilha deu problema. Logs de produção: validação `mode=validar` HTTP 200 em 8,93 s às 11:23:42 -03; confirmação `mode=confirmar` HTTP 500 em 30,21 s às 11:25:24 -03. Os logs não contêm a causa técnica, a resposta nem o estado do lote.
- A cópia local disponível tem 45 cabeçalhos, 15.680 linhas de dados e uma célula excedente vazia por linha; a versão anterior do hook rejeita essas linhas por largura. Patch local estrito aceita uma única célula excedente vazia e ainda rejeita qualquer excesso não vazio. Sobre essa cópia, validação local passou em 15.680 aceitas, 0 rejeitadas; sintaxe do hook passou. Não houve gravação.
- O SHA-256 da cópia local (`4779389f...`) não coincide com o hash registrado na confirmação (`221dc31f...`). Portanto, a correspondência do arquivo e a causa-raiz não estão confirmadas. O 500 após 30,21 s pode ter resultado de uma confirmação demorada; não há evidência suficiente para afirmar timeout ou falha parcial.
- Patch também preparou bloqueio da confirmação na UI quando preview tem zero linhas aceitas e apresentação do código de correlação. Regressão cobre uma célula excedente vazia e excesso não vazio.
- Alterações do hook/UI/teste estão no working tree Skip, não commitadas; QA oficial não executado nem publicado. `skip_project_status` lista `.skip.config.json` como alteração preexistente junto com a correção; o finalize do projeto inclui a working tree inteira, então não toquei nessa alteração nem finalizei.
- Nenhum reenvio, exclusão, limpeza ou rollback foi feito. A produção segue na versão 0.0.4 (`d8d6d24`). Não afirmar se o lote foi ou não gravado: o estado deve ser conferido pelo histórico do app antes de nova tentativa.
- Estado F1-T01: `em_correcao`. Próximo passo: resolver um caminho de QA que preserve a alteração preexistente e todos os dados da produção; executar QA; só então publicar e solicitar novo teste humano. Relatório: `artifacts/F1-T01-debug-2026-10-02.md`.

## 2026-10-01 — Política da CEO: preservar dados no único ambiente de produção

- A CEO confirmou que existe somente um ambiente de produção e determinou manter os dados nele.
- O Preview não deve ser tratado como base separada. Nenhum CSV sintético ou real foi importado por esta rodada; nenhum registro foi excluído, limpo, movido ou revertido.
- O handoff local `artifacts/F1-T01-checkpoint.md`, `STATUS.md`, `04_fase-atual/fase.md` e `.adapta-cliente/estado-atual.md` foram alinhados para orientar o teste apenas na produção e proibir limpeza ou rollback sem autorização explícita.
- As fixtures sintéticas, se usadas, geram registros persistentes em produção e podem aparecer nas consultas/indicadores; essa permanência deve ser aceita antes de importar. A primeira planilha real ainda é exigida e `SENHA_ALUNO` permanece excluída.
- CA-1-006 (rollback) segue pendente; não foi testado em produção. F1-T01 continuava aguardando o teste e aceite humanos. A SPEC não foi alterada.

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
