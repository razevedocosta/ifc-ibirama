# 🐳 Prática 01: Instalação e Primeiros Passos no Docker

**Instituição:** IFC — Instituto Federal Catarinense (Campus Ibirama)  
**Disciplina:** Hardware e Sistemas Operacionais  
**Professor:** Msc. Rodrigo Azevedo

---

## 🎯 Objetivos
* Instalar o Docker Engine / Docker Desktop a partir do site oficial.
* Executar containers prontos via linha de comando (`Docker CLI`).
* Compreender na prática o isolamento de processos e a efemeridade dos containers.

---

## ⚙️ Etapa 1: Instalação do Docker

### No Windows (Docker Desktop)
1. Acesse o site oficial: <https://www.docker.com> e clique em **Download Docker Desktop** (*Windows - AMD64*).
2. Execute o instalador `Docker Desktop Installer.exe`, mantenha marcada a opção **Use WSL 2 instead of Hyper-V** e conclua a instalação.
3. Abra o **Docker Desktop** no menu Iniciar, aceite os termos e clique em **Continue without signing in**.
4. Aguarde a barra inferior ficar verde (**Engine running**).

### No Linux (Ubuntu / Debian)
1. Consulte <https://docs.docker.com/engine/install/> ou execute no terminal:
   ```bash
   sudo apt update
   sudo apt install -y docker.io
   sudo usermod -aG docker $USER
   newgrp docker
   ```

### Validação no Terminal
Abra o terminal (`PowerShell` ou `Bash`) e teste se o cliente e o servidor Docker estão funcionando:
```bash
docker --version
docker run hello-world
```

---

## 🚀 Etapa 2: Executando e Inspecionando Containers

### 1. Subir um servidor web Nginx
Baixe a imagem leve `nginx:alpine` e inicie um container mapeando a porta `8080` do seu computador para a porta `80` interna:
```bash
docker run -d --name meu-site -p 8080:80 nginx:alpine
```
* Acesse `http://localhost:8080` no navegador e verifique a página inicial.

### 2. Listar containers e monitorar recursos (`cgroups`)
Verifique os containers ativos e o consumo real de memória RAM e CPU:
```bash
docker ps
docker stats --no-stream
```

### 3. Acessar o terminal interno e testar a efemeridade
Entre no shell do container e altere a página HTML:
```bash
docker exec -it meu-site sh
echo "<h1>Laboratorio Docker - Aluno: Seu Nome</h1>" > /usr/share/nginx/html/index.html
exit
```
* Atualize `http://localhost:8080` no navegador.
* Em seguida, remova o container e suba-o novamente:
```bash
docker rm -f meu-site
docker run -d --name meu-site -p 8080:80 nginx:alpine
```

> **❓ Questão de Análise:** Ao recarregar `localhost:8080`, a alteração feita no HTML permaneceu ou sumiu? Explique por que isso acontece usando o conceito de camadas do sistema de arquivos.

---

## 📤 Entrega (SIGAA)
Envie um **PDF** contendo:
1. Print do terminal com a saída do `docker run hello-world`.
2. Print do navegador exibindo `localhost:8080` personalizado.
3. Print do `docker stats --no-stream` e a resposta da **Questão de Análise**.