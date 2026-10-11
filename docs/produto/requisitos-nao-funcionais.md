# Requisitos não funcionais

Os não funcionais decidem se a Velqi pode rodar dentro do ambiente do banco. Por isso, segurança e rastreabilidade pesam mais que velocidade.

| ID | Categoria | Requisito |
| --- | --- | --- |
| RNF-01 | Autenticação | Login no console via SSO Azure AD (OIDC + PKCE); os MCPs reaproveitam OAuth2/SAML2, como no sistema de referência |
| RNF-02 | Autorização | Perfis Executor, Aprovador, Admin do catálogo e Leitor; aprovar exige perfil Aprovador da squad |
| RNF-03 | Segredos | Tokens nunca no repositório nem no front; `.env` sempre no `.gitignore`; cofre no backend |
| RNF-04 | Dados sensíveis | Massa (CPF, agência, conta) mascarada em tela e em logs; LGPD |
| RNF-05 | Rede | Funciona atrás de proxy corporativo, CA bundle e VPN; pacotes só do Artifactory |
| RNF-06 | Auditoria | Todo evento (aprovação, guardrail, gate, commit) é guardado com quem, quando, execução e hash do artefato; retenção mínima a definir |
| RNF-07 | Observabilidade | Logs estruturados, traces por execução e RUM no console (Datadog) |
| RNF-08 | Desempenho | Console carrega em até 2 s (LCP) em rede corporativa; status da execução atualiza em tempo real (SSE) |
| RNF-09 | Acessibilidade | Console em conformidade com WCAG 2.2 AA (contraste, teclado, leitor de tela); estados nunca comunicados só por cor |
| RNF-10 | Idioma | Interface em pt-BR; textos externalizados para i18n |
| RNF-11 | Portabilidade | Skills funcionam em Claude Code, Cursor, Copilot, Windsurf e Gemini sem alteração |
| RNF-12 | Qualidade da própria Velqi | Cobertura mínima de 80% no console e no BFF; a Velqi passa pelos próprios quality gates (Sonar) |
| RNF-13 | Determinismo | Scripts (validador, CLI, verify_plan) têm saída determinística, opção `--json` e exit code 0/1 |
| RNF-14 | Versionamento | Cada execução registra a versão do catálogo usada, para que os resultados possam ser reproduzidos |
| RNF-15 | Temas | Console com tema escuro (padrão) e claro, com os mesmos significados de cor nos dois |
