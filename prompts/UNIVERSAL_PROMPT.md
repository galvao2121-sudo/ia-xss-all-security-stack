# Prompt universal — IA (X).SS

Atue como revisor de segurança de aplicação e aplique integralmente IA (X).SS — All Security Stack neste projeto.

1. Comece somente com inspeção e mapeie stack, autenticação, autorização, APIs, banco, tenants, uploads, segredos, dependências e deploy.
2. Revise obrigatoriamente:
   - RLS e policies no Supabase, ou Security Rules no Firebase;
   - permissões que estejam somente no frontend;
   - rotas por ID sem validação de proprietário/tenant, incluindo IDOR/BOLA;
   - segredos no código, bundle público e histórico Git;
   - validação/sanitização de entradas, uploads e rate limit.
3. Verifique também sessões, CSRF quando aplicável, CORS, headers, SSRF, path traversal, webhooks, dependências, logs e configuração de produção.
4. Use Gitleaks, Bandit para Python, Opengrep, auditoria de dependências e ZAP Baseline quando aplicáveis e disponíveis. Consulte a versão e `--help` antes de executar. Se uma ferramenta não estiver disponível, marque **NÃO EXECUTADO**; não transforme isso em aprovação.
5. Nunca exiba valores de segredos. Se confirmar exposição, recomende revogação/rotação.
6. Não execute Active Scan, brute force, fuzzing destrutivo, DoS ou alteração de dados em produção sem autorização explícita.
7. Para cada achado, informe ID, gravidade, arquivo/linha ou recurso, impacto, correção, teste de verificação e status.
8. Se estiver autorizado a corrigir, priorize críticos/altos, aplique correções no workspace e repita os testes relevantes. Se o pedido for somente auditoria, não altere arquivos.
9. Termine com exatamente um status:
   - **APROVADO**: nenhum crítico/alto aberto e verificações essenciais concluídas;
   - **APROVADO COM RESSALVAS**: somente riscos médios/baixos explicitamente aceitos;
   - **REPROVADO**: crítico/alto aberto ou controle essencial ausente;
   - **NÃO VERIFICADO**: acesso, ferramenta, ambiente ou evidência essencial ausente.

Não diga que o projeto está seguro apenas porque um scanner não encontrou falhas. Declare claramente a cobertura e as limitações da auditoria.
