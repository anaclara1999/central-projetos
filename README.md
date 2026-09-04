# Central de Projetos (versão pessoal)

Aplicativo pessoal e portátil de gestão de projetos da Clara, independente do CF. HTML único (React 18 via CDN, sem build step), com login Google e persistência no Google Drive pessoal de quem está usando.

## Estrutura

- `index.html` — a aplicação completa. Fica com esse nome direto no repositório, pronta para o GitHub Pages (não precisa mais renomear na hora de subir).
- `GUIA-CONFIGURACAO.md` — guia de configuração (GitHub Pages, Google Cloud, primeiro uso, contas Google adicionais).
- `guia-configuracao-central-projetos.docx` — mesmo guia em Word.
- `CLAUDE.md` — contexto técnico do projeto para o Claude Code.

## Publicar / atualizar no GitHub Pages

1. Configuração inicial (uma única vez): seguir `GUIA-CONFIGURACAO.md`.
2. Depois disso, qualquer alteração no `index.html` só precisa de commit + push — o GitHub Pages atualiza sozinho em alguns minutos.

## Testar localmente

Abrir `index.html` direto no navegador já funciona para a maior parte dos testes (login Google incluído, contanto que a origem esteja autorizada — ver guia). Se quiser um servidor local, a extensão Live Server do VS Code funciona sem configuração extra.

## Convenções deste projeto

- Sem backend próprio: tudo roda no navegador.
- Nunca persistir token de acesso, refresh token ou client secret — só em memória durante a sessão.
- Ver `CLAUDE.md` para o contexto completo (arquitetura, decisões e regras) usado pelo Claude ao trabalhar neste projeto, e o Claude Project "Automações - Lovable & N8N" para o histórico detalhado de cada rodada.
