# SPEC-1-001 — Contrato do lote e política mínima de dados

**Fase:** 1  
**Status:** bloqueada até B1-001, B1-002, B1-003 e B1-004  
**Dono:** Champion operacional da Ynova  
**Origem no escopo:** RQ-001; AC-003/DH-04; Fase 1  
**Degrau da solução:** construção mínima — formalizar um contrato versionado antes de permitir qualquer ingestão real.

## Contexto e decisões fechadas

- **Estado atual:** inscritos chegam por arquivo/CSV, mas amostra, dicionário, cadência, deduplicação e política ainda não estão formalizados.
- **Estado desejado:** um lote pode ser validado contra contrato explícito, sem armazenar dado real enquanto os gates humanos estiverem abertos.
- **Decisões fechadas:** importação manual rastreável; sem API; envio e decisão comercial permanecem humanos.
- **B1-001 — amostra e dicionário:** Champion entrega arquivo de amostra autorizado, campos, tipos e exemplos válidos/inválidos.
- **B1-002 — identidade/deduplicação:** Champion define chave estável e regra para conflito; o executor não escolhe CPF, e-mail ou telefone por conta própria.
- **B1-003 — política mínima:** responsável humano aprova finalidade, campos mínimos, perfis de acesso, retenção, descarte e tratamento de rejeições.
- **B1-004 — ambiente:** Consultor autoriza projeto/repositório e ambiente de teste antes de qualquer alteração técnica.

## Resultado observável

O Champion consegue submeter uma fixture sintética e receber um relatório versionado de aceitação/recusa por campo. Dado real permanece recusado até B1-001..004 serem fechados.

## Limites e dependências

- **Inclui:** schema de entrada, versão, codificação, delimitador, campos, validações, chave candidata, cadência, responsável, política mínima e relatório de validação.
- **Fora:** ingestão real, integração, fila, mensagens, matrícula e onboarding.
- **Entradas:** fixture sintética; posteriormente, amostra autorizada.
- **Saídas:** contrato versionado, relatório de validação e registro dos bloqueios.
- **Atores/permissões:** Champion aprova; operador autorizado submete; usuário sem perfil recebe negação.
- **Risco/plano B:** se a fonte variar, manter recepção manual e devolver relatório sem persistir linhas.
- **Rollback:** reverter versão do contrato; nenhum dado real deve existir nesta SPEC.

## Dados e integrações

| Origem/destino | Fonte de verdade | Campos/contrato | Autenticação/permissão | Idempotência | Erro |
|---|---|---|---|---|---|
| arquivo → validador | arquivo autorizado + contrato versionado | definidos em B1-001 | operador autorizado | hash do arquivo + versão | recusar sem persistir |

| Regra | Condição | Resultado | Exceção | Fonte |
|---|---|---|---|---|
| RN-101 | bloqueio aberto | dado real recusado | nenhuma | escopo §5 |
| RN-102 | campo fora do contrato | linha recusada com código e posição | nenhuma correção silenciosa | DH-04 |
| RN-103 | mesmo hash e versão | mesmo recibo, sem duplicação | nova versão gera nova validação | F1 |
| RN-104 | dado opcional ausente | preservar ausência; nunca converter em zero/valor inventado | conforme contrato aprovado | escopo |

## Fluxo e recuperação

1. Verificar ambiente e perfil.
2. Calcular hash e identificar versão do contrato.
3. Validar cabeçalho, tipos, obrigatórios e qualidade.
4. Gerar contagens e erros por linha/campo, sem persistir dado real.
5. Champion aceita ou devolve o contrato.

| Cenário | Condição | Esperado | Recuperação |
|---|---|---|---|
| Principal | fixture válida | relatório aceito e reproduzível | guardar recibo |
| Limite | vazio, coluna extra, encoding inválido | recusa explicada | corrigir fonte/contrato |
| Falha | usuário sem perfil ou dado real com gate aberto | acesso negado e zero persistência | fechar gate humano |

## Instruções para o Ethos

1. Ler escopo F1, DH-04 e esta SPEC.
2. Alterar somente validador, contrato e evidências no ambiente autorizado.
3. Não criar integração, fila ou importar dado real.
4. Começar por fixtures sintéticas e testes negativos.
5. Parar diante de campo, chave, política ou ambiente não aprovados.
6. Estado válido: validação sintética funciona e dado real é recusado.

## Checklist
- [ ] B1-004 registrado.
- [ ] Fixture sintética cobre válido, vazio, inválido e duplicado.
- [ ] Relatório não expõe valor pessoal; usa linha, campo e código.
- [ ] Zero persistência de dado real demonstrada.
- [ ] Champion aprovou contrato/política ou bloqueios continuam explícitos.

## Critérios de aceite
- [ ] **CA-1-001:** contrato versionado declara formato, campos, tipos, obrigatoriedade e códigos de erro.
- [ ] **CA-1-002:** fixture válida produz contagem reproduzível; inválida é recusada por linha/campo.
- [ ] **CA-1-003:** reprocessar mesmo hash/versão não duplica recibo.
- [ ] **CA-1-004:** dado real e usuário sem perfil são recusados enquanto os gates estiverem abertos.
- [ ] **CA-1-005:** relatório e logs não expõem valores pessoais.
- [ ] **CA-1-006:** Champion aprova explicitamente contrato, chave e política antes da SPEC-1-002.

## TDD da SPEC

| Etapa | Prova | Ação | Resultado | Evidência |
|---|---|---|---|---|
| RED | fixture inválida e dado real | executar validador antes das regras | falhas observáveis; zero persistência | saída de teste sanitizada |
| GREEN | contrato mínimo | executar fixtures válidas/inválidas | CA-001..005 passam | relatório + testes |
| REGRESSÃO | mesmo hash, usuário negado, coluna extra | repetir cenários | sem duplicidade/vazamento | log sanitizado |

**Fixtures:** exclusivamente sintéticas até B1-001..004.  
**Evidência exigida:** contrato, relatório sanitizado, testes e aceite humano.

## Handoff e operação
- **Demonstrar:** validar quatro fixtures e mostrar zero persistência real.
- **Operar:** Champion mantém versão e aprova mudanças.
- **Monitorar:** recusas por código, versão e hash.
- **Pendência:** B1-001..004.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| F1-T01 | Fechar contrato de entrada, política e ambiente de teste | @Carlos | SPEC-1-001 | CA-1-001, CA-1-006 | Contrato versionado + registro B1-001..004 + aceite explícito do Champion | Contrato, matriz mínima, fixture sintética e recibo de aceite | SPECs F1 aprovadas | ELEGÍVEL |
| F1-T02 | Construir e provar o validador sintético do lote | @Carlos | SPEC-1-001 | CA-1-002..005 | Fixtures válida, inválida, duplicada e acesso negado; repetição idempotente | Testes, relatório sanitizado e prova de zero persistência real | F1-T01 aceita e teste humano autorizado | BLOQUEADA |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
