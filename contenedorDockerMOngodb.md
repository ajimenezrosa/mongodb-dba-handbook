# Contenedor Fedora para docker MongoDB

```bash
docker compose logs mongodb
```

Como estás en Fedora, deja el volumen así en tu `docker-compose.yml`:

```yaml
services:
  mongodb:
    image: mongo:5.0
    container_name: mongodb
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: root123
    volumes:
      - ./mongo_data:/data/db:Z
```

Luego ejecuta limpio:

```bash
docker compose down
sudo rm -rf mongo_data
mkdir mongo_data
docker compose up -d mongodb
docker compose logs mongodb
docker compose ps -a
```

Si en el log ya no sale `Permission denied`, intenta:

```bash
docker exec -it mongodb mongosh -u root -p root123 --authenticationDatabase admin
```
