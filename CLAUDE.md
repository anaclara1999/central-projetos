# Central de Projetos — contexto para o Claude Code

Este repositório é a versão pessoal e portátil da "Central de Projetos" da Clara — independente da conta/infraestrutura do CF, pensada para poder ser levada e reaproveitada em outra empresa se necessário. É uma ramificação separada de uma versão publicada como Artifact do Claude (essa outra versão contém dados de exemplo do CF e não deve ser confundida com este repositório).

## O que é

Aplicativo de gestão de projetos: React 18 (UMD, via CDN) + Babel Standalone (JSX compilado no navegador) + JSZip + Google Identity Services, tudo dentro de um único arquivo `index.html`. Sem build step, sem backend próprio, sem framework de componentização além do React puro.

## Persistência

- Login com Google (OAuth via Google Identity Services) para a "conta de dados da Central", escopo `drive.file` (só arquivos criados pelo próprio app, nunca o Drive inteiro).
- Todos os dados de uso ficam num único arquivo `central-projetos-dados.json`, no Google Drive pessoal de quem está usando.
- Autosave: a gravação (`scheduleDriveSave`) só é considerada concluída depois de uma resposta HTTP real (`response.ok`) do Google Drive — nunca "dispare e esqueça".
- Backup/restauração via `.json` (formato 2), e botão "Exportar tudo e limpar dados" (só limpa depois que o backup é confirmado como baixado).

## Contas Google adicionais e Google Agenda

Além da conta de dados acima, é possível conectar contas Google adicionais (pessoal, profissional, outras) para Google Drive e/ou Google Agenda. Cada conta tem seu próprio token client e access token, guardados só em memória (nunca em `localStorage`, no backup ou no arquivo de dados). O que é persistido é só metadado não sensível: e-mail, apelido, quais serviços usa, calendários escolhidos, última sincronização. Ver `GUIA-CONFIGURACAO.md` (Parte 8) para a configuração adicional no Google Cloud (API do Calendar, escopos, usuários de teste).

## Regras que não podem ser quebradas

- Nunca introduzir backend próprio (Supabase, Firebase, n8n, Vercel Functions, servidor ou banco externo).
- Nunca persistir token de acesso, refresh token, client secret ou senha em `localStorage`, no arquivo `.json`, no código-fonte ou no backup — só em memória, durante a sessão.
- Uma falha ao conectar/sincronizar uma conta Google adicional nunca pode afetar a conta de dados da Central nem as outras contas conectadas — isolar por conta, sempre.
- Cada item de lista configurável (status, prioridade, tipo etc.) tem um identificador interno estável, separado do texto exibido — a lógica de negócio compara sempre pelo identificador interno, nunca pelo texto exibido, para que renomear um item não quebre nada.
- Sessão do Google expirada mostra exatamente "Sessão Google expirada. Entre novamente para continuar." — sem apagar nenhum dado.
- Preferir Google Picker com escopo `drive.file` a acesso total ao Drive quando o Picker for implementado; usar sempre o escopo mínimo necessário (Google Agenda em modo leitura, `calendar.readonly`).
- Não simular integrações que não estejam realmente funcionando — se algo não foi implementado, dizer isso claramente em vez de fingir.

## Antes de considerar uma alteração pronta

Extrair o bloco `<script type="text/babel" data-presets="react">...</script>` do `index.html` e rodar `esbuild.transformSync` (`loader: 'jsx', jsx: 'transform', jsxFactory: 'React.createElement', jsxFragment: 'React.Fragment', target: 'es2020'`) para checar erro de sintaxe antes de dar a alteração como concluída.

## Identidade visual (CF)

Azul-marinho `#102850`, laranja `#FF9933`, cinza `#727376`, branco-claro `#F1F1F1` — mantidos mesmo nesta ramificação pessoal (só os nomes internos dos tokens de cor foram trocados, ex. `--primary`/`--accent`/`--neutral`/`--bg-soft`, sem mudar os valores hexadecimais).

## Onde está o histórico completo

O histórico detalhado de cada rodada de ajustes (o que foi pedido, decidido e entregue, rodada a rodada) fica no Claude Project "Automações - Lovable & N8N", no documento `central-projetos-prototipo.md` — de propósito não duplicado aqui, para não haver duas fontes de verdade. Ao trabalhar numa alteração relevante, vale conferir esse documento antes de propor mudanças de arquitetura.
