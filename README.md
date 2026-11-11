# Exemplo: rodar o Orkestrix com as imagens do Docker Hub

Este compose **baixa** as imagens prontas de [`wendercorrea/orkestrix`](https://hub.docker.com/repository/docker/wendercorrea/orkestrix) e sobe PostgreSQL, backend e front. Não compila nada.

As imagens ficam num repositório único, com o componente na tag: `backend-4.1.10`, `frontend-4.1.10` e `gateway-4.1.10`.

O que mudou em cada versão está em [RELEASE-NOTES.md](RELEASE-NOTES.md).

Endereços, login, API Gateway (Swagger e chaves) e problemas comuns: [ACESSO.md](ACESSO.md).

## Subir

```bash
cp .env.example .env               # troque os 4 segredos
docker compose up -d               # baixa as imagens; o repositório é público, não precisa de docker login
```

Abra http://localhost:8080. Usuário `admin`, senha igual a `ADMIN_PASSWORD` do `.env`.

Banco de dados (opcional, para DBeaver, psql etc.): `localhost`, porta `5432` (ou a do `POSTGRES_PORT`), banco `orkestrix`, usuário `orkestrix`, senha igual a `POSTGRES_PASSWORD` do `.env`.

## API Gateway

O gateway (porta de entrada externa para iniciar processos por HTTP, com chave de API) sobe junto, na porta **8090** (`GATEWAY_PORT`). O Swagger fica em http://localhost:8090/api/v1/docs.

No designer, o botão **Publicar no API Gateway** funciona porque o backend e o gateway usam o mesmo `ORKX_GW_MANAGEMENT_TOKEN` do `.env`. Os dois segredos novos (`ORKX_GW_API_KEY_PEPPER` e `ORKX_GW_MANAGEMENT_TOKEN`, 32 ou mais caracteres cada) vêm no `.env.example`. **Nunca troque o pepper depois**: as chaves de API já emitidas deixam de valer. A porta de gestão (8081) não é publicada.

## Conferir

```bash
docker compose ps                  # os 3 serviços "Up", backend e postgres "healthy"
docker compose logs -f orkestrix-backend
```

## Trocar de versão

Mude `ORKESTRIX_VERSION` no `.env` e rode:

```bash
docker compose pull && docker compose up -d
```

As migrações de banco só acrescentam, mas **faça backup antes** de atualizar uma instalação com dados reais.

## Parar e apagar

```bash
docker compose stop                # para, mantém os dados
docker compose down -v             # apaga também o banco (volume postgres_data)
```

## Limites deste exemplo

- Sobe PostgreSQL, backend, front e o **API Gateway**. Backup, atualização e retorno de versão com scripts ficam no pacote completo de release.
- Sem HTTPS: para expor na internet, ponha um proxy na frente (nginx, Caddy ou Traefik).
- O front fica na porta 8080 (`HTTP_PORT`). O PostgreSQL fica na porta **5432** do seu computador (`POSTGRES_PORT`), acessível só por ele (`POSTGRES_BIND=127.0.0.1`); mude a porta no `.env` se a 5432 já estiver em uso, e use `POSTGRES_BIND=0.0.0.0` só se quiser abrir o banco para a rede.
