## Como subir o servidor ? 

Utilizamos o docker-compose para levantar o servidor mkdocs no seu computador local

Build no docker-compose

```bash
docker-compose build server
```

Levante o servidor mkdocs com o comando abaixo

```bash
docker-compose up server
```

## Como realizar o deploy das alterações ? 

Atualmente a única maneira que temos é utilizar o mkdocs com o comando abaixo

```bash
mkdocs gh-deploy

```


