# Checklist de segurança

Use somente as seções aplicáveis e marque cada item como `PASSOU`, `FALHOU`, `NÃO APLICÁVEL` ou `NÃO EXECUTADO`.

## Sumário

1. Arquitetura e superfície
2. Autenticação e sessão
3. Autorização, IDOR/BOLA e multi-tenant
4. Supabase
5. Firebase
6. APIs e backend
7. Frontend e browser
8. Segredos e configuração
9. Entradas, uploads e conteúdo
10. Dependências, infraestrutura e observabilidade
11. Testes mínimos do portão final

## 1. Arquitetura e superfície

- Identificar entradas públicas, APIs, banco, armazenamento, filas, webhooks e serviços externos.
- Classificar dados pessoais, financeiros, credenciais, tokens e dados de outros tenants.
- Identificar papéis e ações privilegiadas.
- Registrar quais controles ficam no cliente, servidor, banco e provedor.

## 2. Autenticação e sessão

- Validar tokens no servidor: assinatura, emissor, audiência, expiração e algoritmo esperado.
- Não confiar em ID de usuário, papel ou tenant enviado pelo cliente.
- Aplicar expiração, revogação e rotação adequadas.
- Usar cookies `Secure`, `HttpOnly` e `SameSite` quando a autenticação for por cookie.
- Proteger mudança de senha, recuperação, MFA e vinculação de conta contra enumeração e abuso.
- Invalidar sessões quando necessário e não registrar tokens em logs.

## 3. Autorização, IDOR/BOLA e multi-tenant

- Revalidar permissão no servidor/camada de dados em toda ação privilegiada.
- Filtrar por `owner_id`/`tenant_id` derivado da identidade autenticada, não do corpo da requisição.
- Em busca por ID, combinar ID + proprietário/tenant na consulta sempre que possível.
- Testar usuário A lendo, alterando e excluindo objeto do usuário B.
- Testar usuário comum chamando endpoints de admin diretamente.
- Negar por padrão e aplicar menor privilégio.

## 4. Supabase

- Inventariar tabelas, views, funções RPC e buckets alcançáveis pelo cliente.
- Habilitar RLS em toda tabela exposta e criar policies explícitas para `SELECT`, `INSERT`, `UPDATE` e `DELETE` conforme necessário.
- Verificar `WITH CHECK`, não somente `USING`, para impedir escrita em nome de outro usuário/tenant.
- Testar anon, autenticado A, autenticado B e papel de serviço.
- Manter `service_role` somente em backend confiável; lembrar que ele pode ignorar RLS.
- Revisar funções `SECURITY DEFINER`, `search_path`, grants, views e Storage policies.
- Não considerar apenas `ENABLE ROW LEVEL SECURITY` como conclusão: confirmar comportamento das policies.

## 5. Firebase

- Tratar Firestore/Realtime Database/Storage com Security Rules específicas; Firebase não usa RLS do Postgres.
- Negar por padrão e permitir somente documentos/caminhos do usuário ou tenant correto.
- Validar campos, tipos, imutabilidade de owner/tenant e limites de consulta/escrita.
- Executar testes com Emulator Suite para acesso autorizado e cruzado.
- Manter credenciais administrativas e Admin SDK somente em ambiente confiável.

## 6. APIs e backend

- Exigir autenticação nas rotas privadas e autorização por recurso/ação.
- Validar entrada no servidor com schema e rejeitar campos desconhecidos quando apropriado.
- Usar consultas parametrizadas ou ORM sem concatenação de entrada.
- Aplicar rate limit em login, recuperação, códigos, resgate, pagamentos, busca cara e webhooks.
- Limitar paginação, tamanho de corpo, tempo de execução e complexidade.
- Validar assinatura, timestamp e replay de webhooks.
- Evitar mass assignment, retorno excessivo de campos e mensagens de erro internas.
- Revisar CORS por origem/método/header; nunca combinar origem ampla com credenciais.
- Proteger endpoints por cookie contra CSRF quando aplicável.

## 7. Frontend e browser

- Não usar o frontend como autoridade para papel, preço, desconto, dono ou estado de pagamento.
- Confirmar que somente variáveis deliberadamente públicas entram no bundle.
- Evitar renderização insegura de HTML; codificar saída por contexto.
- Revisar URLs, redirects, `postMessage`, armazenamento local e exposição em source maps.
- Aplicar CSP e headers adequados no deploy, considerando compatibilidade.

## 8. Segredos e configuração

- Manter `.env*` sensíveis fora do Git e fornecer somente `.env.example` sem valores reais.
- Escanear working tree, arquivos rastreados e histórico Git.
- Revisar padrões públicos do framework (`NEXT_PUBLIC_`, `VITE_`, `PUBLIC_` etc.).
- Separar chaves de desenvolvimento, staging e produção.
- Rotacionar qualquer segredo confirmado como exposto e investigar uso indevido.
- Aplicar menor privilégio e prazo de validade nas credenciais.

## 9. Entradas, uploads e conteúdo

- Validar tipo, formato, tamanho, faixa e comprimento no servidor.
- Para upload, verificar magic bytes/MIME real, extensão, tamanho, quantidade e nome gerado pelo servidor.
- Armazenar fora da raiz executável; impedir path traversal e execução de arquivo.
- Considerar varredura de malware e processamento isolado para arquivos não confiáveis.
- Bloquear URLs internas/metadados em funcionalidades que buscam URLs fornecidas pelo usuário (SSRF).
- Usar allowlist para destinos e normalizar caminhos com cuidado.

## 10. Dependências, infraestrutura e observabilidade

- Rodar auditoria de dependências compatível com lockfile e revisar severidade/explorabilidade.
- Remover dependências abandonadas/desnecessárias e fixar versões de CI/actions quando apropriado.
- Confirmar modo produção, TLS, headers, permissões, portas e serviços expostos.
- Não incluir segredos em imagem Docker, camada de build, logs ou artefatos.
- Aplicar logs de auditoria para ações privilegiadas sem registrar dados sensíveis.
- Ter backup e restauração testáveis para dados importantes.

## 11. Testes mínimos do portão final

- Testes automatizados do projeto passaram.
- Gitleaks ou revisão equivalente cobriu conteúdo atual e histórico.
- SAST adequado à stack foi executado ou marcado como não executado com motivo.
- Testes de autorização cruzada cobriram usuário A/B e admin/comum quando aplicável.
- RLS/Security Rules foram verificadas por comportamento quando aplicável.
- DAST em local/staging foi executado quando houver alvo web e autorização; caso contrário, justificar.
- Nenhum crítico/alto está aberto antes de `APROVADO`.
