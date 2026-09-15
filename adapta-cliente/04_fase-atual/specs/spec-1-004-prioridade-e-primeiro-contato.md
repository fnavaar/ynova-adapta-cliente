# SPEC-1-004 — Prioridade explicável e pendência de primeiro contato

**Fase:** 1  
**Status:** bloqueada até aceite da SPEC-1-003  
**Dono:** Champion operacional da Ynova  
**Origem:** RQ-002; AC-001/DH-01; Fase 1  
**Degrau:** construção mínima — ordenar por sinais determinísticos já disponíveis, sem classificação por IA.

## Contexto e decisões fechadas
- **Atual:** a prioridade depende da planilha e memória individual.
- **Desejado:** fila explica por que cada inscrito está pendente e qual item vem antes.
- **Decisões:** F1 usa pendência/antiguidade; follow-up homologado pertence à F2; sinal da central é opcional e não atribui responsabilidade.
- **B1-006 — encerramento do primeiro contato:** Champion aprova catálogo mínimo de resultados/justificativas que retiram a pendência.

## Resultado observável
O colaborador abre a fila do polo e identifica inscritos sem primeiro contato, ordenados por regra determinística e com motivo visível. A pendência só encerra com resultado/justificativa homologado e auditado.

## Limites e dependências
- **Inclui:** estado pendente, idade calculada, ordenação estável, motivo, registro mínimo de resultado/justificativa e sinal opcional de ocorrência central.
- **Fora:** mensagem, cadência de follow-up, automação, score preditivo, matrícula e onboarding.
- **Entradas:** ficha autorizada, datas válidas e catálogo B1-006.
- **Saídas:** fila explicável e evento de primeiro contato.
- **Risco/plano B:** sem hora de inscrição, usar hora de importação claramente rotulada; sem catálogo, pendência não encerra.
- **Rollback:** desativar nova ordenação e voltar à ordem cronológica, preservando eventos.

## Dados e regras
| Regra | Condição | Resultado | Exceção | Fonte |
|---|---|---|---|---|
| RN-401 | sem resultado válido | permanece pendente | justificativa homologada | F1 |
| RN-402 | hora de inscrição confiável | idade desde inscrição | senão usar importação rotulada | escopo |
| RN-403 | empate | desempate estável por entrada e ID | nenhum aleatório | explicabilidade |
| RN-404 | sinal central confiável | exibir ocorrência separada | não atribuir contato ao polo | DH-02 |
| RN-405 | resultado editado | preservar evento anterior e motivo | sem sobrescrita silenciosa | auditoria |

## Fluxo e recuperação
1. Calcular pendência e idade com fonte identificada.
2. Ordenar por pendência e antiguidade; explicar motivo.
3. Registrar resultado/justificativa por usuário autorizado.
4. Recalcular pendência; auditar evento.
5. Se dado temporal inválido, marcar inconsistência sem inventar prioridade.

| Cenário | Condição | Esperado | Recuperação |
|---|---|---|---|
| Principal | sem contato | topo conforme antiguidade e motivo | registrar resultado |
| Limite | empate/data ausente | ordem estável/rotulada | corrigir fonte |
| Falha | resultado fora do catálogo | recusar; manter pendente | Champion revisa catálogo |

## Instruções para o Ethos
1. Ler RQ-002, DH-01/02 e B1-006.
2. Implementar apenas regra determinística e registro mínimo.
3. Não criar mensagem, follow-up, score ou contato automático.
4. Exibir fonte temporal e motivo da prioridade.
5. Parar se catálogo ou transição estiverem indefinidos.
6. Manter fila cronológica disponível como fallback.

## Checklist
- [ ] B1-006 aprovado.
- [ ] Regra e desempate documentados.
- [ ] Data ausente/inválida exercitada.
- [ ] Resultado inválido mantém pendência.
- [ ] Champion aprova demonstração da entrega visível F1.

## Critérios de aceite
- [ ] **CA-1-019:** inscrito sem resultado/justificativa homologado aparece como pendente.
- [ ] **CA-1-020:** ordenação é determinística, estável e explica fonte temporal e motivo.
- [ ] **CA-1-021:** data ausente/inválida não vira zero nem prioridade inventada.
- [ ] **CA-1-022:** resultado inválido é recusado e a pendência permanece.
- [ ] **CA-1-023:** sinal central, quando confiável, é exibido separadamente sem atribuição automática.
- [ ] **CA-1-024:** Champion executa roteiro ponta a ponta da F1 e aprova explicitamente.

## TDD da SPEC
| Etapa | Prova | Ação | Resultado | Evidência |
|---|---|---|---|---|
| RED | pendentes misturados e resultado inválido | abrir fila/registrar | regra ausente falha | teste inicial |
| GREEN | datas e resultados válidos | ordenar e encerrar pendência | CA-019/020/022 | teste + captura |
| REGRESSÃO | empate, data ausente, sinal central | repetir cenários | CA-021/023/024 | relatório + aceite |

**Fixtures:** registros sintéticos com datas, empate, ausência e sinal central opcional.  
**Evidência:** testes da regra, captura com motivos e roteiro humano ponta a ponta.

## Handoff e operação
- **Demonstrar:** importar fixture, abrir fila por polo, explicar prioridade e registrar resultado.
- **Operar:** Champion mantém catálogo B1-006; colaborador registra resultado.
- **Monitorar:** pendências, datas inválidas e edições de resultado.
- **Pendência:** follow-up e mensagens somente na F2.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| F1-T07 | Implementar prioridade explicável e pendência de primeiro contato | @Carlos | SPEC-1-004 | CA-1-019..023 | Ordenar pendentes, explicar motivo, tratar empate/data ausente e recusar resultado inválido | Testes determinísticos, captura da fila e auditoria do resultado | F1-T06 aceita; B1-006 aprovado | BLOQUEADA |
| F1-T08 | Executar prova ponta a ponta e aceite da Fase 1 | @Carlos | SPEC-1-004 | CA-1-024 | Validar fixture, importar/reconciliar, abrir fila isolada, explicar prioridade e registrar resultado | Roteiro completo, evidências das 4 SPECs e aceite explícito de Carlos | F1-T07 aceita e regressões anteriores verdes | BLOQUEADA |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
