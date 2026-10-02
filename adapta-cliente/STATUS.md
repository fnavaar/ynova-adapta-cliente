# STATUS — Ynova Educacional

**Recorte:** onboarding + acompanhamento de leads (escopo definitivo v2.0, 25/09) · **Fase:** 1 · **Execução:** 0/4 · **Champion:** Carlos · **Deadline F1-T01:** 02/10/2026

F1-T01 está **aguardando conferência humana do lote anterior**. Em 02/10, a validação respondeu HTTP 200 em 8,93 s e a confirmação retornou HTTP 500 após 30,21 s. Os logs não identificam a causa nem se a transação ficou registrada; o SHA da cópia local difere do hash da requisição. Portanto, não tratar a hipótese da célula final excedente como causa confirmada.

A correção v0.0.5 (`40cd26e`) passou QA oficial (setup, análise estática, build, integrações e testes) e foi publicada em produção. O parser aceita exatamente uma célula extra final vazia e rejeita outros excedentes; a UI identifica o caminho de registro de rejeições; logs novos usam referência/correlação. Parser isolado 8/8; cópia local com 15.680 linhas passou localmente, sem gravar no banco. Isso não valida a transação do arquivo real. A tela publicada foi verificada como “Acesso restrito”. `.skip.config.json` continua pendente, intacta e fora da versão publicada.

Existe **um único ambiente de produção** e os dados ficam nele. Nenhum reenvio, importação, exclusão, limpeza ou rollback foi feito após a falha. O estado do lote de 02/10 ainda não está confirmado. Próxima ação: Carlos consultar o Histórico de lotes e enviar somente ID, estado e totais, sem dados pessoais. **Não reenviar o CSV até a reconciliação.** Se não localizar a tentativa, apresentar evidência visível e parar antes de qualquer nova gravação. Após a reconciliação, retestar a correção em produção mantendo os dados. F1-T02..T04 seguem bloqueadas; CA-1-006 permanece pendente. `SENHA_ALUNO` não é importada. Zero API.
