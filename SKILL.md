---
name: ia-xss-all-security-stack
description: Audit software projects for common security failures and enforce an evidence-based security gate. Use automatically while building, reviewing, changing, or finishing any website, web app, SaaS, API, mobile backend, authentication/authorization flow, database, Supabase/Firebase integration, file upload, secret-bearing integration, infrastructure or deployment configuration. Run an incremental checkpoint after security-sensitive changes and a mandatory final gate before declaring a coding project complete, deployable, production-ready, published, or finished. Also use for OWASP checks, secret scans, RLS reviews, IDOR/BOLA reviews, SAST, DAST, Gitleaks, Bandit, Opengrep, or ZAP.
---

# IA (X).SS — All Security Stack

Aplicar uma porta de segurança durante o desenvolvimento e antes do deploy. Tratar o checklist como requisito mínimo, não como garantia de invulnerabilidade.

## Escolher o momento da auditoria

- Executar um checkpoint durante o projeto após alterar autenticação, autorização, banco, RLS/Rules, APIs, uploads, pagamentos, segredos, dependências, CORS, sessões ou infraestrutura.
- Executar o portão final em todo projeto de software antes de chamá-lo de concluído ou pronto para deploy.
- Para site estático, ainda verificar formulários/endpoints, bundle público, segredos, dependências, headers e configuração de deploy.
- Repetir somente os testes afetados após uma correção; repetir o conjunto final antes da aprovação.

## Respeitar escopo e segurança operacional

- Começar com inspeção somente leitura. Se o pedido for apenas auditar, relatar e não corrigir sem autorização.
- Se o pedido já incluir construir, corrigir ou preparar para deploy, corrigir achados dentro do workspace e verificar novamente.
- Nunca imprimir valores de segredos. Redigir como `prefixo…[REDACTED]`, informar o local e recomendar revogação/rotação quando houver exposição.
- Não executar ataque ativo, brute force, fuzzing destrutivo, DoS, exclusão de dados ou ZAP Active Scan contra produção sem autorização explícita e alvo confirmado.
- Preferir DAST em ambiente local ou staging com dados descartáveis. ZAP Baseline/passive scan é o primeiro nível.
- Não instalar ferramentas globalmente nem alterar infraestrutura fora do projeto sem autorização. Registrar quando uma ferramenta necessária não estiver disponível.

## Executar o fluxo

### 1. Mapear a superfície

Identificar stack, fronteiras de confiança, dados sensíveis, papéis, autenticação, APIs, bancos, tenants, uploads, integrações externas, variáveis públicas/privadas e ambientes de deploy. Ler `references/security-checklist.md` e selecionar todas as seções aplicáveis.

### 2. Revisar as cinco portas críticas

1. **Banco exposto:** no Supabase, confirmar RLS e policies corretas para cada tabela/view/função exposta; no Firebase, confirmar Security Rules e testes. Ativar RLS sem policies pode apenas bloquear tudo e não comprova isolamento correto.
2. **Permissão no frontend:** confirmar autenticação e autorização novamente no servidor ou na camada de dados. Sidebar, rota escondida ou `if (isAdmin)` no cliente não é controle de acesso.
3. **Rotas sem propriedade:** em toda rota por ID, confirmar autenticação, papel e propriedade/tenant no mesmo acesso. Testar IDOR/BOLA com usuário A tentando acessar objeto de B.
4. **Segredos expostos:** revisar código, arquivos rastreados, bundle público e histórico Git. Chaves administrativas, `service_role`, tokens privados e credenciais ficam somente no servidor. Segredo confirmado exige revogação/rotação; apagar o arquivo não basta.
5. **Entrada não confiável:** validar no servidor, usar consultas parametrizadas/ORM seguro, codificar saída, sanitizar somente quando necessário, restringir upload por tipo real, tamanho, extensão e destino, e aplicar rate limit em operações sensíveis.

### 3. Verificar controles complementares

Revisar sessões/cookies, CSRF quando aplicável, CORS, headers, redirecionamentos, SSRF, path traversal, logs, mensagens de erro, webhooks, dependências, configuração de produção, backups e menor privilégio. Nunca aceitar ausência de achados num scanner como substituto desta revisão.

### 4. Usar ferramentas adequadas

- Executar Gitleaks no conteúdo atual e no histórico Git quando disponível.
- Executar Bandit para código Python quando disponível.
- Executar Opengrep para SAST compatível com a stack quando disponível.
- Executar auditoria nativa de dependências da stack quando segura e reproduzível.
- Executar ZAP contra local/staging quando houver uma URL executável e autorização compatível.
- Antes de usar cada CLI, consultar `--help`/versão; não assumir sintaxe de outra versão.
- Se a ferramenta não existir, fazer a revisão manual possível e marcar o teste como **NÃO EXECUTADO**, com motivo e comando recomendado. Não converter ausência de ferramenta em aprovação.

Consultar `references/scanner-playbook.md` para seleção e limites dos scanners.

### 5. Produzir achados acionáveis

Para cada achado, informar ID e título, gravidade, evidência, impacto, correção, teste de verificação e status. Não incluir o valor de uma credencial na evidência. Evitar afirmações sem evidência; marcar hipóteses como tais.

### 6. Corrigir e verificar novamente

Priorizar crítica/alta, depois média/baixa. Após cada correção, executar teste focado, testes do projeto e os scanners relevantes. Verificar que a correção não introduziu regressão nem apenas moveu a regra para outro cliente confiável.

### 7. Aplicar o portão final

Usar exatamente um destes resultados:

- **APROVADO:** verificações críticas aplicáveis executadas; nenhum achado crítico/alto aberto; testes relevantes passaram.
- **APROVADO COM RESSALVAS:** nenhum crítico/alto aberto, mas há riscos médios/baixos aceitos explicitamente e documentados.
- **REPROVADO:** existe achado crítico/alto aberto ou controle essencial comprovadamente ausente.
- **NÃO VERIFICADO:** faltou acesso, ambiente, ferramenta ou evidência indispensável. Não declarar o projeto pronto para produção.

Um achado crítico/alto nunca pode virar ressalva sem correção ou aceitação explícita e informada do usuário. Ausência de DAST pode ser aceitável para código sem alvo executável, mas deve constar como não aplicável ou pendente com justificativa.

## Entregar o relatório

Usar `references/report-template.md`. Liderar com o resultado do portão, depois bloqueadores, correções realizadas, evidências dos testes e próximos passos. Ser claro para usuário iniciante sem omitir arquivos, rotas e controles técnicos necessários.

## Fontes e atualização

Consultar `references/official-sources.md` quando precisar confirmar sintaxe, regras ou versões. Preferir documentação oficial. As ideias iniciais das cinco falhas e das quatro ferramentas vieram do texto fornecido pelo usuário; esta skill amplia o processo com isolamento de tenant, limites de testes e critérios de aprovação.
