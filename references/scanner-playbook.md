# Playbook de scanners

## Princípios

- Consultar a versão e `--help` antes de compor o comando; interfaces mudam.
- Preferir saída legível por máquina quando disponível, mas resumir sem copiar segredos.
- Salvar relatórios dentro do workspace somente quando isso ajudar o projeto e não contiver credenciais.
- Não instalar ferramenta globalmente nem baixar executável sem necessidade e autorização compatível.
- Scanner complementa revisão manual; zero achados não significa zero vulnerabilidades.

## Gitleaks

Usar para segredos no estado atual e histórico Git. Confirmar na ajuda da versão como cobrir ambos. Ativar redaction quando disponível. Não exibir o segredo encontrado. Um segredo real no histórico continua comprometido mesmo depois de removido do commit mais recente: revogar/rotacionar e tratar a limpeza do histórico como operação separada, coordenada e potencialmente destrutiva.

## Bandit

Usar somente para Python. Direcionar ao código da aplicação e excluir ambientes virtuais, dependências vendorizadas, builds e fixtures irrelevantes. Avaliar contexto: um alerta pode ser falso positivo, mas toda supressão precisa de justificativa próxima ao código ou no relatório.

## Opengrep

Usar como SAST multi-linguagem. Escolher regras compatíveis com a stack e registrar o conjunto de regras. Não executar correção automática em massa sem revisar o diff. Confirmar achados no fluxo real antes de classificá-los como exploráveis.

## OWASP ZAP

Começar com Baseline/passive scan contra local ou staging. Confirmar host, escopo e dados descartáveis. Autenticação pode exigir contexto/script para alcançar rotas privadas; registrar cobertura real. Active Scan pode alterar estado e gerar carga: nunca executar em produção sem autorização explícita. ZAP avalia a aplicação web acessível; não substitui inventário de rede/portas nem revisão de código.

## Auditoria de dependências

Usar a ferramenta correspondente ao lockfile/ecossistema, evitando atualizar dependências durante uma auditoria somente leitura. Separar vulnerabilidade presente de vulnerabilidade explorável no caminho usado pelo projeto. Não ignorar pacote transitivo crítico; documentar mitigação e versão corrigida.

## Fallback manual

Quando uma CLI não estiver disponível:

1. Procurar padrões de segredos e variáveis públicas com `rg`, sem imprimir valores completos.
2. Inspecionar autenticação, autorização, consultas por ID, validação e uploads por fluxo.
3. Inspecionar migrações/policies/Rules e testes correspondentes.
4. Marcar o scanner como `NÃO EXECUTADO`, registrar o motivo e fornecer o comando recomendado após confirmar a documentação oficial.
