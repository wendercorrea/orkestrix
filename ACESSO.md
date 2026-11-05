# Como acessar o Orkestrix e o API Gateway

Para quem já subiu o `docker-compose.yml` desta pasta. Os endereços abaixo valem para o computador onde o Docker roda; em outro servidor, troque `localhost` pelo endereço dele.

## Endereços

| O quê | Endereço | Variável do `.env` |
|---|---|---|
| **Orkestrix** (editor, execuções, biblioteca, cofre) | <http://localhost:8080> | `HTTP_PORT` (padrão 8080) |
| API do Orkestrix (pelo mesmo endereço do front) | <http://localhost:8080/api> | `HTTP_PORT` |
| Saúde do backend | <http://localhost:8080/actuator/health> | `HTTP_PORT` |
| **API Gateway**: Swagger | <http://localhost:8090/api/v1/docs> | `GATEWAY_PORT` (padrão 8090) |
| API Gateway: chamar uma API publicada | `http://localhost:8090/api/v1/p/<nome-da-api>` | `GATEWAY_PORT` |
| PostgreSQL (DBeaver, psql) | `localhost:5432`, banco `orkestrix`, usuário `orkestrix` | `POSTGRES_PORT`, `POSTGRES_BIND` |

Não são publicados, de propósito: o backend direto (porta 9000) e a **porta de gestão do gateway (8081)**. A porta de gestão só é usada pelo backend, dentro da rede do Docker.

Para conferir que está tudo no ar: `docker compose ps` (os 4 serviços `Up`, e backend, gateway e PostgreSQL `healthy`).

## Entrar no Orkestrix

1. Abra <http://localhost:8080>.
2. Usuário `admin`. A senha é o valor de `ADMIN_PASSWORD` do seu arquivo `.env`.

A senha do `admin` é criada na **primeira subida** com o banco vazio. Se você mudar `ADMIN_PASSWORD` no `.env` depois, a senha do banco não muda.

O menu **Gateway** só aparece para quem tem o papel `ADMIN`.

## Acessar de outro computador

- Use o endereço do servidor: `http://<ip-do-servidor>:8080` e `http://<ip-do-servidor>:8090`.
- Libere as portas 8080 e 8090 no firewall do servidor.
- O PostgreSQL só aceita conexões do próprio servidor (`POSTGRES_BIND=127.0.0.1`). Só use `POSTGRES_BIND=0.0.0.0` se for preciso abrir o banco para a rede, e com senha forte.
- Para expor na internet, ponha HTTPS na frente (nginx, Caddy ou Traefik). Este compose não faz HTTPS.

## API Gateway: do processo ao Swagger

O gateway é a porta de entrada **externa** para iniciar processos por HTTP, com chave de API. O catálogo começa **vazio**: o Swagger só mostra algo depois dos passos 1 a 3.

1. **Desenhe e publique o processo** no editor, com o evento de início marcado como **Requisição HTTP** (os exemplos do menu **Novo** servem de ponto de partida).
2. **Publique no gateway:** selecione o evento de início e clique em **Publicar no API Gateway**. Escolha o path público, os métodos e a versão. O diálogo mostra a URL pública e um exemplo `curl` para entregar a quem vai chamar.
3. **Crie o consumidor e a chave:** no menu **Gateway** crie um consumidor e emita a chave dele; depois libere a API para ele (o diálogo de publicação também lista os consumidores, para liberar ou retirar o acesso). A chave tem o formato `orkx_<id>_<64 caracteres>` e **aparece uma única vez**: copie e guarde na hora.
4. **Veja o Swagger:** abra <http://localhost:8090/api/v1/docs>, cole a chave e explore. Ele mostra só as APIs liberadas para essa chave. Atenção: **Try it out executa de verdade** e cria uma instância do processo.

### Chamar a API

A chave vai no header `X-API-Key` (na URL ela é recusada):

```bash
curl -X POST http://localhost:8090/api/v1/p/<nome-da-api> \
  -H "X-API-Key: <SUA_CHAVE_DE_API>" \
  -H "Content-Type: application/json" \
  -d '{"campo": "valor"}'
```

### Outros endereços do gateway

Todos com a chave no header `X-API-Key`:

| Para quê | Endereço |
|---|---|
| Catálogo do consumidor | `GET /api/v1/catalog` |
| OpenAPI em JSON (para importar no Postman, por exemplo) | `GET /api/v1/openapi.json` |
| Chamar uma API publicada | `POST /api/v1/p/<nome-da-api>` |

A porta de gestão (8081) não é publicada e não tem Swagger; ela é só para o designer.

Iniciar um processo **de fora** é só pelo gateway: o endereço do front (`/api/processes/<chave>/start`) responde `403` de propósito. A tela do Orkestrix inicia por outro caminho, que o usuário logado já usa.

## Problemas comuns

| Sintoma | O que fazer |
|---|---|
| "A integração com o gateway está desligada" ao publicar no designer | O backend e o gateway precisam do mesmo `ORKX_GW_MANAGEMENT_TOKEN`, e o gateway de `ORKX_GW_CATALOG_STORE=database`. O compose desta pasta já faz isso: confira se o `.env` tem `ORKX_GW_MANAGEMENT_TOKEN` e `ORKX_GW_API_KEY_PEPPER` e suba de novo com `docker compose up -d`. |
| Swagger pede chave e eu não tenho | Crie um consumidor no menu **Gateway** (passo 3). A chave só aparece na criação; para ter outra, gere uma nova. |
| Swagger abre vazio | Nenhuma API foi liberada para essa chave: publique a API (passo 2) e libere-a para o consumidor. |
| `401` ao chamar o gateway | Chave ausente, errada ou revogada. Ela vai no header `X-API-Key`. |
| `403` ao chamar `/start` pelo front (porta 8080) | É o esperado. Use o gateway (porta 8090). |
| Backend reinicia com `password authentication failed` | Já existia um volume do PostgreSQL com outra senha. Apague o volume (`docker volume rm orkestrix_postgres_data`, perde os dados dele) ou volte a senha antiga no `.env`. |
| Porta já em uso ao subir | Mude `HTTP_PORT`, `GATEWAY_PORT` ou `POSTGRES_PORT` no `.env` e rode `docker compose up -d`. |

## Cuidados

- **`ORKX_GW_API_KEY_PEPPER`: nunca troque depois.** Todas as chaves de API já emitidas deixam de valer.
- **`ORKX_SECRETS_KEY`: nunca troque depois.** Os segredos do cofre deixam de abrir.
- O `.env` guarda todos os segredos: não o versione nem o compartilhe.
