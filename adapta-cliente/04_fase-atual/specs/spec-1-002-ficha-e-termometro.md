# SPEC-1-002 — Ficha do aluno e termômetro de engajamento inicial

**Fase:** 1 (Fundação) · **Recorte:** onboarding + acompanhamento de leads (escopo definitivo v2.0)
**Fonte:** escopo v2.0 §4-F1; call 15/09 [17:06, 21:39]; decisão DH-04 (catálogo de eventos pelo Champion na produção).
**Depende de:** SPEC-1-001 (base consolidada).

## Contexto

Com a base consolidada, cada aluno precisa de uma ficha única que mostre quem ele é, de qual polo, quando ingressou e o estado dos eventos de engajamento. O catálogo de eventos (aula inaugural, boleto, avaliação, acesso — e quaisquer outros) **é informado pelo Champion durante a produção desta task** (DH-04): o sistema não presume nenhum evento.

## Resultado observável

O Carlos abre a ficha de um aluno e vê: dados de identificação, polo, ingresso e o **termômetro de engajamento** — um estado explicável (ok / em risco / aguardando trâmite / dado indisponível) calculado a partir dos eventos que ele mesmo informou na produção, com a data/hora do cálculo e a lista de eventos que compõem o estado.

## Critérios de aceite (CA-1-007..012)

- **CA-1-007:** a ficha exibe identificação, polo, ingresso e todos os eventos disponíveis do aluno com suas datas.
- **CA-1-008:** o catálogo de eventos é cadastrado com o Champion na produção da task (nome do evento, campo/coluna de origem, direção do risco); eventos não informados não aparecem nem são simulados.
- **CA-1-009:** o termômetro calcula o estado por aluno a partir dos eventos do catálogo, com data/hora do cálculo e justificativa legível (quais eventos geraram o estado).
- **CA-1-010:** evento ausente na planilha produz estado "dado indisponível" para aquele evento — nunca "ok" nem "em risco" por suposição.
- **CA-1-011:** a ficha mostra o histórico de estados do aluno (linha do tempo dos recalques do termômetro).
- **CA-1-012:** o Champion consegue incluir um novo evento no catálogo durante a produção sem reescrever código (parametrização de leitura de coluna).

## Limites e dependências

- **Bloqueio B1-002 (Champion, na produção):** o catálogo inicial de eventos e suas colunas de origem — coletado na execução da task.
- Sem preditivo, sem IA, sem envio automático — cálculo determinístico apenas.

## Regras

1. O estado só usa eventos presentes no catálogo e com dado na planilha.
2. Ausência de dado é exibida como indisponível, nunca otimista.
3. Recalque do termômetro a cada importação; histórico append-only.

## Fluxo e recuperação

Importação concluída (SPEC-1-001) → recalque do termômetro → ficha atualizada. Evento novo no catálogo: recalque retroativo opcional, sempre regenerando histórico novo (nunca sobrescrevendo o passado).

## TDD

- **RED:** hoje não existe ficha nem termômetro — nenhuma visão por aluno na operação (call [21:39]).
- **GREEN:** fixture sintética com eventos do catálogo informado pelo Champion produz estados explicáveis; evento ausente gera "dado indisponível".
- **REGRESSÃO:** recalque com catálogo ampliado não altera estados históricos já registrados.

## Instruções Ethos

Superfícies: coleção de eventos do catálogo (parametrizável), campo de estado calculado na ficha com justificativa, histórico de estados append-only. Nenhum limiar de risco é inventado: limiares vêm do Champion na produção da SPEC-1-002/F2.

## Tasks vinculadas

| Task | Título | Critérios | Estado |
|---|---|---|---|
| F1-T02 | Construir ficha do aluno e termômetro com o catálogo de eventos do Champion | CA-1-007..012 | Bloqueada (depende de F1-T01) |
