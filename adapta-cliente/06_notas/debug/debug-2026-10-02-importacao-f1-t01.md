# F1-T01 — Debug Summary (2026-10-02)

**Estado da task:** `aguardando_teste_humano` para conferir a tentativa anterior · **Produção:** v0.0.5 (`40cd26e`) · **Nenhum reenvio após a falha original.**

## Problema e evidência

A tentativa de 02/10 validou com HTTP 200 em 8,93 s, mas a confirmação respondeu HTTP 500 após 30,21 s. O log não revela a causa nem o estado persistido do lote. Não houve nova requisição de importação depois dessa tentativa.

A cópia local disponível contém 45 cabeçalhos, 15.680 linhas de dados e uma célula final vazia excedente por linha. SHA local `4779389f...`; o hash da confirmação registrada é `221dc31f...`. Como os hashes diferem, não posso afirmar que o arquivo local é idêntico ao enviado nem tratar essa hipótese como causa confirmada.

## Correção publicada

- O parser remove uma única célula excedente **somente quando vazia** ao final da linha; continua rejeitando conteúdo excedente.
- A UI identifica claramente o caso sem linhas aceitas e a ação `Registrar rejeições`; essa confirmação registra rejeições, não matrículas válidas.
- A resposta de erro apresenta código de referência quando disponível. Logs novos registram contagens/correlação, sem gravar conteúdo de aluno.
- QA oficial do Skip passou: setup, análise estática, build, integrações e testes. Versão 0.0.5 (`40cd26e`) publicada em produção em 2026-10-02 17:05:28Z. Tela pública verificada como `Acesso restrito`.
- Verificações locais adicionais: suíte do parser 8/8; sintaxe do hook; transformação TSX; parser da cópia disponível reconheceu 15.680 aceitas, 0 rejeitadas, e não expôs `SENHA_ALUNO`. Isso não prova a transação real nem o runtime de confirmação do Skip Cloud.
- `.skip.config.json` permanece como alteração pendente, intacta e fora da versão 0.0.5.

## Dados de produção

- Existe um único ambiente e todos os dados devem permanecer nele.
- Nenhum arquivo foi reenviado após a falha; não houve exclusão, limpeza ou rollback por esta correção.
- Não posso inferir se a tentativa anterior persistiu. O histórico de produção precisa ser conferido antes de qualquer reenvio.

## Próxima ação humana — somente leitura

1. Carlos entra na produção v0.0.5 e abre **Histórico de lotes**.
2. Procura a tentativa de 02/10 por horário/nome e anota apenas **ID do lote, estado e totais**.
3. Se encontrar qualquer lote/registro associado, para e envia esses dados sem CPF, nome, senha ou planilha. Se não encontrar, envia uma captura do histórico sem dados pessoais.
4. **Não reenvia a planilha ainda.** Primeiro vou reconciliar o estado anterior; só então decidimos a próxima gravação.

F1-T01 não está concluída. O teste humano da correção publicada e o aceite do Champion ainda estão pendentes; CA-1-006 (rollback) segue pendente; não executar rollback nem remover registros.
