# 🐳 Prática de Laboratório: Containers, Imagens e Docker CLI

**Instituição:** IFC — Instituto Federal Catarinense (Campus Ibirama)  
**Disciplina:** Hardware e Sistemas Operacionais  
**Professor:** Msc. Rodrigo Azevedo  

---

## 🎯 Objetivos da Prática

Ao concluir este roteiro, você será capaz de:
1. Instalar e validar o ambiente Docker na máquina local.
2. Executar e gerenciar o ciclo de vida de containers via linha de comando (`Docker CLI`).
3. Compreender na prática o isolamento de processos (`Namespaces`), limites de hardware (`cgroups`) e a efemeridade do sistema de arquivos em camadas (`OverlayFS`).
4. Construir uma imagem personalizada utilizando um `Dockerfile`.
5. Persistir dados utilizando **Volumes** e orquestrar múltiplos serviços com **Docker Compose**.

---

## ⚙️ Parte 0: Instalação e Configuração Inicial do Docker

Antes de executar os comandos no terminal, siga o passo a passo abaixo conforme o sistema operacional da sua máquina (caso o laboratório já possua o Docker instalado, pule para a etapa **0.3**).

### 0.1. Instalação no Windows (Docker Desktop)
1. **Acesso ao Site Oficial:** Abra o navegador e acesse [https://www.docker.com](https://www.docker.com).
2. **Download:** Clique no botão **"Download Docker Desktop"** e selecione a versão correspondente à arquitetura do seu processador (geralmente **Download for Windows - AMD64**).
3. **Execução do Instalador:**
   * Dê um duplo clique no arquivo `Docker Desktop Installer.exe` baixado.
   * Na tela de configuração, mantenha marcada a opção **"Use WSL 2 instead of Hyper-V"** (recomendado) e clique em **Ok**.
   * Aguarde a conclusão da instalação e clique em **Close and restart** (se solicitado reiniciar o computador).
4. **Inicialização:**
   * Abra o menu Iniciar, procure por **Docker Desktop** e execute-o.
   * Aceite os termos de serviço (*Docker Subscription Service Agreement*).
   * Você pode clicar em **"Continue without signing in"** (não é obrigatório criar conta para usar localmente).
   * Aguarde o ícone da baleia na barra de tarefas parar de animar e exibir o status **Engine running** (verde).

### 0.2. Instalação no Linux (Ubuntu / Debian)
1. **Acesso à Documentação Oficial:** Caso queira consultar outras distribuições, acesse [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/).
2. **Instalação Rápida via Terminal:** Abra o terminal (`Ctrl + Alt + T`) e execute os comandos abaixo para instalar o Docker Engine e o plugin do Compose:
   ```bash
   sudo apt update
   sudo apt install -y docker.io docker-compose-v2
   ```
3. **Permissão sem `sudo` (Opcional, mas recomendado):** Adicione seu usuário ao grupo `docker` para evitar digitar senha a cada comando:
   ```bash
   sudo usermod -aG docker $USER
   newgrp docker
   ```

### 0.3. Validação da Instalação (Teste `hello-world`)
Abra o terminal (`PowerShell`, `Prompt de Comando` ou `Bash`) e execute os dois comandos abaixo para confirmar que o **Docker Client** e o **Docker Daemon** estão ativos e se comunicando:

```bash
docker --version
docker run hello-world
```

* Se aparecer a mensagem `"Hello from Docker! This message shows that your installation appears to be working correctly."`, seu ambiente está pronto para a prática!

---

## 📋 Parte 1: Ciclo de Vida e Efemeridade de Containers

Nesta etapa, você vai subir um servidor web **Nginx**, modificar seu conteúdo interno e comprovar por que containers são efêmeros.

### 1.1. Executando o primeiro container
Abra o terminal e execute o comando abaixo para baixar a imagem leve `nginx:alpine` e iniciar um container em segundo plano (*detached*):

```bash
docker run -d --name meu-servidor -p 8080:80 nginx:alpine
```

* **Verificação:** Abra o navegador e acesse `http://localhost:8080`. Capture um print da tela padrão *"Welcome to nginx!"*.
* **Inspeção:** No terminal, liste os containers ativos e verifique o tamanho da imagem baixada:
  ```bash
  docker ps
  docker images
  ```

### 1.2. Acessando o terminal interno e alterando o HTML
Abra um shell interativo dentro do container `meu-servidor`:

```bash
docker exec -it meu-servidor sh
```

Dentro do container, substitua a página inicial do Nginx pelo seu nome e turma:

```bash
echo "<h1>Laboratorio Docker - Aluno: Seu Nome Aqui</h1>" > /usr/share/nginx/html/index.html
exit
```

* Atualize a página `http://localhost:8080` no navegador para confirmar a alteração.

### 1.3. Testando a efemeridade (O teste do `docker rm`)
Agora remova o container atual e suba um novo exatamente com o mesmo comando inicial:

```bash
docker rm -f meu-servidor
docker run -d --name meu-servidor -p 8080:80 nginx:alpine
```

> **❓ Questão 1 (Para o Relatório):** Ao atualizar `http://localhost:8080`, o que aconteceu com a página que você havia editado? Explique tecnicamente por que isso ocorreu usando o conceito de **camadas do UnionFS/OverlayFS**.

*(Antes de avançar, remova o container de teste com `docker rm -f meu-servidor`).*

---

## 📊 Parte 2: Isolamento e Limites de Recursos (`cgroups`)

Nesta etapa, vamos observar como o Kernel limita o uso de memória RAM de um container.

1. Inicie um container limitando sua memória RAM máxima a **64 MB**:
   ```bash
   docker run -d --name teste-limite --memory="64m" nginx:alpine
   ```
2. Monitore o consumo em tempo real com o comando:
   ```bash
   docker stats --no-stream teste-limite
   ```

> **❓ Questão 2 (Para o Relatório):** Registre a saída do comando `docker stats` (print ou texto). Qual mecanismo do Kernel Linux garante que esse container nunca ultrapasse o teto de `64 MiB` mesmo que a aplicação interna tente alocar mais memória?

*(Limpe o container com `docker rm -f teste-limite`).*

---

## 🏗️ Parte 3: Criando sua Própria Imagem (`Dockerfile`)

Crie uma pasta chamada `pratica-docker` no seu computador e crie dois arquivos dentro dela:

### Arquivo 1: `index.html`
```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Minha Imagem Docker</title>
</head>
<body style="font-family: sans-serif; background: #062319; color: #f8fafc; padding: 40px;">
  <h1>Container Customizado — IFC Campus Ibirama</h1>
  <p>Esta pagina foi empacotada dentro de uma imagem Docker propria!</p>
</body>
</html>
```

### Arquivo 2: `Dockerfile` (sem extensão)
```dockerfile
# Define a imagem base
FROM nginx:alpine

# Copia o HTML local para o diretório padrão do servidor web
COPY index.html /usr/share/nginx/html/index.html

# Documenta a porta utilizada
EXPOSE 80
```

### Construindo e Executando a Imagem
No terminal, dentro da pasta `pratica-docker`, execute o *build* e rode um container a partir da sua nova imagem:

```bash
docker build -t site-ifc:1.0 .
docker run -d --name app-ifc -p 8081:80 site-ifc:1.0
```

* Acesse `http://localhost:8081` no navegador e tire um print comprovando o funcionamento.

---

## 🔗 Parte 4: Orquestração Multi-Container (`Docker Compose`)

Crie uma nova pasta chamada `projeto-compose` e adicione o arquivo `docker-compose.yml` abaixo, que sobe um servidor web conectado a um banco de dados **PostgreSQL** com **Volume persistente**:

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

1. Suba toda a pilha de serviços com um único comando:
   ```bash
   docker compose up -d
   ```
2. Verifique os containers e o volume criados:
   ```bash
   docker compose ps
   docker volume ls
   ```
3. Derrube os containers (`docker compose down`) e verifique novamente `docker volume ls`.

> **❓ Questão 3 (Para o Relatório):** Por que o volume `dados_postgres` continuou existindo mesmo após rodar `docker compose down`? Qual é a importância disso em ambientes de produção?

---

## 📤 Instruções de Entrega (SIGAA)

Elabore um documento em **PDF** contendo:
1. **Identificação:** Nome completo, turma e data.
2. **Evidências (Prints de Tela):**
   * Terminal exibindo o sucesso do `docker run hello-world` na Parte 0.
   * Navegador exibindo `localhost:8080` alterado na Parte 1.2.
   * Saída do terminal com `docker stats` na Parte 2.
   * Navegador exibindo sua imagem customizada em `localhost:8081` na Parte 3.
   * Saída de `docker compose ps` e `docker volume ls` na Parte 4.
3. **Respostas Discursivas:** Respostas claras e objetivas às **Questões 1, 2 e 3**.

**Submissão:** Anexar o arquivo PDF na tarefa correspondente dentro do ambiente **SIGAA** até a data estipulada em aula.