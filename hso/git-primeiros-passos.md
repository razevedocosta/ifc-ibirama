# Guia inicial de Git Bash

Este guia apresenta os primeiros comandos para começar a usar o Git Bash
no Windows e controlar versões de um projeto.

## 1. Abrir o Git Bash

1.  Abra o menu Iniciar do Windows.
2.  Pesquise por **Git Bash**.
3.  Abra o terminal.

Confira se o Git está instalado:

``` bash
git --version
```

## 2. Configurar seu nome e e-mail

Faça esta configuração uma única vez no computador. Use o nome e o
e-mail que deseja associar aos seus commits.

``` bash
git config --global user.name "Seu Nome"
git config --global user.email "seuemail@email.com"
```

Para conferir:

``` bash
git config --list
```

## 3. Criar uma pasta para o projeto

Execute os comandos, um por vez:

``` bash
mkdir meu-projeto
cd meu-projeto
```

Confira a pasta atual e seu conteúdo:

``` bash
pwd
ls
```

## 4. Criar arquivos de exemplo

``` bash
touch index.html
touch style.css
touch script.js
```

Confira os arquivos:

``` bash
ls
```

## 5. Iniciar o repositório Git

Dentro da pasta `meu-projeto`, execute:

``` bash
git init
```

Consulte a situação dos arquivos:

``` bash
git status
```

## 6. Preparar e registrar a primeira versão

Adicione os arquivos à área de preparação:

``` bash
git add .
```

Registre a primeira versão:

``` bash
git commit -m "Primeira versão do projeto"
```

Consulte o histórico:

``` bash
git log --oneline
```

## 7. Fazer uma nova alteração

Abra um dos arquivos, por exemplo `index.html`, e escreva algum
conteúdo. Salve o arquivo.

Depois, no Git Bash, execute:

``` bash
git status
git diff
```

Prepare e registre a alteração:

``` bash
git add .
git commit -m "Atualiza arquivo HTML"
```

## Comandos básicos para consultar

  Comando                      Para que serve
  ---------------------------- ----------------------------------------
  `pwd`                        Mostra a pasta atual
  `ls`                         Lista arquivos e pastas
  `mkdir nome`                 Cria uma pasta
  `cd nome`                    Entra em uma pasta
  `cd ..`                      Volta uma pasta
  `touch arquivo`              Cria um arquivo vazio
  `clear`                      Limpa a tela
  `git init`                   Inicia um repositório
  `git status`                 Mostra a situação dos arquivos
  `git add .`                  Prepara todas as alterações
  `git commit -m "mensagem"`   Registra uma versão
  `git log --oneline`          Mostra o histórico resumido
  `git diff`                   Mostra alterações ainda não preparadas

## Exercício para praticar

1.  Crie a pasta `meu-treino`.
2.  Entre nela usando `cd`.
3.  Inicie um repositório com `git init`.
4.  Crie um arquivo chamado `anotacoes.txt`.
5.  Execute `git status`.
6.  Prepare o arquivo com `git add anotacoes.txt`.
7.  Registre a versão com `git commit -m "Adiciona anotações"`.
8.  Execute `git log --oneline` para conferir o commit.

**Dica:** execute um comando por vez e observe o resultado antes de
continuar.

> Nesta etapa inicial, o objetivo é praticar o trabalho local. GitHub,
> branches e comandos de sincronização remota serão estudados depois.
