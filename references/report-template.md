# Relatório do portão de segurança

## Resultado

**Status:** APROVADO | APROVADO COM RESSALVAS | REPROVADO | NÃO VERIFICADO

**Escopo auditado:** componentes, branch/commit quando disponível, ambiente e limitações.

**Resumo:** uma explicação curta e simples do risco atual.

## Bloqueadores

Listar primeiro achados críticos/altos abertos. Se não houver, escrever “Nenhum bloqueador aberto”.

## Achados

Para cada item:

### SEC-001 — Título

- **Gravidade:** crítica | alta | média | baixa | informativa
- **Status:** aberto | corrigido | risco aceito | não verificado
- **Evidência:** `arquivo:linha`, rota, tabela/policy ou configuração; nunca incluir segredo
- **Impacto:** o que um invasor ou usuário indevido conseguiria fazer
- **Correção:** alteração específica
- **Verificação:** teste/comando e resultado que comprova a correção

## Cinco portas críticas

| Controle | Resultado | Evidência/observação |
|---|---|---|
| RLS do Supabase ou Firebase Security Rules |  |  |
| Autorização no servidor/camada de dados |  |  |
| Propriedade/tenant contra IDOR/BOLA |  |  |
| Segredos no código, bundle e histórico |  |  |
| Validação, uploads e rate limit |  |  |

## Ferramentas e testes

| Verificação | Resultado | Cobertura/limitação |
|---|---|---|
| Testes do projeto |  |  |
| Gitleaks/revisão de segredos |  |  |
| Bandit (Python) |  |  |
| Opengrep/SAST |  |  |
| ZAP/DAST |  |  |
| Dependências |  |  |

Usar `NÃO APLICÁVEL` ou `NÃO EXECUTADO` com justificativa; nunca deixar célula ambígua.

## Correções realizadas

Resumir mudanças e testes de regressão. Em auditoria somente leitura, escrever “Nenhuma alteração realizada”.

## Próximos passos

Listar somente ações restantes, em ordem de prioridade, indicando quais bloqueiam o deploy.
