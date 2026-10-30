# Orkestrix

Motor de execução e orquestração de processos **BPMN 2.0**. Você desenha o fluxo no editor visual, publica, e o motor executa: chama APIs REST, grava e consulta banco, aplica regras de decisão (DMN), espera tarefas de usuário e timers, usa IA com saída estruturada e registra o histórico de cada instância.

> **Esta pasta é uma distribuição compilada**, sem código-fonte: só o backend pronto (`orkestrix-system/orkestrix-app.jar`), o front compilado (`orkestrix-frontend/dist/`), o compose e os scripts de operação. Para desenvolver ou gerar uma versão nova, use o repositório das fontes (`orkestrix-bpmn`); aqui não há `pom.xml`, `mvnw` nem `package.json`.

**Versão instalada:** veja o arquivo [`VERSION`](VERSION). Origem e data de geração em [`BUILD-INFO`](BUILD-INFO); hash de cada arquivo em [`SHA256SUMS`](SHA256SUMS). O que mudou em cada versão está em [release/RELEASE-NOTES.md](release/RELEASE-NOTES.md).

## O que o Orkestrix faz

- **Editor e publicação:** desenha o BPMN no navegador, valida contra o que o motor realmente executa e publica versões (`processKey + version`, status `DRAFT/ACTIVE/DEPRECATED`).
- **Execução:** inicia instâncias por REST, mensagem ou timer; suporta gateways exclusivo/paralelo/inclusivo, `ServiceTask` (chamada REST com retentativas), `ScriptTask`, `UserTask`, `BusinessRuleTask`, `Send/Receive`, subprocessos, `CallActivity` e loops.
- **Dados e integrações:** gravar/consultar banco, ETL com Data Vault, conectores (e-mail, arquivo, ler planilha, gerar arquivo), tarefa de IA e **cofre de segredos** para chaves de API e logins de rede.
- **Planilhas e arquivos:** lê `.xlsx`, `.xls` (Excel antigo, até 65.536 linhas) e `.csv` grandes, com prévia e escolha de colunas, e gera **Excel, CSV, TXT ou JSON**, na pasta compartilhada com o Windows ou em servidores de rede (SMB). Veja [Planilhas e arquivos](#planilhas-e-arquivos-pasta-compartilhada).
- **Biblioteca de processos:** todo fluxo publicado aparece organizado por tipo de processo e departamento, com busca por palavra-chave.
- **Observabilidade:** timeline da execução, requisições HTTP, variáveis e erros por instância.
- **Segurança e escala:** login com JWT e papéis (`ADMIN`, `OPERATOR`, `SYSTEM`, `APPROVER`, `VIEWER`); vários nós do motor sobre o mesmo banco (opcional, desligado por padrão).

## O que tem nesta pasta

```
orkestrix/
├── orkestrix-system/orkestrix-app.jar   # backend compilado (Spring Boot, fat jar)
├── orkestrix-frontend/dist/             # front compilado (estático, servido pelo nginx)
├── Dockerfile                           # imagem do backend a partir do .jar (não compila nada)
├── orkestrix-frontend/Dockerfile        # imagem do front a partir do dist/ (não compila nada)
├── docker-compose.yml                   # sobe PostgreSQL + backend + nginx
├── docker-compose.rede.yml              # opcional: pasta compartilhada num servidor de rede (SMB/NFS)
├── schema.sql                           # DDL completo de referência
├── data/postgres-init.sql               # script que cria o esquema no Postgres na primeira subida
├── orkestrix.sh / orkestrix.cmd         # menu interativo de operação
├── scripts/configurar-pasta-compartilhada.ps1
└── release/                             # scripts de operação (backup, restauração, migrações) e notas de versão
```

Não há `docs/`, `specs/` nem pasta de testes: essas pastas ficam só no repositório das fontes.

## Como subir

```bash
docker compose up -d --build
```

A primeira vez demora alguns minutos (as imagens são montadas a partir do `.jar` e do `dist/` já prontos, sem compilar nada). Quando os três contêineres estiverem `healthy`/`Up`, abra <http://localhost> e entre com `admin` / `admin123` (troque a senha antes de expor o sistema: veja [Usuário e senha do admin](#usuário-e-senha-do-admin)).

| O quê | Endereço |
|---|---|
| **Aplicação (front)** | <http://localhost> |
| API (pelo nginx, mesma origem) | <http://localhost/api> |
| Backend direto | <http://localhost:9000> · saúde: <http://localhost:9000/actuator/health> |
| PostgreSQL | `localhost:5435` (banco `orkestrix_prod`, usuário `orkestrix_user`) |

## Configuração

Crie um `.env` na raiz (o Compose o lê sozinho; já está no `.gitignore`) para definir a sua senha antes da primeira subida:

```bash
# .env
JWT_SECRET=um-segredo-com-32-caracteres-ou-mais
ADMIN_PASSWORD=uma-senha-forte
ORKX_SECRETS_KEY=um-texto-longo-e-secreto-que-voce-nao-vai-trocar
```

| Variável | Para que serve | Cuidado |
|---|---|---|
| `JWT_SECRET` | Chave que assina os logins; 32+ caracteres | Sem ela o `docker-compose.yml` usa um valor de exemplo — troque antes de expor o sistema; trocar depois derruba os logins abertos |
| `ADMIN_PASSWORD` | Senha do `admin`, criada na primeira subida (padrão `admin123`) | Depois de criado, mudar aqui não muda a senha |
| `ORKX_SECRETS_KEY` | Chave-mestra do cofre de segredos | **Nunca troque depois**: os segredos gravados deixam de abrir |
| `ORKX_SHARED_DIR` | Pasta do host montada em `/app/shared` (planilhas e arquivos) | Padrão `./data/files` |
| `ORKESTRIX_CLUSTER_ENABLED` | Liga vários nós do motor sobre o mesmo banco | Padrão `false` (desligado) |

A senha do PostgreSQL (`orkestrix_secure_password_change_me`) e o nome do banco/usuário estão fixos no `docker-compose.yml` desta distribuição — troque-os ali se o sistema for ficar acessível fora do seu computador.

> O `release/.env.example` e o `release/docker-compose.yml` descrevem o **pacote de release em zip** (outra forma de distribuir o Orkestrix, com imagens pré-construídas em `images/*.tar.gz` e nomes de variável diferentes, como `POSTGRES_PASSWORD` e `ORKESTRIX_JWT_SECRET`). Eles não se aplicam a esta pasta — use a tabela acima.

## Primeiros passos de uso

1. Entre em <http://localhost> com `admin` e a senha configurada.
2. No editor, desenhe um fluxo (início → tarefa → fim) ou importe um `.bpmn`.
3. **Publique** o fluxo: ele ganha uma versão. Ative a versão que deve valer.
4. **Inicie** uma instância pela tela de processos ou pela API REST, e acompanhe em *Execuções* a timeline, as requisições HTTP e as variáveis.
5. Para chamar APIs com chave, guarde-a no **Cofre** e use `${secret.nome.TOKEN}` (veja [Cofre de segredos](#cofre-de-segredos-chaves-de-api)).
6. Para trabalhar com Excel, use **Processos → Ler Excel** ou **Processos → Gerar arquivo**. Os arquivos ficam na pasta compartilhada (veja [Planilhas e arquivos](#planilhas-e-arquivos-pasta-compartilhada)).

Saúde da aplicação: <http://localhost:9000/actuator/health> (público); demais endpoints exigem login.

---

## Usuário e senha do admin

**Login padrão: `admin` / `admin123`.** Troque a senha em qualquer ambiente que não seja o seu computador.

**Como o `admin` nasce.** Na subida, o backend olha a variável `ADMIN_PASSWORD` (vira `ORKESTRIX_BOOTSTRAP_ADMIN_PASSWORD` dentro do container): se ela estiver preenchida e **ainda não existir** um usuário `admin`, ele o cria com essa senha (guardada como hash bcrypt) e os papéis `ADMIN`, `OPERATOR` e `SYSTEM`. Sem a variável, vale `admin123`. Se o `admin` já existe, o backend **não mexe** na senha dele, então mudar `ADMIN_PASSWORD` depois da primeira subida não tem efeito.

**1. Escolher a senha antes da primeira subida (banco novo).** Defina no `.env` (veja [Configuração](#configuração)) antes de `docker compose up -d --build`.

**2. Trocar a senha de um sistema que já subiu (sem perder dados).** Não existe endpoint nem tela para trocar senha, então a troca é no banco. O próprio PostgreSQL gera o hash bcrypt que o backend aceita (extensão `pgcrypto`). Troque `nova-senha` pela sua:

```bash
docker exec orkestrix-postgres psql -U orkestrix_user -d orkestrix_prod -c "CREATE EXTENSION IF NOT EXISTS pgcrypto; UPDATE users SET password_hash = crypt('nova-senha', gen_salt('bf', 10)) WHERE username = 'admin';"
```

A senha antiga deixa de valer na hora (não precisa reiniciar). Aparece `UPDATE 1` quando deu certo; `UPDATE 0` significa que o usuário `admin` não existe.

**3. Esqueci a senha.** Use o passo 2: ele não precisa da senha antiga, só do acesso ao contêiner do banco. Se preferir recomeçar do zero, `docker compose down -v` apaga o banco inteiro (**perde todos os dados**); a próxima subida cria o `admin` de novo a partir de `ADMIN_PASSWORD`.

**4. Criar outro usuário.** Também é no banco. `roles` é uma lista separada por vírgula (`ADMIN`, `OPERATOR`, `SYSTEM`, `APPROVER`, `VIEWER`):

```bash
docker exec orkestrix-postgres psql -U orkestrix_user -d orkestrix_prod -c "INSERT INTO users (username, password_hash, roles, created_at) VALUES ('operador', crypt('senha-do-operador', gen_salt('bf', 10)), 'OPERATOR', now());"
```

> O `admin123` é só um padrão de desenvolvimento e está neste README, então qualquer pessoa o conhece. Se o sistema for acessível por outras máquinas, defina `ADMIN_PASSWORD` **antes** da primeira subida (ou troque pelo passo 2) e mude também a senha do PostgreSQL no `docker-compose.yml`.

## Planilhas e arquivos (pasta compartilhada)

Os componentes **Ler planilha** e **Gerar arquivo** trabalham numa pasta que o backend enxerga em `/app/shared`. Por padrão ela é `./data/files`, ao lado do `docker-compose.yml`: copie os arquivos para lá (por exemplo `data/files/entrada/vendas.xlsx`) e, no processo, use o caminho relativo (`entrada/vendas.xlsx`). O botão **Listar arquivos…** mostra o conteúdo da pasta, como o Explorador do Windows.

Para usar outra pasta do Windows, rode na raiz desta pasta (PowerShell):

```powershell
.\scripts\configurar-pasta-compartilhada.ps1 -Caminho D:\orkestrix\arquivos -Aplicar
```

O script cria a pasta (com `entrada`, `saida` e `processados`), grava `ORKX_SHARED_DIR` no `.env` e recria o backend com a pasta montada. Acrescente `-CompartilharNaRede` (PowerShell como administrador) para outras máquinas copiarem arquivos pela rede, e `-VoltarAoLocal` para desfazer.

**Arquivo em outro servidor, com usuário e senha (SMB):** não precisa de script. Cadastre o login no **Cofre** com a opção *Usuário e senha de rede* e, no assistente, escolha *Servidor de rede (SMB)*, informe o servidor e o compartilhamento e selecione o segredo. Letra de unidade mapeada (`Z:`) não funciona, porque o container não a enxerga: use o endereço do servidor (`\\servidor\pasta`).

**NFS ou SMB aberto, montado pelo Docker** (usa [docker-compose.rede.yml](docker-compose.rede.yml)):

```powershell
.\scripts\configurar-pasta-compartilhada.ps1 -Rede -Servidor 192.168.0.10 -Compartilhamento arquivos -Aplicar
```

| Componente | O que faz |
|---|---|
| **Ler planilha** | Lê `.xlsx`, `.xls` ou `.csv` (o `.xls` é lido por inteiro, com o limite de 65.536 linhas do formato; os demais em streaming). **Carregar dados** mostra a prévia (abas, colunas com tipo, primeiras linhas) e deixa escolher as colunas. Ou **grava direto na tabela**, em lotes, ou **entrega as linhas ao passo seguinte** (até 50 mil por padrão). |
| **Gerar arquivo** | Grava **Excel, CSV, TXT ou JSON** a partir das linhas de um passo anterior ou de uma tabela de dados. Se o arquivo já existir: **atualizar**, **gerar um novo a cada execução** ou **acrescentar ao final**. |

## Cofre de segredos (chaves de API)

Para não escrever uma chave de API no fluxo, guarde-a no **Cofre** (menu *Cofre*) e use `${secret.nome.TOKEN}` onde a chave entraria (o token de 32 caracteres é gerado ao cadastrar e aparece na lista do cofre; o botão `{ }` dos campos já o insere): header, parâmetro, autenticação, URL ou corpo da chamada REST. O valor fica **criptografado** (AES-256-GCM) no banco, **nunca volta** para a tela nem pela API, e aparece mascarado (`••••••••`) nos logs e na prévia.

| Regra | Como funciona |
|---|---|
| **Dono** | Qualquer usuário logado cadastra segredos; cada segredo é de **quem o cadastrou**. |
| **Quem vê e altera** | Só o **dono** e o **administrador** (`ADMIN`). Os demais usuários nem recebem o segredo na lista. |
| **Desabilitar** | O segredo continua guardado, mas não é resolvido: o fluxo falha antes de chamar a API, com «Segredo desabilitado no cofre». |
| **Registro de atividades** | Cadastro, troca de valor, desabilitar, habilitar, remoção e tentativas negadas, com **quem** e **quando**. O valor nunca é gravado no registro. |
| **Usuário e senha de rede** | Marque esta opção para guardar o login de um servidor de arquivos (SMB). No fluxo, use `${secret.rede.nome_usuario.TOKEN}` e `${secret.rede.senha_usuario.TOKEN}`. Sem o token a referência não funciona. |

**Chave-mestra (importante).** O valor é criptografado com `ORKX_SECRETS_KEY`. **Defina a sua antes de cadastrar o primeiro segredo e nunca a troque depois:** com outra chave, os segredos já gravados aparecem como *ilegíveis*. Sem nenhuma chave, o backend usa uma aleatória a cada subida e perde o acesso aos segredos a cada reinício.

API: `GET /api/secrets`, `PUT /api/secrets/{nome}`, `PUT /api/secrets/{nome}/enabled`, `DELETE /api/secrets/{nome}`, `GET /api/secrets/log` e `GET /api/secrets/{nome}/log`.

Comandos úteis:

```bash
docker compose ps                         # estado dos contêineres
docker compose logs -f orkestrix-backend  # logs do backend
docker compose --profile dev up -d pgadmin       # opcional: pgAdmin em http://localhost:5050 (admin@orkestrix.local / admin)
docker compose down                       # para e remove os contêineres (os dados ficam no volume)
docker compose down -v                    # idem e APAGA o banco (volume postgres_data)
```

> Portas usadas: **80**, **9000** e **5435** (e 5050 com o pgAdmin). Se alguma estiver ocupada, pare o que a usa ou mude o mapeamento em `docker-compose.yml`. Os dados ficam no volume `postgres_data` e sobrevivem a `docker compose down`.

---

## Operação: menu, backup, restauração e migrações

Os scripts de operação ficam em `release/scripts/` (e `scripts/configurar-pasta-compartilhada.ps1` na raiz). Funcionam em Linux, macOS e Windows com Git Bash. O jeito mais simples é o **menu interativo**:

```bash
./orkestrix.sh        # Windows: orkestrix.cmd (usa o Git Bash)
```

Ele oferece estado do sistema, subir/parar, logs, backup, restauração e pasta compartilhada. Como esta pasta não tem o código-fonte, as opções de gerar pacote e rodar testes não aparecem. Os mesmos passos existem como scripts avulsos:

| Script | O que faz |
|---|---|
| `release/scripts/backup.sh [--completo \| --esquema]` | Gera `backups/orkestrix-<versão>-<tipo>-<data>.sql.gz` com `pg_dump` e confere o arquivo. O padrão é o **completo** |
| `release/scripts/restore.sh <arquivo> [--yes]` | Restaura um backup completo, substituindo todo o banco. Pede para digitar `RESTAURAR` |
| `release/scripts/migrate.sh [--check]` | Aplica as migrações pendentes (`--check` só lista) |
| `release/scripts/status.sh` | Estado do sistema |

**Backup.** Há dois tipos:

- **Completo** (padrão): estrutura e dados (processos, instâncias, segredos cifrados, tabelas dos fluxos). É o que serve para restaurar. Guarde junto o `.env`: sem a `ORKX_SECRETS_KEY` original o cofre do backup não abre.
- **Esquema** (`--esquema`): só a estrutura, sem dados. **Não pode ser restaurado**: `restore.sh` recusa, e o menu nem o lista.

**Restauração.** Apaga o conteúdo atual do banco e para o backend e o front; depois suba de novo com `docker compose up -d`.

**Migrações.** Ficam em `release/scripts/migrations/`. Cada uma só acrescenta (nada de `DROP` ou `RENAME`), então voltar de versão é só trocar a imagem. Regras completas em [release/scripts/migrations/README.md](release/scripts/migrations/README.md).


