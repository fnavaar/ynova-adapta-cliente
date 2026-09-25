# SPEC-1-001 — Importação de planilhas acadêmicas com o Champion na produção

**Fase:** 1 (Fundação) · **Recorte:** onboarding + acompanhamento de leads (escopo definitivo v2.0)
**Fonte:** escopo v2.0 §4-F1; call 15/09 [19:03, 20:06, 24:43]; decisões DH-03/DH-04/DH-08.
**Ambiente:** Skip (GoSkip + SkipCloud) — regra geral do consultor.

## Contexto

O Champion (Carlos) possui as planilhas acadêmicas de formato fixo (principal + complementares) consolidadas por polo EAD, cruzáveis por CPF ou número de matrícula. Não há API e nunca haverá neste ciclo (DH-08). O contrato de dados **nasce com o Champion dentro da task** — a experiência dele precisa ser a mais simples possível (DH-03): ele aponta o arquivo, o sistema valida e explica o que não coube.

## Resultado observável

O Carlos entrega uma planilha real dentro da própria task de execução; o sistema importa o arquivo, valida a estrutura, consolida os registros por CPF/nº de matrícula e devolve um recibo legível: quantos entraram, quantos foram rejeitados e por quê. Nenhuma etapa burocrática prévia de documentação é exigida dele.

## Critérios de aceite (CA-1-001..006)

- **CA-1-001:** o Champion consegue apontar uma planilha real e iniciar a importação sem preencher qualquer formulário ou dicionário prévio; a estrutura do arquivo é lida e exibida de volta para confirmação em uma tela única.
- **CA-1-002:** registros com chave válida (CPF ou nº de matrícula presente) são consolidados; a contagem importada reconcilia com as linhas do arquivo menos as rejeições exibidas.
- **CA-1-003:** rejeições aparecem uma a uma com motivo legível (campo ausente, formato inválido, chave duplicada no mesmo lote); nenhuma rejeição é descartada em silêncio.
- **CA-1-004:** divergência entre registros do mesmo aluno (lotes diferentes) é sinalizada na ficha, nunca corrigida automaticamente.
- **CA-1-005:** a importação é idempotente: reenviar o mesmo arquivo não duplica registros (mesma chave = atualização com proveniência do novo lote).
- **CA-1-006:** o lote inteiro pode ser revertido (rollback) sem afetar outros lotes; a reversão é registrada na trilha.

## Limites e dependências

- **Bloqueio B1-001 (Champion, na produção):** a primeira planilha real e a indicação de qual é a principal e quais são complementares — coletado dentro da task, não antes.
- Sem API, sem integração, sem disparo — somente leitura de arquivo.
- Dados pessoais (CPF, telefone) trafegam apenas no backend fechado (estruturação, DH-05).

## Regras

1. Proveniência obrigatória: arquivo, data de extração, polo, hash do arquivo.
2. Ausência de dado não é zero nem sucesso — campo vazio é "dado indisponível".
3. A chave de consolidação é definida na primeira carga com o Champion (CPF preferencial; nº matrícula como alternativa) e versionada no sistema.

## Fluxo e recuperação

Importar → validar estrutura → consolidar → recibo. Falha no meio do lote: importação interrompida com motivo, lote identificado, nada consolidado parcialmente sem recibo; retomada reprocessa o lote inteiro.

## TDD

- **RED:** hoje o Carlos trata planilhas manualmente, sem recibo nem reconciliação — a prova é a ausência de qualquer importação no sistema.
- **GREEN:** importar fixture sintética (derivada da estrutura da planilha real apontada pelo Champion na task) e conferir recibo reconciliado; rejeições com motivo; idempotência ao reenviar.
- **REGRESSÃO:** lote com erro no meio não deixa registros órfãos; rollback de um lote não toca os demais.

## Instruções Ethos

Superfícies: coleção de alunos (chave estável), coleção de lotes de importação (proveniência + recibo), tela de importação (upload → confirmação de estrutura → recibo). Toda escrita é server-side; nenhuma SPEC posterior pode presumir campos que o Champion não confirmou na produção desta task.

## Tasks vinculadas

| Task | Título | Critérios | Estado |
|---|---|---|---|
| F1-T01 | Importar a primeira planilha real com o Champion e fechar o contrato de dados na produção | CA-1-001..006 | Elegível (aguarda autorização de execução) |
