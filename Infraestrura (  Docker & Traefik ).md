
	1. Caddyfile _(Simplificado para não brigar com o Traefik pelo controle do HTTPS/Domínio)_ ✅

Snippet de código

```
{
    auto_https off
}

:80 {
    root * /srv
    file_server
    encode gzip
    try_files {path} /index.html
}
```

**2. `Dockerfile`** _(Corrigido para aceitar as variáveis de ambiente no momento do build e arrumado o caminho do `chmod`)_

Dockerfile

```
# Estágio 1: Build da aplicação (Vite/React)
FROM node:18-alpine AS builder

WORKDIR /srv
COPY package*.json ./
RUN npm ci
COPY . .

# Garante que o Vite enxergue a URL da API durante o build
ARG VITE_API_BASE_URL
ENV VITE_API_BASE_URL=$VITE_API_BASE_URL

RUN npm run build

# Estágio 2: Produção com Caddy
FROM caddy:2-alpine

COPY Caddyfile /etc/caddy/Caddyfile
COPY --from=builder /srv/dist /srv

# Garante permissões na pasta correta
RUN chmod -R 755 /srv

EXPOSE 80
```


**3. `docker-compose.yml`** _(Corrigido para usar nomes de rota ÚNICOS no Traefik, evitando conflito com suas outras aplicações)_

YAML

```
services:
  web:
    container_name: classcontrol-web
    restart: always
    build:
      context: .
      dockerfile: Dockerfile
      args:
        - VITE_API_BASE_URL=${VITE_API_BASE_URL}
    env_file: .env
    environment:
      - NODE_ENV=production
      - PORT=3000
      - HOST=0.0.0.0
    labels:
      - "traefik.enable=true"
      - "traefik.docker.network=traefik-network"
      # Nomes únicos adicionando "-frontend-ui" para não conflitar:
      - "traefik.http.routers.app-classes-frontend-ui.rule=Host(`app.io-class.ssitconsulting.com.br`)"
      - "traefik.http.routers.app-classes-frontend-ui.tls=true"
      - "traefik.http.routers.app-classes-frontend-ui.entrypoints=websecure"
      - "traefik.http.routers.app-classes-frontend-ui.tls.certresolver=lets-encrypt"
      - "traefik.http.services.app-classes-frontend-ui.loadbalancer.server.port=80"
    networks:
      - traefik-network

networks:
  traefik-network:
    external: true
```

**4. Derrube o que está rodando atualmente (mesmo que quebrado):**

Bash

```
docker-compose down
```

**5. Reconstrua a imagem forçando a leitura do novo Dockerfile:**

Bash

```
docker-compose up -d --build --force-recreate
```

**6. Verifique se o container sobreviveu:**

Bash

```
docker ps | grep classcontrol-web
```

- Se aparecer **"Up X seconds/minutes"**, o container subiu perfeitamente. O Traefik já o detectou.
    
- Se não aparecer nada, rode `docker logs classcontrol-web` para vermos se faltou algo.
    

**7. Verifique os arquivos dentro do container (Prova Real):**

Bash

```
docker exec -it classcontrol-web ls -la /srv
```