# SPEC-1-003 — Estruturação de acesso: backend fechado e acesso por função

**Fase:** 1 (Fundação) · **Recorte:** onboarding + acompanhamento de leads (escopo definitivo v2.0)
**Fonte:** escopo v2.0 §1/§4-F1; decisão DH-05 (política de dados = estruturação, não gate).
**Depende de:** SPEC-1-001/002 (base e ficha existirem).

## Contexto

A proteção de dados deste ciclo é **estruturação técnica** (decisão DH-05): backend fechado, acesso por função, sem gate documental externo. A base contém CPF, telefone e histórico acadêmico de ~19 mil alunos — o isolamento por polo é a exigência estrutural mínima.

## Resultado observável

Cada usuário do sistema acessa somente o que a sua função permite: membro do polo vê alunos do seu polo; coordenação vê o polo; gestão vê tudo. Toda consulta e escrita passa pelo backend com verificação de função; nenhuma leitura ou escrita direta de dados sensíveis acontece fora dele.

## Critérios de aceite (CA-1-013..018)

- **CA-1-013:** papéis mínimos configuráveis (gestão, coordenação, polo) com matriz de permissões declarada no sistema.
- **CA-1-014:** usuário de polo A não lista, abre nem altera aluno de polo B — prova negativa em UI, URL e endpoint.
- **CA-1-015:** dados sensíveis (CPF, telefone) são exibidos conforme função; log de auditoria registra quem acessou qual ficha e quando.
- **CA-1-016:** revogação de acesso é imediata (usuário removido perde acesso na próxima requisição, sem janela de tolerância).
- **CA-1-017:** toda rota nova do sistema nasce com verificação server-side de função — prova negativa de rota sem guarda.
- **CA-1-018:** a trilha de auditoria é append-only e consultável por gestão (quem, o quê, quando).

## Limites e dependências

- **Bloqueio B1-003 (Champion, na produção):** nomes reais dos usuários/papéis por polo — coletado na execução; a matriz estrutural (gestão/coordenação/polo) já vale.
- Sem política documental externa; minimização permanece como regra técnica.

## Regras

1. Fail-closed: sem função definida, nenhum acesso.
2. Auditoria append-only, sem edição ou remoção.
3. Minimização: CPF/telefone só renderizam quando a função exige.

## Fluxo e recuperação

Login → função resolvida server-side → dados filtrados por polo/escopo. Revogação: remoção do vínculo invalida o acesso imediatamente; recuperação de acesso recria o vínculo com trilha.

## TDD

- **RED:** hoje o acesso às planilhas é por arquivo compartilhado, sem qualquer isolamento por função ou trilha — a prova é a ausência de controle.
- **GREEN:** dois polos sintéticos com perfis distintos; listar/abrir/alterar cruzado retorna negado em UI, URL e endpoint; revogação imediata; trilha registra tudo.
- **REGRESSÃO:** nova rota sem guarda derruba o teste de cobertura de guardas automaticamente.

## Instruções Ethos

Superfícies: middleware server-side de função por rota, coleção de usuários/vínculos por polo, coleção de auditoria append-only. RLS/hooks no SkipCloud conforme padrão do ambiente; nenhuma verificação de acesso no cliente é suficiente por si.

## Tasks vinculadas

| Task | Título | Critérios | Estado |
|---|---|---|---|
| F1-T03 | Implementar RBAC server-side por função com isolamento por polo e trilha de auditoria | CA-1-013..018 | Bloqueada (depende de F1-T02) |
