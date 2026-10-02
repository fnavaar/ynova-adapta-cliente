# AP-2026-10-02-1416 — reconciliar confirmação de importação 5xx antes de novo envio

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: F1-T01 / SPEC-1-001
- Sinal: uma confirmação de importação em produção retornou HTTP 500 após 30,21 s; os logs disponíveis não revelaram o estado final do lote. Uma resposta 5xx, sem recibo nem reconciliação, não permite concluir ao operador se o lote foi ou não gravado.
- Evidência: logs observados em 2026-10-02; `artifacts/F1-T01-debug-2026-10-02.md`; código de idempotência do hook `pocketbase/hooks/importacoes.js` publicado em `40cd26e`.
- Regra reutilizável: após falha 5xx na confirmação de um lote, não repetir cegamente nem afirmar que nada foi persistido; primeiro procurar o lote no histórico e reconciliar ID/estado/totais. Compartilhar só esses metadados, sem CPF, senha ou planilha.
- Quando aplicar: confirmação de importação longa em ambiente compartilhado/produção, resposta perdida ou HTTP 5xx e estado do lote desconhecido.
- Quando não aplicar: erro de validação recebido antes da confirmação e com nenhuma solicitação de gravação; ainda assim, corrigir/validar a entrada antes de enviar.
- Confiança: média — a ocorrência e o estado desconhecido são observados; o desfecho transacional dessa tentativa ainda aguarda conferência humana.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.
