# STATUS — Ynova Educacional

**Recorte:** onboarding + acompanhamento de leads (escopo definitivo v2.0, 25/09) · **Fase:** 1 · **Execução:** 0/4 · **Champion:** Carlos · **Deadline F1-T01:** 02/10/2026

F1-T01 está **em correção** após a tentativa de importação em produção retornar HTTP 500. Logs: validação HTTP 200 em 8,93 s; confirmação HTTP 500 após 30,21 s. O log não revela a causa nem se o lote ficou registrado. A cópia local disponível tem 45 cabeçalhos e 15.680 linhas com uma célula final vazia excedente; seu SHA não coincide com o hash da confirmação, portanto essa explicação é provável, não confirmada.

Correção de parser/UI em v0.0.5 (`40cd26e`), publicada em 2026-10-02; QA oficial passou (setup, análise estática, build, integrações, testes). O parser tolera somente uma célula excedente vazia no fim da linha, mantém rejeição de excedentes com conteúdo, e a tela identifica a ação de registrar rejeições. Verificação local adicional: suíte isolada 8/8 e parser da cópia local 15.680/15.680 aceitas; isso não valida a transação real no Skip Cloud. Produção abre em “Acesso restrito”. `.skip.config.json` continua como alteração pendente, intacta e fora da v0.0.5.

Há **um único ambiente de produção** e os dados devem permanecer nele. Não houve novo envio, importação, exclusão, limpeza ou rollback após a falha. O log continua mostrando somente a confirmação HTTP 500 original; o estado do lote não está confirmado. Próximo passo: Carlos entra na produção, consulta o histórico de lotes da tentativa de 02/10 e me informa ID, estado e totais, sem dados pessoais. **Não reenviar o CSV até conferirmos esse resultado.** Se houver lote concluído ou falho, parar para reconciliação antes de qualquer ação. F1-T02..T04 seguem bloqueadas. `SENHA_ALUNO` não é importada. Zero API.
