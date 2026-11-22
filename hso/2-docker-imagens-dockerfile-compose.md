# 🏗️ Prática 02: Criação de Imagens (`Dockerfile`) e Volumes

**Instituição:** IFC — Instituto Federal Catarinense (Campus Ibirama)  
**Disciplina:** Hardware e Sistemas Operacionais  
**Professor:** Msc. Rodrigo Azevedo

---

## 🎯 Objetivos
* Construir uma imagem Docker personalizada utilizando um `Dockerfile`.
* Persistir dados fora do ciclo de vida do container usando **Volumes**.
* Subir múltiplos serviços integrados com **Docker Compose**.

---

## 📦 Etapa 1: Criando sua Própria Imagem (`Dockerfile`)

Crie uma pasta chamada `minha-imagem` e adicione os dois arquivos abaixo dentro dela:

### 1. Arquivo `index.html`
```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Imagem Customizada</title>
</head>
<body style="font-family: sans-serif; background: #062319; color: #f8fafc; padding: 40px;">
  <h1>Container Customizado — IFC Campus Ibirama</h1>
  <p>Empacotado via Dockerfile!</p>
</body>
</html>
```

### 2. Arquivo `Dockerfile` (sem extensão)
```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

### 3. Build e Execução
No terminal, dentro da pasta `minha-imagem`, construa sua imagem e inicie um container:
```bash
docker build -t site-ifc:1.0 .
docker images
docker run -d --name app-ifc -p 8081:80 site-ifc:1.0
```
* Acesse `http://localhost:8081` no navegador e confirme sua página customizada.

---

## 🔗 Etapa 2: Volumes e Orquestração (`Docker Compose`)

Crie uma pasta chamada `projeto-compose` e crie o arquivo `docker-compose.yml` abaixo para conectar um servidor web a um banco **PostgreSQL** com armazenamento persistente:

```yaml
services:
  web:
    image: nginx:alpine
    ports:
      - "8082:80"
    depends_on:
      - banco

  banco:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: aluno
      POSTGRES_PASSWORD: ifc123password
      POSTGRES_DB: laboratorio
    volumes:
      - dados_postgres:/var/lib/postgresql/data

volumes:
  dados_postgres:
```

### Executando e Testando Persistência
No terminal, dentro da pasta `projeto-compose`:
```bash
docker compose up -d
docker compose ps
docker volume ls
```
Agora derrube os containers e verifique os volumes novamente:
```bash
docker compose down
docker volume ls
```

> **❓ Questão de Análise:** Por que o volume `dados_postgres` permaneceu listado mesmo após o comando `docker compose down` remover os containers?

---

## 📤 Entrega (SIGAA)
Envie um **PDF** contendo:
1. Print do comando `docker build` e do navegador exibindo `localhost:8081`.
2. Print do terminal mostrando `docker compose ps` e `docker volume ls` após o `down`.
3. Resposta objetiva para a **Questão de Análise**.