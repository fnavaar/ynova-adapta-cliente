# Modelagem do banco de dados — F1-T01 (validada pelo consultor)

- **Status**: APROVADA pelo consultor (Navaar) em 2026-09-30.
- **Base**: informações reais da planilha do Champion — análise de 2026-09-25 (45 colunas, 15.680 linhas, sha256 `4779389f5a99ef6305e604030fd4e192bd80706f8d71c606adf2929783f63256`). As informações reais são a base ACEITA para a modelagem. Colunas além das nomeadas aqui seguem o contrato da importação: preservar ou rejeitar com motivo — nunca inferir.
- **Papel deste arquivo**: define o modelo que a F1-T01 implementa. A implementação (coleções/migrations no app) continua na F1-T01, uma task por vez, com teste humano de Carlos ao fim.

## Coleções (6)

### 1. alunos
- Chave natural: `cpf` — normalizado para 11 dígitos (somente números), único.
- Campos: campos pessoais da estrutura real da planilha (nome e demais), **exceto SENHA_ALUNO**.
- `lote_importacao` (FK), `linha_origem` (nº da linha no arquivo), timestamps.
- Um aluno pode ter várias matrículas (achado real: 260 CPFs com mais de um CODIGO_ALUNO).

### 2. matriculas
- Chave: `codigo_aluno` (CODIGO_ALUNO).
- FKs: `aluno` (por CPF), `polo`, `curso`.
- `codigo_inscricao`, `divergente` (bool) e `divergencia_ref` (FK para rejeicoes_importacao, tipo=divergencia).
- Campos acadêmicos (incluindo N1..N4): preservar exatamente como recebidos; vazio = null. Nada é inventado.

### 3. polos
- Cadastro normalizado; dedupe por nome normalizado.

### 4. cursos
- Cadastro normalizado; dedupe por nome normalizado.

### 5. lotes_importacao
- Arquivo: nome, sha256 (lote atual: `4779389f...`), total de linhas (15.680), total de colunas (45).
- Contadores: importadas, deduplicadas, divergentes, rejeitadas.
- Status + rollback por lote; reimportação do mesmo lote é idempotente.

### 6. rejeicoes_importacao
- FK `lote`, `linha_origem`, `motivo` (catálogo fechado), `tipo` (`rejeicao` | `divergencia`), payload sanitizado (nunca SENHA_ALUNO).
- **Ficha mínima de divergência (CA-1-004)**: toda divergência entre lotes/linhas fica registrada aqui, com diff do que divergiu — sem sobrescrever o dado importado.

## Regras derivadas dos dados reais

| Achado real (análise 25/09) | Regra de modelagem/importação |
|---|---|
| CPF 100% presente, 11 dígitos | chave de `alunos`; normalizar; inválido → rejeicao (motivo `cpf_invalido`) |
| 260 CPFs com >1 CODIGO_ALUNO | 1 aluno : N matrículas — comportamento esperado, não é erro |
| 108 CODIGO_INSCRICAO duplicados divergentes | importa a primeira ocorrência; as demais geram `divergencia` com diff; nunca sobrescrever; resolução é humana e posterior |
| 274 linhas extras de CPF repetido no lote | dedupe idempotente por (cpf, codigo_aluno) dentro do lote; repetição exata é contada, não reimportada |
| 12 colunas N1..N4 vazias | schema preservado; valores null; nada inventado |
| SENHA_ALUNO em 5,4% das linhas | **NÃO importar** — credencial, não dado de negócio. Necessidade futura = decisão explícita nova |
| Dado real sensível (CPF etc.) | importação server-side via hook; RLS autenticado; testes com fixtures sintéticas; o dado real entra só pela importação da task, com o Champion |

## Fronteira de fase — disparos e envios (decisão do consultor, 2026-09-30)

- Disparos/envios de mensagem **não entram na F1** e **não são gate de nada nesta fase**: serão estruturados em fase futura.
- Na fase futura, muito provavelmente **não haverá integração direta** de envio — apenas um **link que redireciona para o aluno** [INFERÊNCIA] (a estrutura atual já sustenta: o link referencia o aluno por CPF/código; formato do link será definido na fase futura).
- **Regra operacional: não bloquear a F1-T01 (ou qualquer task da F1) por falta de configuração de envio, notificação ou integração de mensagem.** Se surgir essa dúvida, registrar como nota para a fase futura e seguir.

## Não fazer

- Não importar SENHA_ALUNO.
- Não criar coleções/tabelas de campanha, envio ou notificação nesta fase.
- Não inventar campo, chave de dedup ou política fora deste documento. Havendo divergência com o dado real, registrar dúvida dirigida ao Champion (canal único de resposta) e seguir com o que não depende dela — nunca parar a task.
