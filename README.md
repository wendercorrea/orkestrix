# Exemplo: rodar o Orkestrix com as imagens do Docker Hub

Este compose **baixa** as imagens prontas de [`wendercorrea/orkestrix`](https://hub.docker.com/repository/docker/wendercorrea/orkestrix) e sobe PostgreSQL, backend e front. Não compila nada.

As imagens ficam num repositório único, com o componente na tag: `backend-4.1.3`, `frontend-4.1.3` e `gateway-4.1.3`.

O que mudou em cada versão está em [RELEASE-NOTES.md](RELEASE-NOTES.md).

## Subir

```bash
cp .env.example .env               # troque os 4 segredos
docker compose up -d               # baixa as imagens; o repositório é público, não precisa de docker login
```

Abra http://localhost:8080. Usuário `admin`, senha igual a `ADMIN_PASSWORD` do `.env`.

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

- Só PostgreSQL, backend e front. Para o **API Gateway** (início de processos por HTTP com chave de API), use o pacote completo de release, que traz o `docker-compose.yml` com o gateway, o `catalogo.yml` e os scripts de atualização, backup e retorno de versão.
- Sem HTTPS: para expor na internet, ponha um proxy na frente (nginx, Caddy ou Traefik).
- A porta do PostgreSQL não é publicada; só o front (8080) é.
