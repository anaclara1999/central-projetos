# Central de Projetos — versão pessoal

Guia de configuração: GitHub Pages, login com Google e salvamento no seu Drive pessoal.

Configuração feita uma única vez. As telas do Google Cloud e do GitHub mudam de aparência de tempos em tempos — se um nome ou botão não estiver exatamente como aqui, procure a opção equivalente mais próxima. O arquivo não contém nenhuma credencial: você mesma insere o seu Client ID na Parte 3.

## Parte 1 — Preparar o GitHub

1. Criar uma conta pessoal no GitHub, gratuita, se ainda não tiver.
2. Criar um repositório novo — pode ser público, o arquivo não contém informação privada; os dados de uso ficam só no seu Google Drive.
3. Enviar o arquivo `central-projetos.html` para o repositório, renomeando-o para `index.html`.
4. Ativar o GitHub Pages em Settings > Pages, branch `main`, pasta raiz.
5. Copiar a URL gerada (formato `https://SEUUSUARIO.github.io/NOMEDOREPOSITORIO/`) — usada na Parte 2.

## Parte 2 — Configurar o Google Cloud

Hoje a configuração fica dentro do **Google Auth Platform**, dividido em Branding, Audience, Clients e Data Access (nomes antigos: "Tela de consentimento OAuth").

1. Criar um projeto novo em console.cloud.google.com.
2. Ativar a API do Google Drive (APIs e Serviços > Biblioteca > Google Drive API > Ativar).
3. Branding: preencher nome do app e e-mail de suporte.
4. Audience: escolher **External**, Publishing status **Testing** (sem verificação do Google — uso pessoal).
5. Audience > Test users: adicionar seu próprio e-mail do Google.
6. Data Access: confirmar o escopo `https://www.googleapis.com/auth/drive.file` (acesso só aos arquivos criados pelo próprio app, nunca ao Drive inteiro).
7. Clients: criar um Client ID, tipo **Web application**.

**Atenção — origem JavaScript autorizada:** cadastrar somente protocolo + domínio, sem caminho.
Exemplo: se a URL for `https://usuario.github.io/central-projetos-pessoal/`, a origem cadastrada deve ser somente `https://usuario.github.io` — sem `/central-projetos-pessoal` e sem barra final.

8. Copiar o Client ID gerado (termina em `.apps.googleusercontent.com`).

## Parte 3 — Configurar o arquivo

1. Abrir o `index.html`.
2. Localizar `const GOOGLE_CLIENT_ID = "COLE_AQUI_SEU_CLIENT_ID";`.
3. Substituir `COLE_AQUI_SEU_CLIENT_ID` pelo Client ID completo copiado na Parte 2, mantendo as aspas.
4. Salvar e subir a versão atualizada para o GitHub (substituindo o `index.html` anterior).

## Parte 4 — Primeiro teste

1. Abrir o endereço do GitHub Pages.
2. Entrar com Google, usando a conta de teste da Parte 2.
3. Na primeira vez, o Google pode avisar "app não verificado" — clique em Avançado > Acessar (não seguro). Normal para app pessoal em modo Testing.
4. Criar um projeto de teste e uma anotação de teste; confirmar que o autosave funciona.
5. Fechar e abrir a página novamente — confirmar que os dados continuam lá.

## Parte 5 — Teste em outro aparelho

Abrir o mesmo endereço em outro computador ou celular, entrar com a mesma conta Google, e confirmar que os dados aparecem automaticamente.

## Parte 6 — Backup

Exportar backup (.json) e testar a importação. Os dados do dia a dia ficam automaticamente em `central-projetos-dados.json`, no seu Google Drive pessoal — o backup é uma cópia à parte.

## Parte 7 — Como substituir o `index.html` sem perder dados

O arquivo HTML no GitHub Pages não guarda nenhum dado — ele é só a aplicação. Todos os seus projetos, tarefas, anotações etc. ficam em `central-projetos-dados.json`, no seu Google Drive. Por isso, atualizar a versão do app é sempre seguro:

1. Baixar um backup (.json) pela tela **Dados e Backup**, por precaução.
2. No GitHub, substituir o conteúdo do `index.html` atual pelo do novo `central-projetos.html` (mantendo o nome `index.html`).
3. Não é preciso alterar o `GOOGLE_CLIENT_ID` nem repetir a Parte 2. Se você quiser usar as contas Google adicionais (Drive e/ou Agenda) desta rodada, siga também a Parte 8 — quem não precisar de contas adicionais não precisa fazer nada além do passo 2 acima.
4. Abrir o endereço do GitHub Pages normalmente e entrar com a mesma conta Google — os dados aparecem automaticamente, pois continuam no mesmo arquivo do Drive.

## Parte 8 — Contas Google adicionais: Google Drive e Google Agenda (Fase 2)

Só é necessária se você quiser conectar contas Google adicionais (pessoal, profissional, outras) em Configurações > Contas Google, para usar Google Drive e/ou Google Agenda dessas contas a partir da Central. Sem isso, a conta de dados da Central (Parte 2) continua funcionando normalmente, sem qualquer mudança.

1. No mesmo projeto do Google Cloud usado na Parte 2, ativar a API do Google Calendar (APIs e Serviços > Biblioteca > Google Calendar API > Ativar).
2. Google Auth Platform > Data Access: adicionar os escopos:
   - `https://www.googleapis.com/auth/calendar.readonly` — leitura da Google Agenda; a Central nunca cria, edita nem apaga eventos.
   - `https://www.googleapis.com/auth/userinfo.email` e `openid` — usados só para identificar de qual conta se trata, ao conectar.
3. Audience > Test users: adicionar, uma a uma, **todas** as contas Google que você pretende conectar como adicionais — não só a conta de dados da Central. Enquanto o app estiver em modo Testing (uso pessoal, sem verificação do Google), uma conta que não estiver cadastrada como usuário de teste não consegue concluir a conexão; ela aparece com status "Erro ao conectar" na tela Contas Google.
4. Não é preciso criar um novo Client ID nem repetir a Parte 2 inteira — é o mesmo Client ID e a mesma tela de consentimento, só com escopos adicionais liberados.

**Atenção:** o escopo de Google Drive pedido às contas adicionais é o mesmo da conta de dados da Central (`drive.file` — acesso só aos arquivos criados pelo próprio app). Nenhuma conta recebe acesso ao Google Drive inteiro.

## Parte 9 — Google Picker (Documentos por projeto)

Só é necessária se você quiser usar o botão "Selecionar do Google Drive" na aba Documentos de um projeto, ou escolher a pasta do Drive de um projeto. Sem isso, a aba Documentos continua funcionando normalmente pelo formulário manual (nome + link colado à mão).

1. No mesmo projeto do Google Cloud usado na Parte 2, ir em APIs e Serviços > Biblioteca, procurar **Google Picker API** e clicar em Ativar.
2. Ir em APIs e Serviços > Credenciais > Criar credenciais > **Chave de API**.
3. Clicar em "Restringir chave" (recomendado, evita uso indevido da chave por outro site):
   - Restrições de aplicativo: **Referenciadores HTTP (sites)** — adicionar a URL do seu GitHub Pages, no formato `https://SEUUSUARIO.github.io/*` (com o `/*` no final, para cobrir qualquer caminho dentro do seu usuário).
   - Restrições de API: escolher **Restringir chave**, marcar **Google Picker API**.
4. Copiar a chave gerada.
5. Abrir o `index.html`, localizar `const GOOGLE_PICKER_API_KEY = "COLE_AQUI_SUA_API_KEY";` e substituir pela chave copiada, mantendo as aspas.
6. Salvar e subir a versão atualizada para o GitHub.

**Como funciona:** o Picker abre o Drive da conta que você escolher (a conta de dados da Central, ou qualquer conta adicional conectada com Google Drive ativo — ver Parte 8) e devolve só o link do arquivo ou pasta escolhida; a Central nunca baixa nem guarda o conteúdo do arquivo em si, só o link para abri-lo depois. Combinado com o escopo `drive.file` já usado em todo o app, o Picker é o mecanismo que o próprio Google recomenda para dar acesso a um arquivo específico sem pedir acesso ao Drive inteiro.

## Novidades desta rodada (Fase 1)

Esta rodada tornou a aplicação administrável, sem exigir nenhuma configuração adicional no Google Cloud:

- **Configurações** (novo item no menu lateral), com sub-abas Geral, Empresas, Pessoas, Listas e Categorias, Contas Google e Identidade Visual.
- **Empresas**: cadastro próprio (nenhuma empresa vem pré-cadastrada); projetos podem ser vinculados a uma empresa ou ficar como "Pessoal / Sem empresa".
- **Pessoas**: substitui a lista fixa anterior — cadastre quem pode ser Responsável e/ou Sponsor, com opção de cadastrar uma pessoa nova direto de qualquer formulário.
- **Listas e Categorias configuráveis**: status, prioridades, tipos e classificações (projetos, tarefas, decisões, pendências, riscos, marcos, materiais, anotações) agora podem ser renomeados, reordenados, ativados/desativados — sem quebrar os registros já existentes (o identificador interno de cada item não muda quando você renomeia).
- **Decisões, Pendências e Riscos**: agora têm formulário completo de criação e edição (Riscos não tinha nenhuma criação manual antes — só existia via revisão de reunião por IA).
- **Identidade visual**: nome do app, subtítulo e logo (PNG/JPG/WebP) editáveis em Configurações — refletem na barra lateral, no título da aba do navegador e nos documentos Word exportados.
- **Importar Word**: na tela Anotações (geral e dentro de cada projeto), é possível importar um `.docx` — a conversão acontece inteiramente no navegador, via a biblioteca Mammoth.js, carregada de `cdnjs.cloudflare.com` (mesmo CDN já usado para React e JSZip). O conteúdo entra como uma anotação (tipo Anotação, Ata ou Transcrição); o arquivo original não é enviado nem armazenado em lugar nenhum.

## Novidades desta rodada (Fase 2)

Esta rodada conecta contas Google adicionais para Google Drive e Google Agenda, mantendo a conta de dados da Central (Parte 2) totalmente separada e intocada — ela continua funcionando exatamente como antes.

- **Contas Google conectadas** (Configurações > Contas Google): conecte quantas contas quiser (pessoal, profissional, outras), escolhendo para cada uma se ela será usada para Google Drive e/ou Google Agenda. Cada conta guarda apenas e-mail, apelido, quais serviços usa, quais calendários foram escolhidos e a data da última sincronização — nunca senha nem token de acesso. Os tokens ficam só na memória da sessão aberta no navegador e somem ao fechar a página; por isso é normal precisar clicar em "Reconectar" a cada nova sessão.
- **Google Agenda de várias contas**: em Configurações > Contas Google, escolha quais calendários de cada conta entram na Central e use "Sincronizar agenda agora" (todas as contas) ou "Sincronizar esta conta". Os eventos aparecem no Calendário (Planejamento > Calendário) junto com tarefas, marcos e reuniões, com filtros por Fonte, Conta, Calendário e Projeto.
- **Detalhe do evento do Google Agenda**: clicar em um evento do Google mostra data, horário, conta, calendário, organizador, participantes, local e descrição, além de links para abrir no Google Agenda e no Google Meet (quando houver). Duas ações ficam disponíveis: vincular o evento a um projeto da Central, e importar o evento como reunião (usando título, data e horário do próprio evento) — importar o mesmo evento de novo não duplica a reunião.
- **Isolamento entre contas:** uma falha ao conectar, reconectar ou sincronizar uma conta adicional nunca afeta a conta de dados da Central nem as outras contas conectadas — cada conta tem seu próprio status e sua própria mensagem de erro, quando houver.

## Novidades desta rodada (Fase 3)

- **Documentos** (antes "Materiais"): botão único "Adicionar documento" que abre três formas de trazer um documento — **Link** (colar um link qualquer, como antes), **Upload da máquina** (envia o arquivo de verdade para o Google Drive da conta escolhida, limite de 10MB) e **Google Drive** (Picker, ver Parte 9, para escolher um arquivo já existente no Drive de qualquer conta conectada). Nos três casos a Central guarda só o link resultante, nunca o arquivo em si.
- **Pasta do projeto no Drive**: em Visão Geral, cada projeto pode ter uma pasta do Drive associada (escolhida pelo Picker) — um clique abre a pasta, e o Picker de Documentos já começa navegando a partir dela quando a mesma conta é escolhida.
- **Decisões, Pendências, Riscos e Mudanças** deixaram de ser abas próprias do projeto — agora aparecem como categorias dentro da própria Visão Geral, junto com o resumo do projeto e o cronograma.
- **Diário** passou a ser a última aba do projeto.
- **Prioridade de Tarefas e Projetos**: passou de lista de texto (Alta/Média/Baixa) para uma escala numérica fixa P0–P3 (P0 = crítica/pra ontem, P3 = sem prioridade), com uma cor por nível.
- **Tarefas**: agora têm edição completa, exclusão, comentários (com opção de colar prints via Ctrl+V) e anexos próprios da tarefa, além de filtro por status, cards de contagem por status em Minha Central, e datas de criação/última mudança de status/conclusão visíveis no detalhe.

## Limitações

O login expira periodicamente; ao acontecer, a aplicação mostra "Sessão Google expirada. Entre novamente para continuar." sem apagar dados. Enquanto o app estiver em modo Testing, só as contas cadastradas como usuário de teste conseguem entrar — comportamento correto para uso pessoal.

**Limitações específicas desta rodada (Fase 2):** as contas Google adicionais (Drive e/ou Agenda) precisam ser reconectadas a cada nova sessão no navegador — os tokens não são persistidos, de propósito, por segurança (ver Parte 8).

**Limitações específicas desta rodada (Fase 3):** Google Meet, atas e transcrições automáticas a partir de reuniões do Google Agenda e exportação em ZIP continuam sendo evoluções planejadas para próximas rodadas e **não estão implementadas nem simuladas** nesta versão. O upload da máquina em Documentos tem limite de 10MB (upload simples) — arquivos maiores devem ser enviados pelo Google Drive Web e trazidos depois pela opção "Google Drive". O Picker de Documentos exige a chave de API da Parte 9 configurada — sem ela, as opções "Google Drive" e "Pasta no Drive" mostram um aviso em vez do seletor. A importação de Word não salva o arquivo `.docx` original — apenas o conteúdo convertido.
