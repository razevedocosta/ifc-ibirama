# 🐳 Prática 01: Containers na Nuvem com Play with Docker

**Instituição:** IFC — Instituto Federal Catarinense (Campus Ibirama)  
**Disciplina:** Hardware e Sistemas Operacionais  
**Professor:** Msc. Rodrigo Azevedo

---

## 🎯 Objetivos
* Executar um ambiente Linux com Docker Engine diretamente no navegador, sem necessidade de permissão de administrador na máquina local.
* Gerenciar o ciclo de vida de containers via `Docker CLI` (`run`, `ps`, `exec`, `stats`, `rm`).
* Publicar uma aplicação web na nuvem e testar a efemeridade do sistema de arquivos em camadas.

---

## 🌐 Etapa 1: Acessando o Play with Docker
1. Abra o navegador e acesse: <https://labs.play-with-docker.com/>
2. Clique em **Login** $\rightarrow$ **docker** e entre com sua conta gratuita do Docker Hub *(caso não tenha, clique em **Sign Up** no canto superior direito para criar em 1 minuto)*.
3. Após o login, clique no botão verde **START** (inicia uma sessão gratuita de 4 horas).
4. No menu lateral esquerdo, clique em **+ ADD NEW INSTANCE**. Um terminal Linux Alpine com Docker já instalado será aberto na tela.

---

## 🚀 Etapa 2: Executando e Publicando um Servidor Web

### 1. Subindo um container Nginx
No terminal do Play with Docker, execute o comando abaixo para baixar a imagem `nginx:alpine` e iniciar o container em segundo plano (*detached*):
```bash
docker run -d --name meu-site -p 8080:80 nginx:alpine
```

### 2. Acessando a aplicação no navegador
* Observe que, ao mapear a porta `8080`, aparecerá automaticamente um badge azul **`8080`** no topo da página do Play with Docker (ao lado do IP da instância).
* Clique no link **`8080`** para abrir o servidor Nginx rodando em uma nova aba.

### 3. Monitorando processos e memória (`cgroups`)
De volta ao terminal, verifique o container ativo e o consumo real de CPU e memória RAM:
```bash
docker ps
docker stats --no-stream
```

### 4. Modificando o HTML interno e testando a efemeridade
Acesse o terminal interno do container e personalize a página inicial:
```bash
docker exec -it meu-site sh
echo "<h1>IFC Campus Ibirama - Aluno: Seu Nome</h1>" > /usr/share/nginx/html/index.html
exit
```
* Atualize a aba da porta **`8080`** no navegador e tire um print da página personalizada.
* Agora remova o container e suba-o novamente:
```bash
docker rm -f meu-site
docker run -d --name meu-site -p 8080:80 nginx:alpine
```

> **❓ Questão de Análise:** Ao atualizar a página no navegador após recriar o container, a alteração no HTML permaneceu ou sumiu? Explique por que isso acontece usando o conceito de camadas (`Read-Only` vs. `Read-Write`).

---

## 📤 Entrega (SIGAA)
Envie um **PDF** contendo:
1. Print do terminal do Play with Docker mostrando `docker ps` e `docker stats --no-stream`.
2. Print do navegador exibindo a página personalizada aberta pelo link da porta `8080`.
3. Resposta objetiva para a **Questão de Análise**.