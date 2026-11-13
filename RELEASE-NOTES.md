# Orkestrix 4.1.22: notas da versão

Data: 13/11/2026. Mudam a **tela Processos** (grupos), o **layout** (responsivo em todas as telas) e o **editor** (paleta, linha de comando e teste). **Há uma migração de banco nova**: `4.1.22_01` (coluna do grupo do processo). Só acrescenta; a versão anterior ignora a coluna.

- **Grupos de processos.** Na tela Processos, **Novo grupo** (ou o botão do processo) reúne vários processos sob um nome, por exemplo `whats` com todos os `whats_*`. A lista mostra uma seção por grupo, que recolhe e abre. O grupo não muda o tipo nem a pasta da biblioteca.
- **Responsivo.** O menu vira gaveta no celular; botões e campos ficam maiores para toque; os diálogos ocupam a tela inteira em tela pequena e podem ser **redimensionados arrastando** o canto em tela grande. Tabelas escondem colunas secundárias e rolam dentro do cartão.
- **Ações das linhas.** Em telas médias, a linha mostra **Iniciar** e um menu «⋯» (editar, execuções, grupo, arquivar); a partir de 1280 px os botões completos voltam. Vale para Processos (linhas e versões), Cofre e Dados.
- **Editor.** A **paleta** e a **linha de comando** podem ser recolhidas (no celular nascem recolhidas; a escolha fica salva). O painel lateral reduzido vira uma barra pequena. O painel **Teste** liga à tela Execuções (a execução aberta e a lista de testes do processo).
- **Biblioteca.** «Nova pasta» permite cadastrar um tipo de processo novo (só ADMIN) sem sair do diálogo; em tela estreita o painel de pastas fica atrás do botão «Pastas e ações».
- **Atenção:** a lista de Processos agrupa os processos que tiverem grupo; os sem grupo aparecem em «Sem grupo». Nenhum dado existente muda.
- Imagens no Docker Hub: `wendercorrea/orkestrix:backend-4.1.22`, `frontend-4.1.22` e `gateway-4.1.22`, com `upgrade.sh --pull` como na 4.1.3.

---

## Versão anterior: 4.1.21: notas da versão

Data: 13/11/2026. Mudam as **execuções** (número da execução separado da revisão) e a **ofuscação no log** (campos escolhidos em consulta, chamada REST e transformação). **Não há migração de banco nova.**

- **Número da execução.** A lista de Execuções mostra a coluna **Execução** (#1, #2…): a posição da instância entre as do mesmo processo. Fica separada da **Revisão** (vN, a versão do processo) e do identificador da instância.
- **Ofuscar campos no log.** Em «Consultar banco», chamada REST (POST, GET, PUT, PATCH, DELETE) e «Mapear e transformar», escreva os nomes de campo a ocultar (`cpf, email`). Eles saem como `***` no log de execução, em qualquer nível do JSON (entrada e saída do nó, corpo e resposta da chamada, linhas da consulta).
- **Atenção:** campos com nome de segredo (`token`, `senha`, `api_key`, `authorization`) continuam ocultados sempre. Um valor que aparece no caminho da URL (ex.: `/clientes/{cpf}`) não é ocultado pelo nome do campo.
- Imagens no Docker Hub: `wendercorrea/orkestrix:backend-4.1.21`, `frontend-4.1.21` e `gateway-4.1.21`, com `upgrade.sh --pull` como na 4.1.3.

---

## Versão anterior: 4.1.20: notas da versão

Data: 13/11/2026. Mudam a **biblioteca** (tipos de processo cadastrados), a **tela Dados** (categoria, visibilidade e filtros), os **logs** (mascaramento da consulta e da chamada REST), o **editor** (subprocesso no gerador de BPMN) e a **tela de Execuções** (link a partir de Processos). **Há migrações de banco novas**: `4.1.20_01` (tipos de processo) e `4.1.20_02` (categoria e visibilidade das tabelas). Só acrescentam; a versão anterior ignora as tabelas novas.

- **Tipos de processo cadastrados.** Os tipos deixam de ser fixos no código. Só o perfil **ADMIN** cadastra, renomeia e apaga um tipo; apagar exige tipo sem pastas nem processos (409). Os três tipos atuais continuam com os mesmos códigos.
- **Tabelas de dados: categoria e visibilidade.** No nó «Gravar no banco», escolha a categoria e os perfis que veem a tabela. Vale quando a tabela é **criada**. A tela Dados filtra por nome, categoria e linhas, e só mostra as tabelas que o perfil vê (o ADMIN vê todas). A consulta da tela Dados também respeita a visibilidade.
- **Mascarar no log.** Nova opção **Mascarar no log** no «Consultar banco» (esconde resultado e erros) e na chamada REST (esconde URL, cabeçalhos, corpo, resposta e erro, mostrando `***`). Não altera o que o fluxo usa.
- **Execuções a partir de Processos.** Cada processo, e cada versão, tem um botão «Execuções» que abre a lista já filtrada. A lista de Execuções abre na janela dos últimos 20 minutos.
- **Gerador de BPMN (skill).** Suporta subprocesso embutido e as opções novas acima; o validador exige Java 21 em `JAVA_HOME`.
- **Atenção:** tabelas criadas antes da 4.1.20 ficam sem categoria e visíveis a todos os perfis, como antes. Os processos continuam com o tipo que tinham.
- Imagens no Docker Hub: `wendercorrea/orkestrix:backend-4.1.20`, `frontend-4.1.20` e `gateway-4.1.20`, com `upgrade.sh --pull` como na 4.1.3.

---

## Versão anterior: 4.1.19: notas da versão

Data: 13/11/2026. Mudam a **segurança** (sessão, logs), o **editor de dados** (origem dos dados e exportação CSV) e a **tela de dados**. **Não há migração de banco nova.**

- **Segredos fora do log.** O log de cada nó nunca guarda segredo: variável cujo nome sugere segredo (`token`, `senha`, `api_key`, `authorization`, `secret`…) sai como `***`, em qualquer nível do JSON. O token usado numa chamada REST, inclusive o buscado de outro nó, é apagado do corpo enviado, do recebido e da URL (EXE-99).
- **Mascarar no log.** Nova função de transformação «Mascarar no log» (`log_mask`, grupo Segurança): não muda o valor, só esconde no log de execução o campo indicado (TRF-35).
- **Sessão encerra após 2 h sem uso.** O frontend encerra a sessão depois de 2 h sem mouse, teclado, toque ou rolagem; o último uso vale para todas as abas, e a tela de login explica o motivo (SEG-26). O JWT continua válido por 24 h: é um controle do navegador.
- **Dados de origem no editor.** O editor de transformação e de consulta mostra as variáveis de origem do processo (o que chega antes de qualquer tarefa): as entradas do início HTTP, o último teste, o JSON da aba Teste ou um JSON de exemplo colado.
- **Exportar CSV.** Exportação de tabelas da tela de dados e do editor, com escape de aspas e separadores (`csv`, `exportCsv`).
- **Atenção:** a sessão do navegador passa a encerrar após 2 h sem uso; quem deixa a tela aberta e volta depois precisa entrar de novo.
- **Atenção:** o mascaramento é por nome do campo: um segredo num campo de nome comum (`valor`, `dado`) não é detectado. Use «Mascarar no log» nesses casos.
- Imagens no Docker Hub: `wendercorrea/orkestrix:backend-4.1.19`, `frontend-4.1.19` e `gateway-4.1.19`, com `upgrade.sh --pull` como na 4.1.3.

---

## Versão anterior: 4.1.18

Data: 13/11/2026. Mudam o **editor**, a **tela de Processos e de Execuções** e o **motor** (log de auditoria das chamadas REST). **Não há migração de banco nova.**

- **Editor.** Depois de publicar um diagrama novo, aparece **Salvar** (sobrescreve a versão, sem gerar revisão) e o botão principal vira **Implantar fluxo** (gera a revisão). O ícone de «Conectar elementos» passa a ser a seta.
- **Processos.** A lista padrão mostra só os implantados; as cópias de teste (`__teste`) ficam no filtro «Testes». Novo filtro «Todos os usos» (Normais, Chamadores, Subprocessos) com selos e links «chama / chamado por» que abrem a lista de processos relacionados (`kind`, `calls` e `calledBy` no catálogo).
- **Execuções.** Coluna «Revisão» e filtros por status, origem, chave, revisão e período (data e hora), aplicados no botão **Filtrar**. A listagem aceita esses filtros no backend (EXE-97).
- **Logs da execução.** Filtro por fase e busca; clicar num log abre um modal com o que o nó fez (variáveis criadas e alteradas, duração, status e tentativas) e a **chamada HTTP** completa.
- **Auditoria das chamadas REST.** Cada nó grava sempre entrada e saída; a tarefa REST grava sempre método, URL, headers, corpo enviado e recebido, status e duração, com **tokens, chaves e senhas mascarados** (EXE-98). As variáveis de cada nó continuam dependendo da captura de logs.
- **Atenção:** o log de auditoria passa a gravar mais linhas por execução (entrada e saída de todo nó, mesmo com a captura desligada, e uma linha `HTTP` por chamada REST); o mascaramento é por nome do campo, então um segredo num campo de nome comum não é detectado. Execuções anteriores a esta versão não têm a chamada HTTP no log.
- **Imagens** `wendercorrea/orkestrix:backend-4.1.18`, `frontend-` e `gateway-`; `upgrade.sh --pull` segue o mesmo caminho.

---

## Versão anterior: 4.1.17: notas da versão

Data: 12/11/2026. Mudam o **editor** e o **motor** (nó «Consultar banco»). **Não há migração de banco nova.**

- **Consultar banco com variáveis de saída.** Cada campo do resultado vira uma variável própria (`[0].cnpj` → `cnpj`), para os próximos nós usarem `${cnpj}` em vez de `${registros[0].cnpj}`. O editor mostra o valor do último teste e tem atalhos «+ coluna». Texto, número e booleano ficam nativos (dá para comparar em condições); caminho sem linha não grava a variável e não falha o nó. **Atenção:** a variável de linhas (`registros`) continua igual.
- **Corpo da requisição REST.** Painel «Dados disponíveis» ao lado do corpo, com «Buscar amostra» e inserção da variável no cursor.
- **Subprocesso, chamada de processo e repetição por item** no editor (validados contra o motor; ver `docs/testes/` e a 4.1.16).
- **Imagens** `wendercorrea/orkestrix:backend-4.1.17`, `frontend-` e `gateway-`; `upgrade.sh --pull` segue o mesmo caminho.

---

## Versão anterior: 4.1.16: notas da versão

Data: 12/11/2026. Muda só o **editor**: subprocesso, chamada de processo e repetição por item. **Não há migração de banco nova e o motor não muda** (ele já executava esses elementos; faltava desenhá-los e validá-los no editor).

- **Subprocesso no editor.** Novo componente «Subprocesso» (também na busca «Componente»): nasce expandido, com um início e um fim ligados dentro dele, num único passo do desfazer. O painel explica que o corpo roda como instância-filha e escolhe quais variáveis entram e saem (`*`, lista ou `destino=origem`).
- **Chamar processo.** Novo componente «Chamar processo»: escolha do processo publicado (lista do catálogo, com aviso se não estiver publicado), versão opcional e variáveis que entram e saem.
- **Repetir por item.** Tarefas, subprocesso e chamada de processo podem repetir por item de uma lista ou um número de vezes, em paralelo ou em sequência, guardando as saídas numa variável.
- **Validação no editor.** Subprocesso sem início ou sem fim dentro dele, chamada sem processo, versão que não é número inteiro, variáveis mal escritas e repetição sem lista nem número passam a ser **erros** antes de publicar. O corpo do subprocesso também é conferido (elemento que o motor não executa, passo sem entrada).
- **Atenção:** um subprocesso sem início e fim dentro dele, que antes passava no editor, passa a ser recusado; o motor já o recusava (PRF-26).
- **Lacunas conhecidas do reuso**, registradas em `specs/12-lacunas.md` (LAC-38 a LAC-40): o deploy não confere se o processo chamado existe; nada barra chamada circular; a tela de execuções não liga mãe e filha.
- Imagens `wendercorrea/orkestrix:backend-4.1.16`, `frontend-` e `gateway-`; `upgrade.sh --pull` segue o mesmo caminho.

---

## Versão anterior: 4.1.15

Data: 12/11/2026. Muda o **editor**, a **tela de execuções e de tarefas**, o **motor** (log de nós em espera e atualização da versão ativa) e a **consulta ao banco** (SQL). **Não há migração de banco nova.**

- **Consultar banco mais seguro e mais prático.** O SELECT recusa texto fora de aspas simples (`$$texto$$`, `$1`, barra invertida antes de aspa), porque o PostgreSQL os lê de outro jeito. O parâmetro `${variavel}` vai sem tipo no PostgreSQL, e um número que não combina com a coluna (um CNPJ numérico contra coluna de texto) é tentado de novo sem tipo. Datas no padrão SQL (`CURRENT_TIMESTAMP`, `CURRENT_DATE`, `INTERVAL '7' DAY`); `SYSDATE`, `GETDATE()` e parentes são recusados com a troca sugerida. No editor, uma barra insere comparações e datas e «Formatar» organiza o SQL. **Atenção:** consultas que usavam `SYSDATE`, `GETDATE()` ou `$$…$$` passam a ser recusadas.
- **Nó em espera aparece como espera.** Um nó que suspende (tarefa de usuário, timer, mensagem) grava `NODE_WAITING` e a fase `WAITING` no log, em vez de parecer concluído. A tela da execução mostra «em espera» e, para tarefa de usuário, de quem o fluxo espera (`GET /api/tasks?instanceId=`, de qualquer responsável).
- **Atualizar a versão atual.** Novo botão «Atualizar» no editor de um processo publicado, que troca o XML da versão ativa sem criar versão nova (`PUT /api/processes/deploy`), com as mesmas validações da publicação.
- **Tarefa de usuário.** O prazo (`dueIn`) é editado por número e unidade; o motor lê só dias, horas, minutos e segundos (`P2D`, `PT4H`, `P1DT12H`), e o editor avisa o que o motor ignora (`P1W`, meses, anos). Nova janela da tarefa e lista das tarefas em espera.
- **Do 4.1.14 sem mudança:** erro do «Mapear e transformar» com linha e coluna e a validação por amostra maior ou aleatória já estavam na versão anterior.
- Imagens `wendercorrea/orkestrix:backend-4.1.15`, `frontend-` e `gateway-`; `upgrade.sh --pull` segue o mesmo caminho.

---

## Versão anterior: 4.1.14

Data: 11/11/2026. Muda o **editor** («Mapear e transformar»), a **API de prévia** do editor (`/api/designer`) e o **motor** só nas mensagens de erro. **Não há migração de banco nova.**

- **Erro do «Mapear e transformar» com o campo e como ajustar.** Cada linha rejeitada mostra «Linha 44 · coluna retorno: «0.1» não é um número inteiro.», quantas outras linhas têm o mesmo problema, os valores de exemplo e uma frase de **como ajustar** (inteiro recebendo decimal, vírgula decimal, valor vazio, texto que não é número, booleano, data fora do formato ISO). Quando um tipo resolve (por exemplo Inteiro → Número), o botão «Trocar o tipo» faz a troca; «Ver o campo» leva à linha do mapeamento, que fica destacada. Na execução, a mensagem do nó cita o campo, a origem e «Como ajustar: …». A resposta da prévia ganha `issues` (erros por campo, contando todas as linhas) e, em cada erro, `source`, `type`, `value`, `hint` e `suggestType`.
- **Validar com mais dados.** «Validar com» escolhe 50, 500 ou 5.000 primeiras linhas, ou 500, 2.000 ou 5.000 aleatórias da planilha anterior. A aleatória lê o arquivo inteiro numa passada (sem guardá-lo na memória) e mostra «Amostra aleatória de N linhas, de T do arquivo», com o número da linha da planilha no erro. A prévia da planilha (`POST /api/designer/spreadsheet/preview`) aceita `rows` até 5.000 (era 200) e `mode` `first` ou `random`, e devolve `rowNumbers` e `totalRows`.
- **Spec de limites** (`specs/20-limites.md`) e specs 08 (TRF-33, TRF-34) e 15 (CON-23a).
- **Sem outra mudança para quem instala.** Imagens `wendercorrea/orkestrix:backend-4.1.14`, `frontend-` e `gateway-`; `upgrade.sh --pull` segue o mesmo caminho.

---

## Versão anterior: 4.1.13

Data: 11/11/2026. Muda o **motor** («Mapear e transformar») e o **`application.yml`** do backend. **Não há migração de banco nova.**

- **«Mapear e transformar»: o limite de linhas por etapa é configurável e subiu de 10.000 para 500.000.** Uma planilha de 163.302 linhas lida pelo «Ler planilha» parava aqui com «O limite por etapa é 10000». Nova variável `ORKX_TRANSFORM_MAX_ROWS` (padrão 500000); o `maxRows` do próprio nó vence. Valor inválido (menor que 1) impede a subida, como nos outros limites. A mensagem de erro cita as duas formas de aumentar.
- **Correção no `application.yml`:** na 4.1.12 o padrão do limite do collect da planilha ficou `:-500000`, que o Spring lê como `-500000`. Rodando pelo compose nada mudava (ele sempre repassa um valor válido), mas fora dele (IntelliJ, `spring-boot:run`) o backend não subia. Agora é `:500000`.
- **Compose, `.env.example`, README do pacote e spec 08 (TRF-24)** trazem `ORKX_TRANSFORM_MAX_ROWS`.
- **Sem outra mudança para quem instala.** Imagens `wendercorrea/orkestrix:backend-4.1.13`, `frontend-` e `gateway-`; `upgrade.sh --pull` segue o mesmo caminho.

---

## Versão anterior: 4.1.12

Data: 11/11/2026. Muda o **conector de planilha** (limite padrão e mensagem de erro) e os textos do editor. **Não há migração de banco nova.**

- **Planilha com «collect»: o limite padrão subiu de 50.000 para 500.000 linhas.** Na 4.1.11 a variável `ORKX_SPREADSHEET_COLLECT_MAX_ROWS` existia, mas o padrão continuava 50.000 e uma planilha de 163.303 linhas ainda parava sem configurar nada. Agora ela passa de fábrica; continua valendo a variável, e o `maxRows` do nó vence. As linhas ficam numa variável do processo (armazenada à parte quando grande), então planilhas muito maiores pedem «Gravar direto na tabela».
- **A mensagem de erro diz como resolver:** cita `maxRows` no nó, `ORKX_SPREADSHEET_COLLECT_MAX_ROWS` no servidor ou gravar direto na tabela.
- **Editor, README e spec 15** passam a falar em 500.000.
- **Sem outra mudança para quem instala.** Imagens `wendercorrea/orkestrix:backend-4.1.12`, `frontend-` e `gateway-`; `upgrade.sh --pull` segue o mesmo caminho.

---

## Versão anterior: 4.1.11

Data: 11/11/2026. Muda o **conector de planilha** (limite de linhas) e o **compose** (variáveis). **Não há migração de banco nova.**

- **Planilha com «collect»: o limite de linhas é configurável.** No modo «passar as linhas pelo fluxo» o limite era fixo em 50.000 linhas e `ORKX_SPREADSHEET_MAX_ROWS` só valia para o modo «gravar direto na tabela»: uma planilha de 163.303 linhas parava com «O arquivo passa de 50000 linhas». Nova variável `ORKX_SPREADSHEET_COLLECT_MAX_ROWS` (padrão 50000); o `maxRows` do próprio nó continua vencendo os dois. Para planilhas grandes, prefira gravar direto na tabela (sem esse teto e bem mais leve na memória).
- **`docker-compose.yml` do pacote repassa as duas variáveis de variáveis grandes** (`ORKX_VARIABLE_INLINE_MAX_CHARS` e `ORKX_VARIABLE_WARN_CHARS`), que a 4.1.10 aceitava no backend mas o compose não passava ao contêiner.
- **Sem outra mudança para quem instala.** As imagens do Docker Hub (`wendercorrea/orkestrix:backend-4.1.11`, `frontend-` e `gateway-`) e o `upgrade.sh --pull` seguem o mesmo caminho.

---

## Versão anterior: 4.1.10

Data: 11/11/2026. Mudam o **motor** (gravação em lotes, retenção, tarefas), o **backend** (histórico e listagens), a **imagem do backend** e o **front** (tela de tarefas, editor). **Não há migração de banco nova**: a `4.1.9_01` já cobre o esquema; o código novo lê o que a 4.1.9 gravou.

- **Gravação em tabela em lotes.** O «Gravar no banco» e o conector de planilha enviam as linhas ao banco em lotes de 500, na mesma transação, e o conector reaproveita o plano entre os lotes. Em PostgreSQL, 50 mil linhas de `upsert` que atualizam tudo passaram de 33 s para 4 s. Regra GRV-36. Sem mudança de esquema.
- **Retenção de dados antigos (desligada por padrão).** Com `ORKX_RETENTION_INSTANCES_DAYS=N`, uma limpeza agendada apaga instâncias terminadas há mais de N dias, com histórico, logs, tarefas e variáveis, em rodadas pequenas e sem apagar mãe de instância em andamento. Regra EXE-95. **Atenção:** o que sai não volta; faça backup antes de ligar. Sem mudança de esquema.
- **Imagem do backend.** A JVM passa a usar até 75% da memória do contêiner (`-XX:MaxRAMPercentage=75`; antes usava o padrão de 25%), o `HEALTHCHECK` da imagem vira uma chamada `curl` a `/actuator/health` (antes subia outra JVM inteira a cada 30 s) e os `-D` que vinham depois de `-jar` (ignorados pela JVM) saíram; o perfil `prod` vem de `ENV SPRING_PROFILES_ACTIVE=prod`, que o compose ou `-e` ainda trocam. **Atenção:** com `mem_limit` baixo a JVM agora enxerga o teto do contêiner e o heap cresce até 75% dele.
- **Tela de tarefas de usuário (`/tarefas`).** Antes só pela API: agora a lista (minhas, do meu grupo, todas para o ADMIN) com filtro de status e paginação, e o detalhe com o formulário e as ações Reivindicar, Devolver, Concluir, Aprovar e Reprovar, conforme o que o backend permite. Regra TAR-26; risca a LAC-34.
- **Correção na lista de execuções.** A paginação da 4.1.9 escondia a última linha de cada página (pedia «tamanho + 1» e o servidor desloca pelo tamanho). Agora cada página mostra todas as linhas; «Próxima» fica ativa com página cheia.
- **Editor: responsável e formulário da tarefa de usuário.** O painel de propriedades da «Tarefa de usuário» ganhou responsável, usuários e grupos candidatos, prazo, prioridade e o formulário (campos com nome, rótulo, tipo e obrigatório). Antes isso só entrava por XML. Regra TAR-27.
- **Editor: timer e erro.** Dois componentes novos, «Esperar (timer)» e «Tratar erro (borda)»; arrastados para a borda de uma atividade viram prazo (timer de borda) e caminho de erro. O painel configura o tipo e o valor do timer (com validação do formato ISO 8601), «Interromper a atividade» e o código do erro. O motor já executava esses eventos (EXE-50 a EXE-55); faltava o editor. Regra EXE-96.
- **Histórico e logs de uma instância com variáveis grandes não travam mais.** As entidades de histórico, logs e tarefas tinham uma relação «lazy» com `ProcessInstance` por `instance_id` (que não é a chave primária), e o Hibernate não cria proxy assim: carregava a instância inteira, com as variáveis, a cada linha lida (medido: 18,7 s para o histórico de uma instância de 58 MB, consultado a cada 3 s pela tela). As relações (e as coleções inversas `tasks` e `executionHistories`, que ninguém usava) saíram; as chaves estrangeiras continuam no banco. Sem mudança de esquema.
- **Editor: botão «Organizar».** Refaz o layout visual do desenho inteiro (posições e traçado dos fluxos) e desfaz sobreposições, usando o `bpmn-auto-layout` que já arrumava os importados. O processo não muda: elementos, ligações e configuração dos nós ficam iguais, e as anotações continuam ao lado do passo a que estão ligadas. «Layout anterior» volta ao desenho de antes e some na primeira edição.
- **Prévia das variáveis de instância terminada.** O resumo é calculado uma vez e reaproveitado (várias requisições juntas liam dezenas de MB ao mesmo tempo). Instâncias antigas e enormes ainda levam cerca de 10 s na primeira leitura a frio (LAC-37).
- **Sem outra mudança para quem instala.** As imagens do Docker Hub (`wendercorrea/orkestrix:backend-4.1.10`, `frontend-` e `gateway-`) e o `upgrade.sh --pull` seguem o mesmo caminho; o `docker-compose.yml` ganha só variáveis opcionais novas (`ORKX_RETENTION_INSTANCES_DAYS`, `ORKX_FILE_MAX_READ_SIZE`, `ORKX_SPREADSHEET_MAX_ROWS`, `ORKX_REST_PREVIEW_MAX_SIZE`, `ORKX_MAX_BODY_SIZE`, `ORKX_VARIABLE_*`); sem elas valem os padrões de sempre.

---

## Versão anterior: 4.1.9

Data: 11/11/2026. Mudam o **motor** (variáveis grandes e «Gravar no banco»), o **backend** (listagem de execuções, consulta SQL, limites) e o **front**. O banco ganha **uma tabela nova** (`process_variable_blob`, migração `4.1.9_01`): só acrescenta, a 4.1.8 a ignora.

- **Execuções não travam mais.** Uma instância que carrega uma planilha ou um JSON grande chegava a dezenas de MB em `process_instance.variables_json`, regravados a cada nó e relidos a cada consulta (backend a 250% de CPU). Agora o valor de variável acima de 64 KB vai para `process_variable_blob` e a instância guarda só a referência com o hash; o valor só é regravado quando muda. O motor, a retomada após reinício e o gateway continuam recebendo o valor inteiro. Instâncias antigas seguem legíveis como estão. Limites: `ORKX_VARIABLE_INLINE_MAX_CHARS` (padrão 65536; `0` desliga) e `ORKX_VARIABLE_WARN_CHARS` (o log avisa a partir de 5 MB).
- **Listagem de execuções (`/execucoes`).** O backend lê só o resumo (sem `variables_json`) e trata «em execução» (`RUNNING` + o legado `ACTIVE`) numa consulta paginada só. A tela deixou de buscar o detalhe de cada linha, ganhou 20/50/100 por página e «Próxima» exata. Os logs da execução renderizam 50 por vez.
- **Detalhe da execução e painel «Teste».** O acompanhamento pede só o estado; as variáveis vêm à parte, com valor grande resumido (`{"$truncated": true, "size", "preview"}`) e o botão «Carregar valor inteiro». Rotas: `GET /api/processes/instances/{id}/variables?preview=true` e `GET .../variables/{nome}`. Sem `preview`, a rota devolve tudo, como antes.
- **«Gravar no banco»: limpar a tabela antes (`dbClear`).** Apaga todas as linhas na mesma transação da gravação (se a gravação falhar, as linhas antigas voltam) e grava `<id>_deleted`. Regra GRV-05.
- **Consulta SQL melhor.** Editor com autocompletar (tabelas, colunas, variáveis, funções), `Ctrl+Enter`, «SELECT *» por tabela e tempo de execução. O erro aponta o trecho do SQL e sugere a correção (`FORM` → «Você quis dizer FROM?»). A tela «Dados» ganhou a consulta SQL (`POST /api/designer/data/query/run`, `PROCESS_READ`; o «Máximo de linhas» sugere 500 e aceita até 10.000) e paginação das linhas.
- **Limites configuráveis.** `ORKX_MAX_BODY_SIZE` (nginx, padrão `1m`), `ORKX_FILE_MAX_READ_SIZE` (conector «file», `5MB`), `ORKX_SPREADSHEET_MAX_ROWS` (`2000000`) e `ORKX_REST_PREVIEW_MAX_SIZE` (`1MB`, até 64MB). Sem a variável vale o valor de sempre.
- **«Mapear e transformar»: nova transformação «Truncar texto» (`truncate`).** Corta o texto no tamanho escolhido, de 1 até 4000 (o que cabe numa coluna `text`; o campo mostra o máximo). O «final» opcional (ex.: `…`) entra na conta. Texto menor não muda e o corte respeita emoji e acentos. Regra no quadro de funções de `specs/08`.
- **Editor:** os botões do painel lateral viraram um só (ocultar no próprio painel; o do canto só reabre).
- **Atenção:** o `nginx.conf` do front agora é um **modelo** (`/etc/nginx/templates/default.conf.template`, troca `${ORKX_MAX_BODY_SIZE}` ao subir). Quem monta esse arquivo direto em `conf.d` precisa montá-lo em `templates/`. O `ORKX_MAX_BODY_SIZE` entra no serviço do front do compose.
- **Atenção:** tela e API passam a ver valor grande de variável só resumido; quem precisa do valor inteiro usa `GET .../variables/{nome}` (ou a rota sem `preview`).
- **Sem outra mudança para quem instala.** As imagens do Docker Hub (`wendercorrea/orkestrix:backend-4.1.9`, `frontend-` e `gateway-`) e o `upgrade.sh --pull` seguem o mesmo caminho; rode `scripts/migrate.sh` (o `upgrade.sh` já faz).

---

## Versão anterior: 4.1.8

Data: 06/11/2026. Completa o «Montar JSON» da 4.1.7. Mudam o **motor** (um recurso no modelo do JSON) e o **front**. O banco é o mesmo da 4.1.7: **não há migração nova**, e a 4.1.7 não chegou a ser publicada, então quem vem da 4.1.6 recebe tudo junto.

- **«Montar JSON» lê os dados do nó anterior.** O diálogo ganhou o painel «Dados disponíveis», igual ao do «Mapear e transformar»: as variáveis dos nós anteriores com a amostra em árvore e o botão «Buscar amostra». Clicar num campo escreve o `${...}` dele no ponto do cursor. Uma lista entra inteira (`${linhas}`); um campo de um item entra como a lista desse campo (`${linhas[*].pedido}`); «Todos os campos» insere um objeto com os campos simples.
- **`[*]` no modelo (motor).** `${linhas[*].pedido}` entrega a lista desse campo em todos os itens; item sem o campo vira `null`. Regra JSN-02 em `specs/08-componentes-de-dados.md`. Modelos da 4.1.7 continuam iguais.
- **Modelo padrão corrigido.** O modelo de um «Montar JSON» novo usava `${linhas_count}`, variável que o motor não grava (o «Mapear e transformar» e o «Consultar banco» gravam `<id do nó>_count`). Agora o modelo já vem com as variáveis reais do nó anterior, e a prévia deduz a quantidade a partir da amostra. **Atenção:** um «Montar JSON» criado na 4.1.7 com o modelo padrão antigo falha na execução («a variável `linhas_count` não existe»); troque pela variável `<id>_count` do nó anterior, que aparece como botão no painel.
- **«Mapear e transformar» guarda a amostra ao salvar.** Antes ela só era guardada se o nome da variável de saída já estivesse preenchido durante o cálculo; agora os próximos nós («Montar JSON», «Gravar no banco») sempre a mostram.
- **Busca de componentes (Ctrl+K):** digitar logo depois de abrir não perde mais o foco (ele caía no botão «minimizar» do diálogo e o Enter minimizava em vez de adicionar).
- **Cursor no modelo do JSON:** o campo clicado entra onde o cursor estava, e no fim do texto quando o cursor não foi colocado (antes entrava no começo).
- **Sem mudança para quem instala.** As imagens do Docker Hub (`wendercorrea/orkestrix:backend-4.1.8`, `frontend-` e `gateway-`) e o `upgrade.sh --pull` seguem como na 4.1.3.

---

## Versão anterior: 4.1.7

Data: 06/11/2026. Mudam o **motor** (um componente novo), o **backend** (só o editor: uma rota de prévia) e o **front**. O banco é o mesmo da 4.1.6: **não há migração nova**. Quem não usa o componente novo só vê as mudanças da paleta do editor.

- **Componente novo «Montar JSON».** Preenche um modelo JSON com `${variável}` e grava o documento numa variável, para ser a resposta do processo (`response.body` do gateway) ou a entrada do próximo nó. Fora de aspas o valor entra como JSON (a lista do «Mapear e transformar» ou do «Consultar banco» entra como lista, número como número, texto ganha aspas); dentro de aspas entra como texto, já escapado. O conteúdo de uma variável nunca quebra o JSON nem acrescenta campos. Aceita caminhos (`${linhas[0].id}`) e `${$now}`, `${$instanceId}`, `${$nodeId}`. Variável inexistente falha o nó e segue pelo fluxo `exception`, como nos outros componentes. Regras JSN-01 a JSN-09 em `specs/08-componentes-de-dados.md`.
- **Aviso novo no deploy (PRF-32).** O validador avisa «Montar JSON» com modelo vazio, `${}` sem nome, modelo que não vira JSON válido ou variável de saída com nome inválido. É aviso, não erro: nenhum fluxo existente muda.
- **Prévia no editor.** O diálogo do «Montar JSON» tem «Ver o JSON», que monta o documento no backend com o mesmo código da execução (`POST /api/designer/data/json/preview`, permissão `PROCESS_CREATE`) e não grava nada.
- **Paleta com nomes.** Passar o mouse num ícone mostra o nome e o que o componente faz; um botão no topo da paleta («Mostrar ou esconder os nomes») escreve o nome ao lado de cada ícone e lembra a escolha. **Atenção:** os nomes das entradas ficaram curtos (por exemplo, «Tarefa de serviço (chamada REST)» passou a «Chamada REST»), a «Tarefa manual» ficou junto das tarefas, antes dos componentes de dados, e a paleta ganhou dois botões novos no topo (lupa e nomes). Os `data-action` das entradas não mudaram.
- **Busca de componentes.** Pela lupa da paleta, pelo botão «Componente» da barra ou por **Ctrl+K**: busca pelo nome, pelo que o componente faz ou por palavras conhecidas («api», «excel», «sql», «json», «decisão»), sem diferenciar acento. Enter adiciona: com um passo selecionado, o novo entra logo depois e já ligado a ele; sem seleção, entra no centro da tela.
- **Skill do gerador de BPMN** aceita `type: "json"` (`.claude/skills/orkestrix-bpmn`).
- **Sem mudança para quem instala.** As imagens do Docker Hub (`wendercorrea/orkestrix:backend-4.1.7`, `frontend-` e `gateway-`) e o `upgrade.sh --pull` seguem como na 4.1.3.

---

## Versão anterior: 4.1.6

Data: 05/11/2026. Mudam o **gateway**, o **backend** (só a publicação pelo editor), o **front** e o **menu do servidor**. O banco é o mesmo da 4.1.5: **não há migração nova**. Quem não usa o API Gateway só vê as mudanças do editor.

- **O gateway pode devolver o resultado do processo.** Uma API com `wait-for` e `response.body: <variável>` responde, no sucesso, com o valor dessa variável em JSON, sem envelope (o texto JSON gravado pelo motor vira lista ou objeto), e a instância vai nos cabeçalhos `X-Orkx-Instance-Id` e `X-Orkx-Instance-Status`. Variável ausente no fim do processo responde `502`; `202` (ainda rodando) e `422` seguem como antes, e a consulta pelo ID opaco devolve o resultado quando a instância conclui. Sem `response.body`, nada muda. Detalhes em `GATEWAY.md`.
- **Limite da resposta do motor no gateway** (`ORKX_GW_MAX_RESULT_SIZE`, padrão `2MB`, de 256KB a 64MB). **Atenção:** uma resposta do motor maior que o limite passa a dar `502`, com a explicação no log, em vez dos `202`/`413` enganosos de antes. Aumente o limite se uma API devolve muitas linhas.
- **Contrato com resposta.** O contrato em banco guarda o `body` (contratos sem ele continuam com o mesmo JSON e o mesmo hash). Na comparação de versões, ligar ou desligar o `body` é mudança **incompatível**; trocar a variável é compatível. Swagger e menu 13 (catálogo) mostram a resposta.
- **Editor: seção «Resposta» no «Publicar no API Gateway»**: só confirmar o recebimento, devolver o resultado do processo ou a situação com variáveis. O diálogo sugere as variáveis que o diagrama grava antes do fim e reabre a resposta já publicada.
- **Menu, opção 2 («Subir o sistema»):** pergunta se sobe o API Gateway também. Com «s», grava no `.env` só o que falta (profile, pepper e token da Management API aleatórios, nunca trocando um existente, e, se aceitar, catálogo em banco e contrato interno) e, ao fim, confere se a publicação pelo editor chega ao gateway. Com «n», sobe sem o gateway e para o que estiver rodando. A opção 12 (assistente do `.env`) pede o token e pergunta pela publicação pelo editor.
- **Exemplos do menu «Novo» corrigidos** para usar só o que o motor executa: na consulta de cliente, uma resposta `404` não derruba mais a instância (novo passo «Cliente encontrado?»); na análise de crédito, a análise manual não é mais pulada (a decisão `risco` não era sobrescrita pelo mapa da linha DMN); na carga de vendas, as linhas da planilha chegam ao mapeamento; nos itens do pedido, a lista `itens` entra pelo início HTTP; no pedido com aprovação, a decisão segue os botões Aprovar e Reprovar e o valor da fatura vai como número. Fluxos já criados a partir dos exemplos antigos não mudam: abra o exemplo de novo para pegar a versão corrigida.
- **Editor, painel Propriedades:** deixa de dizer «O motor não executa este elemento» em gateway baseado em evento, eventos intermediários e de borda, regra de negócio (DMN), chamada de processo, subprocesso e tarefa de recebimento, que o motor executa. O editor também deixa de descartar o formulário da tarefa de usuário (`orkx:formField`) ao abrir um diagrama.
- **Editor, mais espaço para o desenho:** os diálogos podem ser arrastados pelo título e minimizados para uma barra no canto; o painel lateral (no editor e na tela de execução) minimiza sem perder o que foi preenchido; a barra de ferramentas recolhe para cima e a paleta para a esquerda (as duas lembram o estado entre recargas).
- **Imagens no Docker Hub:** `wendercorrea/orkestrix:backend-4.1.6`, `frontend-4.1.6` e `gateway-4.1.6`, com `upgrade.sh --pull` como na 4.1.3.

---

## Versão anterior: 4.1.5

Data: 05/11/2026. **Só o front muda**, e só no que ele entrega ao navegador: o motor, o gateway e o banco são os mesmos e **não há migração nova**.

- **A imagem do front deixa de trazer o código-fonte.** Até a 4.1.4, o front compilado vinha com 17 arquivos `.map` (source maps), que embutem o código TypeScript original (94 arquivos de `src/`). Agora o build não gera `.map`, e a geração da versão **recusa** uma imagem do front que tenha algum. Nada muda no uso: o `.map` só era baixado com as ferramentas de desenvolvedor abertas, e as mensagens de erro no console passam a apontar para o arquivo minificado. **Atenção:** as imagens `frontend-4.1.2`, `frontend-4.1.3` e `frontend-4.1.4` publicadas no Docker Hub têm os `.map`; use a 4.1.5.
- **Menu «Novo» do editor** (vindo da 4.1.4): saem os cinco exemplos de IA e entram seis exemplos que rodam no motor, descritos nas notas abaixo.

---

## Versão anterior: 4.1.4

Data: 05/11/2026. **Só o front muda**: o motor, o gateway e o banco são os mesmos da 4.1.3 e **não há migração nova**.

- **Menu «Novo» do editor com outros exemplos.** Saem os cinco exemplos de IA (chat com o Claude, chat com o ChatGPT, ChatGPT gera pergunta e grava na tabela, Gemini gera pergunta e grava na tabela, pergunta básica ao Gemini). Entram seis exemplos que rodam no motor e passam no validador do designer: **classificar pedido** (decide por regra, sem nada externo: bom para o primeiro teste), **consulta de cliente com tratamento de falha** (REST com segredo `cadastro`), **análise de crédito com espera e decisão** (regra de risco, análise manual com prazo, espera de documentos e timers), **carga de vendas a partir de planilha**, **processar cada item do pedido** (laço sobre uma coleção) e **pedido com aprovação e nova tentativa**. O «Diagrama em branco», o exemplo de chamada REST e os assistentes «Processo que lê uma planilha…» e «Processo que gera um arquivo…» continuam. **Atenção:** quem usava os exemplos de IA do menu para começar um fluxo deixa de os ter lá; fluxos já criados não mudam.
- **Sem mudança para quem instala.** As imagens do Docker Hub (`wendercorrea/orkestrix:backend-4.1.4`, `frontend-` e `gateway-`) e o `upgrade.sh --pull` seguem como na 4.1.3.

---

## Versão anterior: 4.1.3

Data: 05/11/2026. **Sem mudança no motor, no banco nem no gateway** em relação à 4.1.2: o código que roda é o mesmo e **não há migração nova**. A novidade é a forma de receber as imagens.

- **Imagens pelo Docker Hub (opcional).** `scripts/upgrade.sh --pull <repositório>` e `scripts/rollback.sh --pull <repositório>` baixam as imagens de um repositório do Docker Hub em vez de carregar o `images/*.tar.gz`; o mesmo vale com `ORKX_IMAGE_REPO` no `.env`. O repositório é único e o componente vai na tag (`<repo>:backend-4.1.3`, `:frontend-4.1.3`, `:gateway-4.1.3`); as imagens baixadas são reetiquetadas como `orkestrix/<componente>:<versão>`, então o `docker-compose.yml` não muda. Sem a opção, tudo segue como antes (arquivo `.tar.gz` do pacote).
- **Para quem publica:** `release/scripts/publicar-imagens-docker-hub.sh <versão> [repositório]` envia as três imagens (confere a tag git e o commit das imagens, pede confirmação). Fica só nas fontes, fora do pacote. Mantenha o repositório do Docker Hub **privado**: o JAR vai dentro da imagem.
- **Skills do projeto** (só nas fontes, fora do pacote) renomeadas para `orkestrix`, `orkestrix-bpmn` e `orkestrix-arquiteto`. Quem está na 4.1.2 não precisa atualizar por causa desta versão, a não ser que queira usar o Docker Hub.

---

## Versão anterior: 4.1.2

Data: 05/11/2026. **Sem mudança de código nem de comportamento** em relação à 4.1.1: imagens, banco e configuração são os mesmos, e **não há migração nova**. Esta versão só traz, no repositório de fontes, as skills do projeto para o Claude Code (`orkestrix`, `arquiteto-de-integracoes` e `desenhar-bpmn`, em `.claude/skills/`), que não vão no pacote. Quem está na 4.1.1 não precisa atualizar.

---

## Versão anterior: 4.1.1

Data: 05/11/2026. O API Gateway ganha contrato interno com idempotência, entradas declaradas no início HTTP, catálogo em banco com versões e telas de gestão no designer. **Três migrações** (`4.1.0_01` a `4.1.0_03`), todas só acrescentam. Quem não liga os recursos novos não vê diferença: `ORKX_GW_ENGINE_API` continua `legacy` e `ORKX_GW_CATALOG_STORE` continua `file`. Detalhes de uso em `GATEWAY.md`.

**Novidades desde a tag 4.1.0.** Cinco mudanças; a primeira muda o comportamento de quem integra pelo endereço do front:

- **Início de fora só pelo gateway.** O nginx do pacote deixa de encaminhar `/api/processes/{chave}/start` e `/versions/{n}/start` (responde `403` com o aviso de usar o API Gateway). A tela passa a iniciar por `/manual-start`, que recusa conta só de serviço. **Atenção:** integração que chamava o `/start` pelo endereço do front (porta 80) passa a receber `403`: migre para o API Gateway. O gateway e os scripts do servidor não mudam (usam a rede interna).

- **`Idempotency-Key` vence em 24 h.** A reserva vale por 24 h (`ORKESTRIX_IDEMPOTENCY_TTL`); depois a mesma chave é um pedido novo, e um `409` de pedido que caiu no meio deixa de ficar preso para sempre. As chaves vencidas saem de `orkx_start_request` de hora em hora, e a tabela deixa de crescer sem limite. Sem migração.

- **Exemplo `curl` para o consumidor.** Ao publicar uma API pelo designer, o painel mostra um `curl` por método, montado só do contrato público (host e path públicos, chave como marcador, `Idempotency-Key` com o contrato `internal`), para entregar a quem vai chamar. Nada do processo aparece nele. Novo `ORKX_GW_PUBLIC_URL` (opcional) para o host do exemplo. Também em `GET /management/v1/apis/{id}/versions/{n}/curl`.

- **Pacote menor.** Só empacotamento, sem mudança de comportamento para quem usa o `prod` com PostgreSQL. O JAR do backend perde ~4,6 MB (`h2` e `aspectjweaver` ficam de fora, perfil `slim`) e o do gateway ~8,3 MB (`bcprov`). A imagem do backend deixa de guardar o JAR duas vezes (`COPY --chown` no lugar de `RUN chown -R`, menos uma camada de ~86 MB). As imagens do backend e do gateway passam para a base **Alpine** (`eclipse-temurin:21.0.6_7-jre-alpine`, com `curl` para os healthchecks): cada imagem fica ~112 MB menor no disco, e as duas juntas ~22 MB menores no `tar.gz` do pacote (a base é compartilhada). **Atenção:** o JAR de release não sobe mais sem o perfil `prod` (antes caía em H2 em memória), e os containers do backend e do gateway não têm mais `bash` nem `apt-get` (só `sh` e `apk`), para quem entra neles com `docker exec`.

- **Rotação de chave sem carência.** Rotacionar uma chave com carência 0 agora a invalida de fato na hora: o banco arredondava a expiração para cima (até 0,5 µs), e com o relógio do Windows uma chamada no mesmo instante ainda passava. Só afetava a janela de microssegundos logo após a rotação, e nenhum dado muda.

As seções abaixo descrevem tudo o que mudou em relação à 4.0.5, inclusive o que veio na 4.1.0.

## O que mudou em relação à 4.0.5

### Correções

- **Listas vazias no JSON do jar de release.** No pacote, `GET /api/processes/{key}/triggers` devolvia `[{},{}]` e os avisos de validação do deploy (`schemaWarnings`, `modelWarnings`) vinham vazios. Pelo mesmo motivo, as respostas de erro do `/api/processes` (processo inexistente, conflito de versão, corpo inválido) e o `/api/fallout/stats` chegavam como `401`, mesmo com login válido. A causa era a ofuscação do backend renomeando os campos de alguns records. A regra do ProGuard agora mantém records, controllers e DTOs, e a geração da versão confere isso e falha se algum sair renomeado. Sem mudança de contrato.
- **Conexão que cai depois de o pedido sair responde `502`** em vez de `500`. Sem `Idempotency-Key` o gateway continua sem repetir o pedido (a instância pode já existir).

### Contrato interno, papel `GATEWAY` e idempotência

- Novo contrato interno `/internal/v1` e papel `GATEWAY`, que só inicia processos, lê os inícios declarados e acompanha instâncias. `ORKX_GW_ENGINE_API` (`legacy` | `internal`, padrão `legacy`) escolhe qual o gateway usa.
- **Migração `4.1.0_01`** (tabela `orkx_start_request`). Com `internal`, o consumidor pode mandar `Idempotency-Key`: o gateway repete com segurança também `502`, `504` e conexão caída, e o motor devolve a mesma instância em vez de criar outra. A mesma chave com outro corpo recebe `422`.
- `GET /api/processes/{key}/triggers` passa a exigir `PROCESS_TRIGGERS_READ`, que todos os papéis com leitura já têm.

### Entradas do início HTTP e parâmetros de path

- `orkx:input` no início HTTP declara de onde vem cada valor (path, query ou corpo), o tipo e se é obrigatório; o que não for declarado é recusado (só no contrato `internal`; o `/start` legado não muda). No designer: painel de entradas no início HTTP e importar cURL de exemplo, com diff.
- `path-params` por API no gateway: `/api/v1/p/<api>/<p1>/...`, até 5 segmentos, com recusa de `%2F`, `%5C`, `.`/`..` e caracteres de controle.

### Catálogo do gateway em banco (opcional)

Com `ORKX_GW_CATALOG_STORE=database`:

- **Migrações `4.1.0_02` e `4.1.0_03`**: schema `orkx_gateway` (APIs, versões, revisões, auditoria, consumidores, chaves e permissões). O `catalogo.yml` é importado na primeira subida, sem mudar URLs.
- Versão pública na URL (`/api/v2/p/...`), revisões imutáveis com rollback, contrato público com parâmetros tipados e binding privado, diff de compatibilidade que só recomenda (versão nova só por pedido explícito).
- Management API na porta de gestão, com `ORKX_GW_MANAGEMENT_TOKEN`. Consumidores e chaves em banco: emitir, rotacionar com carência, revogar, suspender o consumidor e liberar ou retirar APIs, sem reiniciar. O jar do gateway traz só o driver do PostgreSQL.

### Designer

- **Publicar no API Gateway** (no início HTTP): cria e versiona a API a partir do processo publicado, com diff e decisão de versão na tela. Exige `ORKX_GW_MANAGEMENT_URL` e `ORKX_GW_MANAGEMENT_TOKEN` no backend.
- **Nova página Gateway** (só administradores): criar, ligar e desligar consumidores; emitir, rotacionar e revogar chaves (a chave completa aparece uma única vez); liberar e retirar APIs.

### Operação

- ID público opaco por API (`opaque-id`, com consulta `GET /api/v1/instances/{id}`), limite de chamadas por consumidor (`rate-limit`, por nó) e métricas `orkx.gateway.requests` na porta de gestão. O papel `GATEWAY` acompanha instâncias, o que libera o `wait-for` com ele. O nginx recusa `/internal/`.

## Como atualizar

`scripts/upgrade.sh` aplica as migrações, faz o backup completo antes e volta sozinho se algo falhar. As migrações só acrescentam, então voltar para a 4.0.x com o banco já migrado funciona (as tabelas novas ficam sem uso).

## Pontos de atenção

- `/api/processes/*/start` continua acessível a quem tem usuário do Orkestrix, e a porta 9000 do backend segue publicada. Para consumidores externos, entregue só a chave do gateway.
- O validador do motor avisa (PRF-19) que o início HTTP `PATCH` não é aceito, mas o PATCH funciona pelo gateway nos dois contratos. É só a mensagem que está desatualizada.
- O limite de chamadas (`rate-limit`) é por nó do gateway.

---

## Notas das versões anteriores

### 4.0.3: API Gateway configurável e assistente do catálogo

**API Gateway configurável e assistente do catálogo.** Sem mudança no motor, sem migração; quem não usa o gateway não vê diferença, e um catálogo da 4.0.x continua valendo com o mesmo comportamento.
- **Assistente no menu** (`orkestrix.sh`, opção 13): o **passo 1** gera a chave de um consumidor e já o grava no catálogo (rodar de novo troca a chave); o **passo 2** monta ou altera uma API por perguntas. Também libera, lista e remove, e aplica (reinicia o gateway). Nada de copiar e colar YAML: quem grava é a imagem do gateway, que valida tudo com as mesmas regras de quando sobe.
- **Opções por API** no catálogo: `retry` (repete só quando é certo que nada foi criado: conexão recusada ou `503`), `errors` (mensagem própria por status), `response` (status de sucesso, campos e `wait-for`: esperar o processo terminar e devolver as `variables` escolhidas; `422` se ele terminar sem sucesso, `202` se o tempo acabar antes). O `timeout` precisa cobrir as tentativas.
- Novos códigos de resposta: `503` com `Retry-After` (Orkestrix indisponível, nada criado), `422` e `202` (só nas APIs com `wait-for`). O Swagger de cada API mostra o que ela devolve e as mensagens próprias.
- Por dentro: o filtro do gateway passou a fazer ele mesmo as chamadas ao Orkestrix, em vez de usar o proxy do Spring Cloud Gateway (necessário para repetir com o corpo e esperar o fim). Todas as proteções anteriores continuam e foram testadas de novo.

**4.0.2 (sobre a 4.0.0):** só ferramentas e tela, sem mudança no motor, sem migração e sem mudança na API. (1) O menu (`orkestrix.sh`) ganhou a opção 12, o **assistente do `.env`**: pergunta cada valor, gera segredos aleatórios, e, se já existe um `.env`, oferece o backup `.env_bkp_AAAAMMDD` antes de alterar. (2) Novo guia `INSTALAR-PASSO-A-PASSO.md` no pacote. (3) Editor: a paleta de elementos deixa de ficar cortada quando a janela é menor que ela (passa para duas colunas e rola se ainda não couber).

O texto abaixo é o da 4.0.0, que continua valendo.

Motor de execução e orquestração de processos BPMN 2.0, com backend Java 21/Spring Boot, editor visual e telas de operação. Data: 02/11/2026.

**Versão maior porque o produto ganha uma parte nova: o API Gateway** (módulo `orkestrix-gateway`, opcional). Nada do que existia muda: **sem migração de banco** e sem mudança na API do motor. Quem atualiza e não liga o gateway não tem nenhuma diferença de comportamento.

As versões 3.0.7 a 3.1.3 só mexeram nas ferramentas de geração do pacote e da distribuição (nenhuma mudança no código do sistema entre a 3.0.6 e a 3.1.3, conforme o histórico do git); por isso estas notas comparam com a 3.1.3, e o motor, o banco e as telas são os da 3.0.6.

## O que mudou em relação à 3.1.3

### API Gateway (novo, opcional)

Porta de entrada **externa** dos processos que começam por HTTP (`GET`, `POST`, `PUT`, `PATCH`): quem consome usa uma chave de API e uma URL estável (`/api/v1/p/<api>`), e o backend não precisa ficar exposto. Guia completo no **`GATEWAY.md`** do pacote.

- **Chave de API por consumidor**, guardada só como hash (HMAC-SHA-256 com um segredo, o *pepper*). Cada consumidor é liberado só para as APIs dele. `scripts/gateway-chave.sh <consumidor>` gera a chave pela imagem Docker.
- **Catálogo** em `gateway/catalogo.yml`: quais processos são publicados, com quais métodos, limites de corpo e de tempo, título, descrição e exemplo.
- **Proteções:** só os métodos liberados (`405` no resto), corpo e query com limite, só JSON, JSON com profundidade e chaves duplicadas controladas, e **recusa dos campos que controlam o motor** (`autoCompleteUserTasks`, `persistence`, regras de ETL), para um cliente externo não pular aprovações humanas nem gravar em tabelas. A resposta traz só `instanceId`, `status` e `startedAt`; erros no formato RFC 9457, sem dado interno. Headers de segurança em toda resposta e correlation-id.
- **Swagger** em `/api/v1/docs` (OpenAPI 3.1 gerado do catálogo, filtrado pela chave, sem CDN e com CSP restritiva), `GET /api/v1/catalog` e `GET /api/v1/openapi.json`.
- **Como entrar no Orkestrix:** com o usuário de serviço (padrão `admin`; o ideal é um usuário próprio com papel `SYSTEM`, no `GATEWAY.md`).
- **O que não faz ainda:** não devolve o resultado do processo (`201` confirma só o **início**), sem limite de requisições por consumidor, sem idempotência, sem CORS (é para chamadas entre servidores), o catálogo é lido na subida (editar exige reiniciar o gateway) e a revogação é tirar o consumidor do catálogo. O plano das próximas etapas está em `docs/arquitetura/api-gateway.md` do repositório.

### Editor

- O gatilho de início «Requisição HTTP» passa a oferecer **`PATCH`** (além de `GET`, `POST` e `PUT`). Antes, um início `PATCH` era desfeito pelo editor ao reabrir o processo.

### Pacote e operação

- O pacote leva a imagem **`orkestrix/gateway`** (junto com a do backend e a do front, no mesmo `images/*.tar.gz`), o `gateway/catalogo.yml`, o `GATEWAY.md` e `scripts/gateway-chave.sh`.
- No `docker-compose.yml` o gateway é o serviço `orkestrix-gateway`, com o **profile `gateway`**: só sobe com `COMPOSE_PROFILES=gateway` no `.env`. Porta `8090` (`GATEWAY_PORT`). Variáveis novas no `.env.example`, todas opcionais para quem não liga o gateway.
- `scripts/upgrade.sh`: com o gateway ligado, confere o `.env` (o pepper) **antes** de parar qualquer coisa, inclui o gateway na parada e na subida, e exige que ele fique saudável e responda; senão volta sozinho para a versão anterior. `scripts/rollback.sh` para uma versão **anterior à 4.0.0** avisa que ela não tem o gateway e o deixa desligado. `restore.sh`, `status.sh` e o menu (`orkestrix.sh`: logs e testes do gateway) também o conhecem.
- `build-release.sh` constrói, confere a origem (commit) e empacota a imagem do gateway, e passa a recusar arquivo novo não versionado em `orkestrix-gateway/` ou `release/`. `gerar-distribuicao.sh` compila o JAR do gateway e o leva na distribuição compilada, **onde o gateway sobe por padrão**: o gerador tira o profile `gateway` do compose e dá a ele um pepper padrão de desenvolvimento (como o JWT e a senha do admin dessa distribuição, que devem ser trocados antes de expor o sistema); sem nenhuma API no catálogo, ele sobe sem publicar nada.

## Como atualizar

`scripts/upgrade.sh --from-dir ../orkestrix-<versão atual>`, como sempre. **Sem migração**: as imagens trocam e o banco continua como está. Para usar o gateway depois de atualizar, siga o `GATEWAY.md` (ligar, gerar a chave, declarar a API, reiniciar).

## Limites conhecidos

- O gateway entra no Orkestrix com o usuário de serviço (padrão `admin`, que tem poderes além de iniciar processos). Uma permissão dedicada está planejada.
- `GET /api/processes/{chave}/triggers` do motor devolve a lista de inícios com objetos vazios (`[{},{}]`); o gateway não depende dele hoje, mas a correção é pré-requisito da próxima etapa do gateway.
- O `startedAt` que o motor devolve (e o gateway repassa) vem deslocado pelo fuso do servidor; o histórico da instância tem a hora correta.

---

Notas das versões anteriores:

## Orkestrix 3.0.6: notas da versão

Motor de execução e orquestração de processos BPMN 2.0, com backend Java 21/Spring Boot, editor visual e telas de operação. Data: 30/10/2026.

**Sem migração nova** e sem mudança no banco, na API nem nas telas: o contrato é o mesmo da 3.0.5. Muda só o que vai dentro do JAR.

## O que mudou em relação à 3.0.5

### Build

- O backend passa a sair com o miolo interno do `orkestrix-starter-engine` e do `orkestrix-starter-auth` ofuscado (ProGuard, profile Maven `obfuscate`, ligado só na geração da versão — o `mvn test` do dia a dia não muda). Ofusca campos e métodos privados (o runtime do motor BPMN e a cripto do cofre de segredos); a API pública, os beans do Spring, os DTOs e os records continuam com o nome original, porque são usados entre módulos ou serializados na API.
- As regras ficam em `orkestrix-system/orkestrix-starter-engine/proguard.conf` e `orkestrix-system/orkestrix-starter-auth/proguard.conf`.

### Como foi verificado

- App empacotado com os dois módulos ofuscados: smoke completo (saúde, login, publicar, rodar até concluir) e o cofre de segredos (criar, ler o token, remover) — a cripto ofuscada do `SecretVault` segue funcionando.
- Build da imagem Docker com o profile `obfuscate` ligado.

---

## Orkestrix 3.0.5: notas da versão

Motor de execução e orquestração de processos BPMN 2.0, com backend Java 21/Spring Boot, editor visual e telas de operação. Data: 30/10/2026.

**Sem migração nova** e sem mudança no motor, no banco ou nas telas: o backend e o front são os da 3.0.4. Muda o pacote e a distribuição.

## O que mudou em relação à 3.0.4

### Pacote

- O pacote passa a levar o `docker-compose.rede.yml` (pasta compartilhada num servidor de arquivos, SMB ou NFS) e o `scripts/configurar-pasta-compartilhada.ps1`, que o próprio `docker-compose.yml` manda rodar e não vinha junto.
- A geração do pacote recusa terminar se o compose montar um arquivo, ou o compose e o `INSTALL.md` citarem um script, que não foi para o pacote.

### Distribuição compilada

- Nova forma de entrega, num repositório separado só com o compilado: o JAR do backend, o front já compilado, o compose, o menu e os scripts de operação, sem código-fonte. As imagens Docker são montadas desses arquivos. Ela é gerada a partir da tag, e o passo a passo está em `docs/execucao/COMO-GERAR-TAG-E-RELEASE.md` (passo 11).
- O menu (`orkestrix.sh`) mostra a versão gravada em `VERSION` e, numa distribuição sem fontes, esconde «Gerar o pacote de release» e «Rodar os testes».

### Como foi verificado

- Instalação do zero da distribuição compilada (sem containers nem volume do banco anteriores), seguida do teste rápido: saúde, login, publicar e rodar um processo.

---

## Orkestrix 3.0.4: notas da versão

Motor de execução e orquestração de processos BPMN 2.0, com backend Java 21/Spring Boot, editor visual e telas de operação. Data: 30/10/2026.

**Sem migração nova** e sem mudança no esquema do banco: a atualização da 3.0.3 (ou anteriores) é só trocar as imagens.

## O que mudou em relação à 3.0.3

### Cofre de segredos

- **A forma `${secret.nome}` deixou de valer.** Só `${secret.nome.TOKEN}` (e `${secret.rede.nome_usuario.TOKEN}` / `senha_usuario` nos de rede) é resolvida. Fluxo que ainda usa a forma antiga falha antes de sair, com «Segredo não cadastrado no cofre».
- **Antes de atualizar:** troque, nos fluxos, `${secret.nome}` por `${secret.nome.TOKEN}`, copiando o token na tela do cofre (o botão `{ }` do editor já insere a forma certa). Só o dono do segredo e o administrador veem o token.
- Os modelos do editor (Claude, ChatGPT, Gemini) trazem `.TOKEN` como marcador para ser trocado pelo token do seu segredo.

---

## Orkestrix 3.0.3: notas da versão

Motor de execução e orquestração de processos BPMN 2.0, com backend Java 21/Spring Boot, editor visual e telas de operação. Data: 30/10/2026.

**Sem migração nova** e sem mudança no esquema do banco: a atualização da 3.0.2 (ou da 3.0.1 e da 3.0.0) é só trocar as imagens. Muda o cofre, a leitura de planilhas e o editor.

## O que mudou em relação à 3.0.2

### Cofre de segredos

- **Token em todo segredo.** Os segredos comuns também ganham, ao cadastrar, um token aleatório de 32 caracteres, e o fluxo usa `${secret.nome.TOKEN}` (como já era nos de rede). O token aparece na lista do cofre, com botão de copiar, só para o dono e o administrador, e o botão `{ }` do editor já o insere. A forma antiga `${secret.nome}` continua valendo, para não quebrar fluxos existentes. Segredos antigos recebem o token no primeiro uso.

### Ler planilha

- **Excel antigo (`.xls`)**: lido pelo Apache POI (limite do formato: 65.536 linhas). Antes o arquivo nem aparecia no seletor.
- **Linha do cabeçalho**: ignora os títulos antes dos nomes das colunas (extratos, relatórios). Fica nas propriedades do nó e em «Carregar dados», onde também há «Tem cabeçalho», com um aviso quando a primeira linha parece dado.
- **CSV**: o delimitador (`;`, `,` ou tab) é deduzido de até 30 linhas, não só da primeira. Um título sem delimitador no topo quebrava a leitura.
- **Chaves**: `keys` só são cobradas quando há tabela e o modo é `upsert`. Um valor esquecido nas propriedades derrubava a leitura.
- **Mapear e transformar** passa a enxergar as linhas do «Ler planilha» (`<id>_linhas`), e «Buscar amostra» lê as primeiras 50 linhas do arquivo pelo backend.
- **Mapear todos**: cria uma coluna para cada campo do objeto (ou da lista de objetos) de uma vez.

### Editor

- O ícone dos componentes de dados no diagrama ficou menor, no canto, e o nome da tarefa não é mais coberto.

### Documentação

- `docs/execucao/COMO-GERAR-TAG-E-RELEASE.md`: passo a passo para gerar a tag e o pacote (fica no repositório; o pacote não leva a pasta `docs/`).

### Como foi verificado

- Testes automatizados: 19 do conector Ler planilha (inclusive `.xls`, linha do cabeçalho, delimitador e chaves), 17 do Gerar arquivo, o cofre e a chamada REST e 229 do frontend.
- No Docker, com PostgreSQL: backup completo e restauração, leitura do `.xls` e de um CSV com título pela API, prévia com a linha do cabeçalho.

---

Notas das versões anteriores:

## Orkestrix 3.0.2: notas da versão
Motor de execução e orquestração de processos BPMN 2.0, com backend Java 21/Spring Boot, editor visual e telas de operação. Data: 29/10/2026.

Esta versão é **igual à 3.0.1 no motor e no esquema do banco** (sem migração nova). Muda o editor, e os scripts de operação ganham um menu interativo e backup completo ou só do esquema. A atualização da 3.0.0 ou da 3.0.1 é só trocar as imagens.

## O que mudou em relação à 3.0.1

### Editor

- **Ícones novos na paleta** para Mapear e transformar, Gravar no banco, Consultar banco, Ler planilha e Gerar arquivo, no mesmo estilo das tarefas nativas (retângulo arredondado com um marcador).
- **O mesmo ícone no desenho do processo**: esses cinco componentes aparecem no diagrama (editor e tela de execuções) com o ícone da paleta, no lugar da engrenagem ou do rolinho de script. Os marcadores de repetição continuam.

### Scripts de operação

- **Menu interativo** (`orkestrix.sh`, ou `orkestrix.cmd` no Windows, que usa o Git Bash): estado, subir e parar, logs, backup, restauração, pasta compartilhada e, no pacote, atualizar, voltar de versão e aplicar migrações. Funciona em Linux, macOS e Windows com Git Bash.
- **Backup completo ou só do esquema** (`backup.sh --completo|--esquema`; o padrão é o completo). O arquivo é conferido ao final. **O backup do esquema não restaura**: `restore.sh` e `rollback.sh --restore` o recusam.
- **Pacote enxuto**: o pacote leva só o `README.md`, `INSTALL.md`, estas notas, o `schema.sql` e os scripts; specs, a pasta `docs/`, `CLAUDE.md` e `GEMINI.md` ficam no repositório.

---

## Orkestrix 3.0.1: notas da versão

Esta versão é **igual à 3.0.0** no motor, no esquema do banco, nos scripts e nas telas: muda só a documentação que vai no pacote. Quem já instalou a 3.0.0 não precisa atualizar. Quem for instalar agora deve usar a 3.0.1, porque as notas da 3.0.0 ficaram incompletas na tag que já estava publicada (e uma tag publicada não se move).

## O que mudou em relação à 3.0.0

- **Notas da versão** completas: a seção «Como foi verificado» (testes e o que foi rodado no Docker com PostgreSQL) e a lista da documentação.
- **README** da raiz e do frontend com planilhas e arquivos (pasta compartilhada, servidor de rede, Gerar arquivo), o cofre com usuário e senha de rede, a biblioteca de processos e os módulos atualizados.
- **Sem migração nova** e sem mudança de comportamento: a atualização da 3.0.0 é só trocar as imagens. Da 2.0.4, siga «Atualizando da 2.0.4», nas notas da 3.0.0 logo abaixo.

---

## Orkestrix 3.0.0: notas da versão

Motor de execução e orquestração de processos BPMN 2.0, com backend Java 21/Spring Boot, editor visual e telas de operação. Data: 29/10/2026.

A 3.0.0 leva o Orkestrix além da orquestração: os processos passam a **ler planilhas grandes e gerar arquivos**, na pasta compartilhada com o Windows ou em servidores de rede, com o login guardado no cofre. Chegam também a **biblioteca de processos** e o **Java 21**.

### O que mudou em relação à 2.0.4

#### Planilhas e arquivos (novo)

- **Ler planilha** (conector `spreadsheet`): lê `.xlsx` e `.csv` de dezenas de MB linha a linha, sem carregar o arquivo na memória (um xlsx de 28 MB com 600.000 linhas foi lido em 29 s com 400 MB de memória). Escolhe a aba e as colunas, deduz o tipo de cada coluna (inteiro, decimal, data, texto), converte datas do Excel e mantém zero à esquerda. Dois jeitos de entregar os dados: **gravar direto na tabela**, em lotes e com atualização por chave (para arquivos muito grandes), ou **entregar as linhas ao passo seguinte** para o fluxo Ler planilha → Mapear e transformar → Gravar no banco.
- **Carregar dados**: antes de montar o fluxo, o editor abre o arquivo e mostra a prévia (abas, colunas com o tipo e as primeiras linhas) para escolher o que carregar.
- **Listar arquivos**: seletor de pastas e planilhas em lista, como o Explorador do Windows (nome, data de modificação, tipo e tamanho), na pasta compartilhada ou no servidor de rede.
- **Gerar arquivo** (conector `export`): grava **Excel, CSV, TXT ou JSON** a partir das linhas de um passo anterior ou de uma tabela de dados. Escolhe-se o que fazer com o arquivo: **atualizar** (grava por cima), **gerar um novo a cada execução** (com sufixo de data e hora, só data ou contador 1, 2, 3…), **acrescentar ao final** ou recusar se já existir. O nome aceita `{data}`, `{hora}` e `{timestamp}`. O Excel é escrito linha a linha, com cabeçalho em negrito, linha congelada e datas formatadas; o arquivo só aparece no destino quando está completo.
- **Andamento no log**: os dois conectores registram no log do nó o início, as linhas processadas com a velocidade (linhas/s) e a hora do fim; a tela de execuções mostra isso enquanto o nó ainda roda.
- **Pasta compartilhada com o Windows**: o backend monta uma pasta do computador (`ORKX_SHARED_DIR`, padrão `./data/files`) em `/app/shared`. O script `scripts/configurar-pasta-compartilhada.ps1` cria a pasta, grava o `.env`, compartilha na rede (opcional) e recria o backend. O `docker-compose.rede.yml` monta NFS ou SMB aberto pelo próprio Docker.
- **Servidor de rede (SMB)**: o aplicativo lê, lista e grava direto em compartilhamentos SMB2/3 (Windows, Samba, NAS), sem montar nada no Docker.
- **Cofre: usuário e senha de rede**: nova opção no cofre. Usuário e senha ficam criptografados juntos (AES-256-GCM, como os outros segredos). No processo, a referência leva um token aleatório de 32 caracteres, `${secret.rede.nome_usuario.TOKEN}` e `${secret.rede.senha_usuario.TOKEN}`: quem só sabe o nome do segredo não consegue usá-lo. O editor recusa senha escrita no processo.
- **Assistentes e paleta**: Novo → «Processo que lê uma planilha» e «Processo que gera um arquivo», botões **Ler Excel** e **Gerar arquivo** em Processos, e os itens **Ler planilha** e **Gerar arquivo** na paleta, com painel de propriedades e validação.

#### Biblioteca de processos

- Todo diagrama publicado aparece numa biblioteca organizada por tipo de processo (principais, apoio, gestão) e departamento, com busca por palavra-chave (sem diferenciar acento ou caixa; procura na chave, no nome, na descrição e nos nomes dos elementos do BPMN), pastas e classificação. API em `/api/catalog`. A árvore da lateral começa por «Sem classificação» e «Todos os processos».

#### Dados

- Na tela **Dados**, o administrador geral pode **apagar as linhas** ou **excluir a tabela** de um fluxo, confirmando pelo nome da tabela. Só tabelas criadas por «Gravar no banco» podem ser apagadas, e só por `ADMIN`.

#### Editor e telas

- O painel lateral do editor e o da tela de execução podem ser **desacoplados**: viram uma janela arrastável e redimensionável.
- Tarefas de conector (e-mail, arquivo, planilha) deixam de receber os avisos de «Gravar no banco» ao publicar.

#### Plataforma

- **Java 21**; guias de execução no IntelliJ, scripts do backend em `orkestrix-system/scripts/` e documentação reorganizada em `docs/` (execução, Docker, produto).

### Atualizando da 2.0.4

- **Migração `3.0.0_01`** (tabelas da biblioteca, `process_catalog_entry` e `process_catalog_folder`): só acrescenta; o `scripts/upgrade.sh` aplica. A 2.0.4 ignora essas tabelas, então voltar de versão continua sendo trocar a imagem.
- **Cofre**: as colunas novas de `orkx_secret` (`kind` e `ref_token`) são criadas sozinhas no primeiro uso, como o resto do cofre. Os segredos antigos continuam valendo, como segredos comuns, e `${secret.nome}` não mudou.
- **Pasta compartilhada**: o backend ganha um volume (`ORKX_SHARED_DIR` → `/app/shared`). Sem configurar nada, usa `./data/files` ao lado do `docker-compose.yml`.
- Nenhuma outra migração.

### Limites conhecidos

- O acesso SMB foi testado com um servidor simulado, não contra um Windows ou Samba real. Se o primeiro acesso falhar, a mensagem de erro traz a causa (versão do SMB, domínio ou permissão do compartilhamento).
- As horas dos logs e do histórico saem no horário do container (UTC). Para o horário local, defina `TZ` no backend; as execuções antigas continuam no horário em que foram gravadas.
- Ao entregar as linhas ao passo seguinte, o limite padrão é de 50.000 linhas (ajustável em `maxRows`); acima disso, grave direto na tabela. O Excel gerado aceita até 1.048.575 linhas de dados.
- O Excel gerado foi conferido com uma biblioteca independente (`openpyxl`), mas não foi aberto no Microsoft Excel.
- A referência com token aparece no XML do processo: quem lê o XML publicado a vê. Ela só vale nos conectores, não na chamada REST.
- A atualização por chave de um arquivo grande no PostgreSQL é mais lenta que a gravação nova: 600.000 linhas levaram 6 min 24 s.

### Como foi verificado

- **Testes automatizados** desta versão: 16 do conector Ler planilha e 17 do Gerar arquivo (cada formato, cada modo, sufixos, acrescentar e a pasta de rede com servidor simulado), 22 do cofre (inclusive usuário e senha de rede e o token), 7 de apagar e ler tabelas em streaming, 43 do validador do BPMN e 228 do frontend.
- **No Docker, com PostgreSQL 16 e o aplicativo completo**, pelo navegador: assistente de leitura com a planilha real do projeto (1.332 linhas, 6 colunas escolhidas na prévia), fluxo Ler planilha → Mapear e transformar → Gravar no banco publicado e executado, tabela conferida no banco, log de andamento na tela de execução; assistente de geração com contador (`relatorio_1`, `_2`, `_3`) e, por API, os modos atualizar, novo com data e hora e com só a data, acrescentar em TXT, Excel e JSON, e recusar se já existe.
- **Excel gerado** conferido com `openpyxl` (aba, cabeçalho em negrito, linha congelada, datas, números e quantidade de linhas); **leitura** de um xlsx de 28 MB e 600.000 linhas em 29 s com 400 MB de memória.
- **Segurança do cofre**: usuário e senha de rede cifrados em repouso, sem aparecer na API, no log nem nas variáveis do processo; referência sem o token não resolve.

### Documentação

- `README.md`: seção «Planilhas e arquivos (pasta compartilhada)», cofre com usuário e senha de rede, módulos atualizados (conectores e biblioteca).
- `orkestrix-frontend/README.md`: telas novas e como funcionam os assistentes, a prévia e o seletor de arquivos.
- `specs/`: COF-50 a COF-53 (cofre de rede) e CON-23 e CON-24 (Ler planilha e Gerar arquivo).
- `docs/produto/EVOLUCAO.md`: a evolução da ferramenta, versão a versão, e o que vem a seguir.

## Orkestrix 2.0.4: notas da versão

Motor de execução e orquestração de processos BPMN 2.0, com backend Java/Spring Boot, editor visual e telas de operação. Data: 21/10/2026.

### O que mudou em relação à 2.0.3

- **PostgreSQL publicado na porta 5435 do servidor** (`127.0.0.1:5435→5432`), para ferramentas de banco como DBeaver, pgAdmin ou `psql -h 127.0.0.1 -p 5435`. Só no próprio servidor, como a API. A porta muda com `POSTGRES_PORT` no `.env`. O backend continua falando com o banco pela rede interna do Docker.
- Motor, esquema do banco e scripts iguais aos da 2.0.3: a atualização não tem migração nova.

### O que mudou da 2.0.2 para a 2.0.3

O motor é o mesmo da 2.0.2. Esta versão traz a **atualização de versão por script**, que passa a vir em todo pacote:

- `scripts/upgrade.sh` atualiza uma instalação existente mantendo os dados. Ele faz backup conferido do banco, para o backend com parada limpa, aplica as migrações pendentes, sobe a versão nova, espera ficar saudável e roda o smoke. Se algo falhar, volta sozinho para a versão anterior.
- `scripts/rollback.sh` volta de versão. Por padrão troca só as imagens, porque as migrações só acrescentam e a versão anterior lê o banco novo; com `--restore` volta também os dados para um backup.
- `scripts/migrations/` traz as mudanças de esquema versionadas. Cada migração roda numa transação e fica registrada na tabela `orkestrix_schema_version`, com o SHA-256 do arquivo. O histórico de atualizações fica em `orkestrix_release_history`.
- `scripts/status.sh`, `backup.sh`, `restore.sh` e `migrate.sh --check` completam o conjunto.
- A versão das imagens vem de `ORKESTRIX_VERSION` no `.env`, e `ORKESTRIX_DDL_AUTO=validate` permite rodar sem o Hibernate alterar o esquema.
- Cada pacote sai de um commit com tag, e as imagens levam o commit de origem (`org.opencontainers.image.revision`) e o arquivo `BUILD-INFO`. A imagem 2.0.1 que circulou localmente em 20/10/2026 tinha sido gerada com alterações não commitadas e não disparava timers. O `dist/orkestrix-2.0.1.zip` foi regerado a partir da tag `v2.0.1`.

Para atualizar a partir da 2.0.1 ou da 2.0.2: veja «Atualizar de versão» no `INSTALL.md`.

### O que mudou da 2.0.1 para a 2.0.2

Confiabilidade do motor (detalhes, garantias, limites e medições em `docs/arquitetura/confiabilidade-do-motor.md`):

- **Timers:** cada timer é reivindicado no banco antes de disparar, e o token é gravado antes de o timer ser dado como concluído. Uma queda no meio do disparo não avança o processo duas vezes nem deixa o token esperando para sempre. O relógio lê lotes limitados, acorda no vencimento e, sem trabalho, consulta o banco 1 vez por segundo (antes, 4). Os timers de instâncias encerradas são apagados.
- **Junção paralela:** espera um token por fluxo de entrada. Antes, dois tokens pelo mesmo fluxo (num ciclo) ou o mesmo token contado duas vezes depois de uma queda liberavam a junção sem o outro ramo. Um ramo lento não derruba mais a instância por esperar a trava.
- **Recuperação:** a junção pela metade, o timer vencido durante a queda e a tarefa humana voltam ao ponto certo, validados por um teste que mata a JVM à força.
- **Memória:** só as últimas 1000 instâncias encerradas ficam na memória; tokens concluídos e o histórico do token têm limite; um ciclo sem espera falha em vez de prender a instância.
- **Logs:** o SQL do Hibernate não vai mais para o log por padrão.
- **Listagem de instâncias:** a paginação é feita no banco.

**Banco criado pela 2.0.1:** o `scripts/upgrade.sh` aplica a migração `2.0.2_01` (coluna `process_join_arrival.flow_id` e índice `idx_pjob_token`).

### O que mudou da 2.0.0 (retirada) para a 2.0.1

- **Editor:** a validação do editor passou a aceitar os elementos que o motor executa. Na 2.0.0, um diagrama com evento intermediário de timer ou mensagem, evento de borda, gateway baseado em evento, subprocesso, chamada de processo, tarefa de recebimento ou de regra, ou início por mensagem aparecia com erros, e o botão «Testar» mostrava «Corrija os erros do diagrama antes de testar». O que o motor descarta (sinal, mensagem de borda, `timeCycle` fora de borda não interruptora) continua sendo erro.

### O que a versão entrega

**Motor**
- Semântica do BPMN 2.0.2 no que o motor executa: junção paralela, fim comum e `terminate`, fluxo `default`, gateways exclusivo, paralelo, inclusivo e misto, condições com `!`, parênteses e caminhos de variável.
- **Gateway baseado em evento** (timer, mensagem, tarefa de recebimento): o primeiro evento vence e apaga as esperas dos outros.
- **Estado durável:** tokens, variáveis e junções ficam no banco; ao subir, o motor retoma as instâncias em andamento. Garantia de entrega: pelo menos uma vez.
- **Eventos de erro e de tempo:** evento de borda de erro (com código) e de timer (interruptor e não interruptor), timer intermediário, e **`timeCycle`** em borda não interruptor (`R/PT1H`, `R3/PT30S`, cron).
- **Composição:** `callActivity`, subprocesso embutido e multi-instância (paralela e sequencial).
- **Mensagens:** `receiveTask` e evento intermediário com chave de correlação; **início por mensagem** (`POST /api/messages` inicia instâncias novas).
- **Tarefas humanas:** responsável, candidatos, prazo, prioridade e formulário; reivindicar, devolver, aprovar e reprovar.
- **Tabelas de decisão (DMN)** publicadas e versionadas à parte, usadas por `businessRuleTask`.
- **Operação:** repetir o nó que falhou, corrigir variáveis, mover o token, migrar de versão, cancelar com motivo; relatórios por processo e por nó; eventos `process.completed` e `process.failed`; métricas Micrometer.
- **Conectores:** e-mail (SMTP) e arquivo, com `${variavel}` e `${secret.nome}`.
- **Tarefa de IA:** Anthropic e APIs compatíveis com OpenAI, saída estruturada, confiança mínima com desvio para tarefa humana, auditoria de prompt, resposta, tokens e custo.
- **Vários nós (opcional):** posse de instância por lease no banco, adoção após queda, comandos entre nós e disparo único de gatilhos. Desligado por padrão.

**Plataforma**
- Validação de deploy: XSD do OMG mais as regras do motor (`PRF-*`); o perfil de produção recusa desenhos que o motor executaria diferente do esperado (`enforce`).
- Autenticação JWT, papéis e permissões, cofre de segredos criptografado (AES-256-GCM).
- Front React com editor bpmn-js, publicação, processos e versões, execuções com diagrama colorido, dados e cofre.

### Pacote

| Imagem | Conteúdo |
|---|---|
| `orkestrix/backend:2.0.4` | API e motor (Java 17, Spring Boot 3.5) |
| `orkestrix/frontend:2.0.4` | Front estático em nginx, com proxy de `/api` para o backend |
| `postgres:16-alpine` | Banco (imagem pública) |

Instalação e configuração: [INSTALL.md](INSTALL.md). Documentação funcional: pasta `docs/` do pacote (`docs/specs/` traz as regras com identificadores estáveis).

### Verificação desta versão

- Suíte automatizada do backend: **439 testes, 0 falhas**.
- Atualização 2.0.1 → 2.0.3 com instâncias em andamento, retorno para a 2.0.1, falha com retorno automático e nova atualização: veja `docs/VERIFICACAO.md`.

### Limites conhecidos

- **Garantia de entrega:** pelo menos uma vez. Um efeito externo (um e-mail, uma chamada REST) pode sair duas vezes depois de uma queda.
- **Não implementado:** compensação e transação, subprocesso de evento, gateway complexo, eventos de sinal e escalonamento, mensagem de borda, `sendTask`, pools e fluxos de mensagem, `timeCycle` em evento intermediário, Kafka, RabbitMQ, S3, agente de IA com ferramentas, MCP, tracing OpenTelemetry, exportador Prometheus.
- **Segurança e empresa:** sem SSO (OIDC, SAML, LDAP), sem multiempresa e sem limites por empresa.
- **Cluster:** só validado em testes automatizados, com um segundo nó simulado; ações do operador e o agendador de jobs avulsos não são encaminhados entre nós.
- **IA:** testada só com provedor simulado; nenhuma chamada real a um modelo foi feita.
- **Casos de falha** (`/api/fallout`) ficam em memória e somem ao reiniciar.
- O jar interno ainda se chama `0.1.0-SNAPSHOT`; a versão do release é a das imagens (`2.0.4`).

### Atualizando de uma versão anterior

Extraia este pacote ao lado da instalação atual e rode `scripts/upgrade.sh --from-dir ../orkestrix-<versão atual>` (detalhes em «Atualizar de versão», no `INSTALL.md`). Vale para a 2.0.1, a 2.0.2 e a 2.0.3. Instâncias criadas antes do estado durável (bancos da branch de desenvolvimento, anteriores à 2.0.0) terminam `FAILED` com o motivo ao subir, em vez de ficarem `ACTIVE` para sempre.
