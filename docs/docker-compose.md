### Testar a aplicação com Docker

Estes comandos devem ser utlizados no terminal.

1. Utilize o Docker Compose:

```bash
docker-compose up -d
```

2. Acessando a aplicação pelo terminal, pode ser feito pelo navegador no link: http://localhost:8080

```bash
curl http://localhost:8080
```

3. Acessando a aplicação, pode ser feito pelo navegador no link: http://localhost:8080/employee

```bash
curl http://localhost:8080/employee
```

4. Vamos remover os containers.

```bash
docker compose down -v
```
