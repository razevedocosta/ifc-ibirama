# ☁️ Prática 02: Imagens (`Dockerfile`) e Compose no GitHub Codespaces

**Instituição:** IFC — Instituto Federal Catarinense (Campus Ibirama)  
**Disciplina:** Hardware e Sistemas Operacionais  
**Professor:** Msc. Rodrigo Azevedo

---

## 🎯 Objetivos
* Utilizar um ambiente de desenvolvimento Linux completo na nuvem (**GitHub Codespaces**) com Docker pré-instalado.
* Construir uma imagem Docker personalizada usando um `Dockerfile`.
* Persistir dados fora do ciclo de vida do container utilizando **Volumes** e **Docker Compose**.

---

## 💻 Etapa 1: Abrindo o Ambiente no GitHub Codespaces
1. Acesse <https://github.com> e faça login na sua conta.
2. Abra o repositório da disciplina (<https://github.com/razevedocosta/ifc-ibirama>) ou qualquer repositório pessoal seu.
3. Clique no botão verde **`<> Code`**, selecione a aba **Codespaces** e clique em **Create codespace on main**.
4. Aguarde alguns segundos até o **VS Code Web** carregar com o terminal Linux na parte inferior.
5. Valide que o Docker está ativo digitando no terminal:
   ```bash
   docker --version
   ```

---

## 🏗️ Etapa 2: Criando sua Própria Imagem (`Dockerfile`)

No terminal do Codespaces, crie uma pasta de trabalho e entre nela:
```bash
mkdir pratica-imagem && cd pratica-imagem
```

Crie dois arquivos dentro de `pratica-imagem` (pelo explorador lateral do VS Code ou via terminal):

### 1. Arquivo `index.html`
```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Imagem Docker - IFC</title>
</head>
<body style="font-family: sans-serif; background: #062319; color: #f8fafc; padding: 40px;">
  <h1>Container Customizado — IFC Campus Ibirama</h1>
  <p>Construído via Dockerfile no GitHub Codespaces!</p>
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
No terminal, dentro de `pratica-imagem`, construa sua imagem e execute um container:
```bash
docker build -t site-ifc:1.0 .
docker run -d --name app-ifc -p 8081:80 site-ifc:1.0
```
* O VS Code exibirá uma notificação no canto inferior direito informando que a porta **8081** está disponível — clique em **Open in Browser** (ou acesse a aba **PORTS** ao lado do Terminal).

---

## 🔗 Etapa 3: Volumes e Orquestração (`Docker Compose`)

Volte à pasta anterior e crie a pasta do projeto Compose:
```bash
cd .. && mkdir projeto-compose && cd projeto-compose
```

Crie o arquivo `docker-compose.yml` com o conteúdo abaixo:
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
Suba os dois serviços simultaneamente e inspecione os volumes criados:
```bash
docker compose up -d
docker compose ps
docker volume ls
```
Agora derrube os containers e verifique novamente os volumes:
```bash
docker compose down
docker volume ls
```

> **❓ Questão de Análise:** Por que o volume `dados_postgres` continuou existindo após o comando `docker compose down` remover os containers `web` e `banco`?

---

## 📤 Entrega (SIGAA)
Envie um **PDF** contendo:
1. Print do VS Code Web (Codespaces) mostrando o comando `docker build` e a página aberta na porta `8081`.
2. Print do terminal exibindo `docker compose ps` e `docker volume ls` após o `down`.
3. Resposta objetiva para a **Questão de Análise**.