# SPEC-1-002 — Importação reconciliável e reversão de lote

**Fase:** 1  
**Status:** bloqueada até aceite da SPEC-1-001  
**Dono:** Champion operacional da Ynova  
**Origem:** RQ-001; DH-04; Fase 1  
**Degrau:** construção mínima — importar somente o contrato aprovado com recibo, idempotência e reversão por lote.

## Contexto e decisões fechadas
- **Atual:** o CSV é controle operacional sem recibo sistêmico reproduzível.
- **Desejado:** cada tentativa gera lote, contagens, rejeições e reconciliação; nada é corrigido em silêncio.
- **Bloqueios:** SPEC-1-001 aceita; ambiente autorizado; amostra real autorizada.

## Resultado observável
Um operador autorizado importa um lote aprovado, reconcilia total = inseridos + atualizados + rejeitados + duplicados e consegue reverter apenas os efeitos daquele lote sem apagar histórico anterior.

## Limites e dependências
- **Inclui:** prévia, confirmação humana, lote, hash, idempotência, upsert conforme chave aprovada, rejeições, auditoria e reversão.
- **Fora:** integração automática, edição da origem, regra comercial, fila/UX final.
- **Saídas:** recibo, registros vinculados ao lote, rejeições sanitizadas e comprovante de reversão.
- **Permissões:** operador importa/reverte; Champion aprova primeira amostra; demais perfis não executam.
- **Risco/plano B:** fallback de prévia e importação manual controlada; se reconciliação falhar, lote fica FAILED e não é publicado.
- **Rollback:** transação ou compensação determinística por lote, preservando trilha append-only.

## Dados e regras
| Regra | Condição | Resultado | Exceção | Fonte |
|---|---|---|---|---|
| RN-201 | hash+versão já concluídos | retornar recibo existente | reprocessar só com motivo e novo identificador | DH-04 |
| RN-202 | linha inválida | rejeitar com motivo | nunca corrigir origem | F1 |
| RN-203 | conflito na chave | aplicar regra aprovada | ambíguo vira rejeição | B1-002 |
| RN-204 | soma não fecha | lote FAILED, zero publicação | investigação humana | aceite F1 |
| RN-205 | reversão | desfazer somente mutações do lote | preservar histórico prévio e recibos | escopo |

## Fluxo e recuperação
1. Pré-validar arquivo e mostrar prévia sanitizada.
2. Exigir confirmação do operador.
3. Criar lote com hash, versão, ator e horário.
4. Processar linhas de forma idempotente.
5. Conferir equação de reconciliação antes de concluir.
6. Em falha, interromper e compensar; em reversão, registrar ator/motivo.

| Cenário | Condição | Esperado | Recuperação |
|---|---|---|---|
| Principal | lote válido | COMPLETED e contagem fechada | recibo |
| Limite | duplicado/rejeitado | classificação explícita | correção na fonte |
| Falha | timeout/parcial | FAILED/PARTIAL, nunca sucesso | retomar idempotente ou reverter |

## Instruções para o Ethos
1. Ler SPEC-1-001 aceita e contrato aprovado.
2. Implementar somente ingestão manual e controles de lote.
3. Não integrar Portal/Uniasselvi nem criar mensagens.
4. Testar com sintético; primeira amostra real só após autorização humana registrada.
5. Parar se chave, retenção, ambiente ou compensação forem ambíguos.
6. Manter histórico anterior intacto.

## Checklist
- [ ] Contrato/política aceitos.
- [ ] Prévia e confirmação implementadas.
- [ ] Equação de reconciliação passa.
- [ ] Timeout/parcial e reversão exercitados.
- [ ] Logs sanitizados e aceite humano anexados.

## Critérios de aceite
- [ ] **CA-1-007:** todo processamento possui lote, hash, versão, ator, início, fim e estado.
- [ ] **CA-1-008:** contagem total fecha contra inseridos, atualizados, rejeitados e duplicados.
- [ ] **CA-1-009:** duplicado e rejeição têm motivo sem expor dado pessoal em log.
- [ ] **CA-1-010:** mesmo lote não duplica registros; retomada é idempotente.
- [ ] **CA-1-011:** falha parcial nunca é declarada sucesso e pode ser retomada ou revertida.
- [ ] **CA-1-012:** reversão afeta apenas o lote-alvo e preserva auditoria e histórico anterior.

## TDD da SPEC
| Etapa | Prova | Ação | Resultado | Evidência |
|---|---|---|---|---|
| RED | lote com inválido, duplicado e timeout | importar fixture | soma/falha detectadas | teste falhando antes da entrega |
| GREEN | lote válido | importar e reconciliar | CA-007..010 passam | recibo + consulta |
| REGRESSÃO | reprocessar e reverter | repetir hash, simular parcial e reverter | CA-011/012; sem órfãos | diff antes/depois |

**Dados:** sintéticos; amostra real mínima somente com autorização.  
**Evidência:** recibos sanitizados, consultas, diff de reversão e teste humano.

## Handoff e operação
- **Demonstrar:** importar, reconciliar, repetir e reverter fixture.
- **Operar:** operador autorizado; Champion valida primeira amostra.
- **Monitorar:** lotes PARTIAL/FAILED e soma divergente.
- **Pendência:** nenhuma integração automática.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| F1-T03 | Implementar importação manual e reconciliação do lote | @Carlos | SPEC-1-002 | CA-1-007..010 | Importar fixture aprovada, fechar equação e repetir sem duplicar | Recibo de lote, contagens, consulta e teste idempotente | F1-T02 aceita; amostra real continua opcional e autorizada à parte | BLOQUEADA |
| F1-T04 | Provar falha parcial, retomada e reversão de lote | @Carlos | SPEC-1-002 | CA-1-011..012 | Simular timeout/parcial, retomar e reverter somente lote-alvo | Logs sanitizados, diff antes/depois e ausência de órfãos | F1-T03 aceita | BLOQUEADA |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
