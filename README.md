# IA (X).SS — All Security Stack

Portão de segurança reutilizável para projetos desenvolvidos ou mantidos com agentes de IA. Atua durante mudanças sensíveis e antes de qualquer deploy, produzindo evidências e um resultado objetivo: **APROVADO**, **APROVADO COM RESSALVAS**, **REPROVADO** ou **NÃO VERIFICADO**.

Esta ferramenta reduz falhas comuns; ela não substitui pentest profissional, revisão humana especializada nem programa contínuo de segurança.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## O que acompanha o repositório

- `SKILL.md`: fonte principal, compatível com o padrão aberto Agent Skills.
- `references/security-checklist.md`: checklist completo por camada e stack.
- `references/scanner-playbook.md`: uso seguro de Gitleaks, Bandit, Opengrep e ZAP.
- `references/report-template.md`: formato obrigatório do relatório.
- `prompts/UNIVERSAL_PROMPT.md`: versão para qualquer IA que aceite texto ou arquivos.
- `AGENTS.md`: instrução persistente para Codex e agentes compatíveis.
- `CLAUDE.md`: instrução persistente para Claude Code.
- `.cursor/rules/ia-xss-all-security-stack.mdc`: regra de projeto do Cursor.
- `.github/copilot-instructions.md`: instrução de repositório do GitHub Copilot.

## Regra principal

Rodar um checkpoint após mudanças em autenticação, autorização, banco, APIs, uploads, pagamentos, segredos ou infraestrutura. Antes de concluir ou publicar qualquer projeto, executar o portão final. Não declarar aprovação com achado crítico/alto aberto nem quando faltar evidência indispensável.

## 1. Usar no Hermes Agent

O Hermes é compatível com o padrão Agent Skills. As skills do usuário ficam em `~/.hermes/skills/`.

### Instalação no Linux, macOS ou WSL

Como este repositório é público, qualquer pessoa pode instalá-lo diretamente:

```bash
git clone https://github.com/galvao2121-sudo/ia-xss-all-security-stack.git \
  ~/.hermes/skills/ia-xss-all-security-stack
```

### Instalação no Windows PowerShell

```powershell
git clone https://github.com/galvao2121-sudo/ia-xss-all-security-stack.git `
  "$env:USERPROFILE\.hermes\skills\ia-xss-all-security-stack"
```

Não copie apenas o `SKILL.md`: a skill usa os arquivos da pasta `references/`.

### Como chamar

No Hermes CLI ou numa conversa conectada ao Hermes:

```text
/ia-xss-all-security-stack
```

Ou peça em linguagem natural:

```text
Use IA (X).SS — All Security Stack para auditar este projeto antes do deploy.
Corrija os achados dentro do workspace, rode novamente os testes e não aprove
se houver risco crítico ou alto aberto.
```

## 2. Usar no ChatGPT/Codex

Na conta em que a skill foi instalada, mencione:

```text
$ia-xss-all-security-stack
```

Exemplo:

```text
Use $ia-xss-all-security-stack durante esta implementação e execute o portão
final antes de considerar o projeto concluído.
```

Para um repositório usado pelo Codex CLI, copie ou mescle a seção de `AGENTS.md` no `AGENTS.md` do projeto. Não apague instruções já existentes.

## 3. Usar no Claude Code

Copie ou mescle o conteúdo de `CLAUDE.md` no `CLAUDE.md` da raiz do projeto. Mantenha esta pasta em um caminho acessível ao agente ou copie também `SKILL.md` e `references/` para o projeto.

Prompt recomendado:

```text
Leia e aplique IA (X).SS — All Security Stack. Faça um checkpoint agora e o
portão completo ao final. Mostre arquivo/linha, gravidade, impacto, correção e
teste de verificação. Não execute teste ativo em produção sem autorização.
```

## 4. Usar no Cursor

Copie o arquivo:

```text
.cursor/rules/ia-xss-all-security-stack.mdc
```

para a mesma pasta no projeto de destino. Mantenha também `SKILL.md` e `references/` acessíveis na raiz ou ajuste o caminho indicado pela regra. O Cursor também pode ler `AGENTS.md` e `CLAUDE.md`.

## 5. Usar no GitHub Copilot

Mescle `.github/copilot-instructions.md` com o arquivo de mesmo nome no projeto. Não substitua instruções existentes sem revisá-las. Copie `SKILL.md` e `references/` para a raiz do projeto ou ajuste a referência.

## 6. Usar em qualquer outra IA

1. Anexe `SKILL.md` e a pasta `references/`, se a plataforma aceitar arquivos.
2. Cole `prompts/UNIVERSAL_PROMPT.md` como instrução do projeto ou no início da conversa.
3. Dê acesso somente leitura ao código para auditoria; autorize escrita apenas quando quiser correções.
4. Informe se o alvo é local, staging ou produção.
5. Nunca envie chaves, senhas ou tokens pelo chat.

Se a IA não puder executar comandos ou ler todo o código, o resultado deve ser **NÃO VERIFICADO**, nunca **APROVADO**.

## Quando rodar

### Durante o projeto

```text
Execute um checkpoint IA (X).SS somente nas mudanças desta etapa. Corrija os
achados críticos/altos dentro do escopo e repita os testes afetados.
```

### Antes do deploy

```text
Execute o portão final IA (X).SS em todo o projeto. Revise as cinco portas
críticas, rode os scanners aplicáveis e entregue o relatório completo.
```

### Somente auditoria, sem alterações

```text
Faça uma auditoria IA (X).SS somente leitura. Não modifique arquivos. Liste os
achados com evidência e plano de correção.
```

## Como interpretar o resultado

- **APROVADO:** controles críticos verificados e nenhum crítico/alto aberto.
- **APROVADO COM RESSALVAS:** somente riscos médios/baixos aceitos e documentados.
- **REPROVADO:** há crítico/alto aberto ou controle essencial ausente.
- **NÃO VERIFICADO:** faltou acesso, ambiente, ferramenta ou evidência essencial.

## Limites obrigatórios

- Não executar ZAP Active Scan, brute force, fuzzing destrutivo ou DoS em produção sem autorização explícita.
- Não mostrar segredos encontrados; redigir o valor e orientar revogação/rotação.
- Não tratar “zero achados” de scanner como garantia de segurança.
- Não alterar infraestrutura fora do projeto sem autorização.

## Comunidade e atribuição

Este projeto é distribuído sob a [licença MIT](LICENSE). É permitido usar,
copiar, modificar e compartilhar, inclusive em projetos comerciais, desde que
o aviso de copyright e a licença sejam preservados.

O GitHub e a licença MIT não notificam automaticamente o autor quando alguém
usa esta skill. Se ela ajudou no seu projeto, considere deixar uma estrela,
abrir uma Discussion ou contar como foi utilizada. Esse contato é voluntário.

Ao adaptar ou redistribuir, uma atribuição como esta é bem-vinda:

```text
Baseado em IA (X).SS — All Security Stack,
por Galvão / LVM Tech — https://github.com/galvao2121-sudo/ia-xss-all-security-stack
```

## Licença

Copyright (c) 2026 Galvão / LVM Tech. Consulte [LICENSE](LICENSE).

## Atualização

Atualize sempre a fonte principal e depois replique somente as instruções curtas dos adaptadores. Evite criar checklists independentes que possam divergir do `SKILL.md` e de `references/security-checklist.md`.
