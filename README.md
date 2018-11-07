# docker-stellar-core
stellar-core dockerization

Usages:
## Init the stellar-core-db postgres
```bash
$ docker run \
  --network="compose_default" \
  -v ${WORKSPACE}/etc/stellar-core.cfg:/etc/stellar-core.cfg \
  stellar-core:latest /bin/bash -c "stellar-core --conf /etc/stellar-core.cfg --newdb"
```
## docker-compose.yml
```yml
version: '3'
services:
  stellar-core:
    container_name: stellar_core
    image: stellar-core:latest
    ports:
      - 11626:11626
    volumes:
      - ${CONFIG_ROOT}/stellar-core.cfg:/etc/stellar-core.cfg:ro
    command:
      - stellar-core
      - --conf
      - /etc/stellar-core.cfg
    links:
      - stellar-core-db
  stellar-core-db:
    container_name: fox_stellar_core_db
    hostname: fox_stellar_core_db
    image: postgres:11.0-alpine
    ports:
      - 5432:5432
    environment:
      - POSTGRES_DB=core
      - POSTGRES_PASSWORD=core
      - POSTGRES_USER=core
    volumes:
      - ${DATA_ROOT}/stellar-core-db:/var/lib/postgresql/data:z
```

