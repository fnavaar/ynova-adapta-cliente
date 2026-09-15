# SPEC-1-003 — Ficha, acesso mínimo e fila por polo

**Fase:** 1  
**Status:** bloqueada até aceite da SPEC-1-002 e matriz de acesso aprovada  
**Dono:** Champion operacional da Ynova  
**Origem:** RQ-002; Fase 1  
**Degrau:** recurso nativo da plataforma autorizada — usar autenticação/RBAC nativos; se indisponíveis, parar e retornar à SPEC.

## Contexto e decisões fechadas
- **Atual:** inscritos são acompanhados em CSVs/planilhas locais, com busca manual e responsabilidade pouco rastreável.
- **Desejado:** ficha e fila por polo, com isolamento server-side e trilha de alterações.
- **B1-005 — matriz de acesso:** Champion nomeia perfis e polos; padrão fail-closed.

## Resultado observável
Colaborador autorizado vê somente inscritos dos polos atribuídos; coordenador vê apenas o recorte aprovado; acesso cruzado por tela, URL ou consulta é negado. Cada ficha mostra origem, datas, responsável, status e próximo passo.

## Limites e dependências
- **Inclui:** autenticação, autorização server-side, vínculo usuário-polo, ficha, fila, filtros e auditoria.
- **Fora:** mensagens, follow-up, matrícula, onboarding, integração e painel gerencial F3.
- **Entradas:** registros aceitos da SPEC-1-002 e matriz B1-005.
- **Saídas:** ficha/filas e log append-only sanitizado.
- **Risco/plano B:** sem RBAC nativo comprovado, não expor dados reais; demo com fixtures sintéticas.
- **Rollback:** desabilitar superfície e revogar vínculos sem apagar registros.

## Dados e regras
| Regra | Condição | Resultado | Exceção | Fonte |
|---|---|---|---|---|
| RN-301 | sem vínculo ativo ao polo | negar no servidor | nenhuma por UI | escopo |
| RN-302 | campo pessoal não necessário à fila | ocultar/minimizar | perfil aprovado em B1-003 | política |
| RN-303 | alteração de responsável/status/próximo passo | registrar antes/depois, ator, horário, motivo quando exigido | sem edição silenciosa | RQ-002 |
| RN-304 | ausência de hora de inscrição | mostrar indisponível e usar importação separadamente | nunca inventar | escopo |

## Fluxo e recuperação
1. Autenticar e resolver vínculos ativos.
2. Filtrar no servidor antes de retornar dados.
3. Exibir fila e ficha mínima.
4. Validar transição de status/responsável/próximo passo.
5. Auditar mudança; negar acesso cruzado.

| Cenário | Condição | Esperado | Recuperação |
|---|---|---|---|
| Principal | usuário vinculado | fila do polo | — |
| Limite | usuário com dois polos | união apenas dos vínculos aprovados | revogar vínculo |
| Falha | URL/consulta de outro polo | 403/negação sem PII | log sanitizado |

## Instruções para o Ethos
1. Ler política e matriz B1-005.
2. Implementar autorização no servidor em toda leitura/escrita.
3. Não confiar em filtro da interface.
4. Usar fixtures sintéticas antes do lote real.
5. Parar se perfis, plataforma ou enforcement não estiverem definidos.
6. Preservar importação e auditoria ao desabilitar a interface.

## Checklist
- [ ] Matriz B1-005 aprovada.
- [ ] Campos mínimos mapeados.
- [ ] Provas negativas por UI, URL e consulta.
- [ ] Auditoria sanitizada demonstrada.
- [ ] Champion testa dois perfis e dois polos.

## Critérios de aceite
- [ ] **CA-1-013:** ficha exibe polo, origem, datas disponíveis, responsável, status e próximo passo sem inventar ausências.
- [ ] **CA-1-014:** fila pode ser filtrada por polo e pendência dentro do recorte autorizado.
- [ ] **CA-1-015:** usuário sem vínculo não lê nem altera registro de outro polo por UI, URL ou endpoint.
- [ ] **CA-1-016:** mudança relevante gera evento append-only com ator e horário, sem PII em log técnico.
- [ ] **CA-1-017:** revogar vínculo remove acesso imediatamente sem apagar o histórico.
- [ ] **CA-1-018:** Champion aprova roteiro com ao menos dois polos e perfis distintos.

## TDD da SPEC
| Etapa | Prova | Ação | Resultado | Evidência |
|---|---|---|---|---|
| RED | acesso cruzado | consultar registro alheio | falha antes do RBAC | teste negativo |
| GREEN | acesso válido | listar/abrir/alterar no polo | CA-013/014/016 | capturas + testes |
| REGRESSÃO | URL direta, endpoint, revogação | executar matriz | CA-015/017/018 | relatório RBAC |

**Fixtures:** dois polos, dois colaboradores, um coordenador e registros sintéticos.  
**Evidência:** testes server-side, capturas sem PII e aceite humano.

## Handoff e operação
- **Demonstrar:** alternar perfis, provar acesso permitido/negado e revogação.
- **Operar:** Champion administra matriz; alterações exigem registro.
- **Monitorar:** negações, vínculos órfãos e falhas de auditoria.
- **Pendência:** B1-005.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| F1-T05 | Construir ficha e fila por polo com RBAC server-side | @Carlos | SPEC-1-003 | CA-1-013, CA-1-014, CA-1-016 | Dois polos/perfis: listar, abrir e alterar somente no recorte permitido | Testes server-side, capturas sintéticas e eventos de auditoria | F1-T04 aceita; B1-005 aprovado | BLOQUEADA |
| F1-T06 | Provar isolamento entre polos e revogação de acesso | @Carlos | SPEC-1-003 | CA-1-015, CA-1-017, CA-1-018 | Negar UI/URL/endpoint cruzados e revogar vínculo imediatamente | Matriz de testes negativos e aceite humano de Carlos | F1-T05 aceita | BLOQUEADA |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
