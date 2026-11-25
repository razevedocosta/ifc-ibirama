# 🏗️ Trabalho Prático em Sala: Projeto Arquitetural do Sistema

* **Instituição:** Instituto Federal Catarinense (IFC) — Campus Ibirama
* **Disciplina:** Engenharia de Software
* **Professor:** Prof. Msc. Rodrigo Azevedo
* **Formato:** Equipes de 2 a 3 alunos (mesmas equipes do protótipo)
* **Ferramenta:** Quadro branco/papel A3, [Draw.io (diagrams.net)](https://app.diagrams.net/), [Excalidraw](https://excalidraw.com/) ou Figma (FigJam)

---

## 🎯 Objetivo da Atividade

Transformar o **Protótipo Visual (Interface)** desenvolvido na aula anterior em uma **Arquitetura de Software estruturada**, definindo como as telas conversam com a lógica de negócio e como os dados são persistidos no banco de dados.

---

## 📋 Cenário e Contexto

Vocês já possuem o **Documento de Requisitos (ERS)** e o **Protótipo Navegável** do sistema da equipe (ex: *IFC Achados e Perdidos*, *Cantina Escolar* ou o *Projeto Integrador* da turma). 

Agora, antes de escrever o código final, a equipe precisa entregar o **Blueprint Arquitetural** para que os programadores de *Front-end* e *Back-end* trabalhem em paralelo sem gerar código fortemente acoplado ou desorganizado.

---

## 🛠️ Entregáveis Obrigatórios (4 Etapas)

### **Etapa 1: Escolha e Justificativa do Estilo Arquitetural (10 min)**

A equipe deve escolher **1 estilo arquitetural** para o projeto (*Monólito em Camadas*, *Cliente-Servidor com BaaS/Supabase* ou *Microsserviços*) e responder em um parágrafo:

1. Qual estilo arquitetural foi escolhido pela equipe?
2. Por que ele é o mais adequado para o tamanho da equipe, prazo de entrega e infraestrutura do campus? *(Avaliação de trade-offs: simplicidade vs. complexidade).*

---

### **Etapa 2: Diagrama de Arquitetura em Camadas / Fluxo de Dados (20 min)**

Desenhe um diagrama de blocos mostrando como o sistema está dividido em **3 Camadas** (ou **Padrão MVC**):

* **Camada de Apresentação (View / Front-end):** Liste as **3 principais telas** do protótipo da equipe (ex: *Tela de Login*, *Tela de Cadastro de Item*, *Painel de Consulta*).
* **Camada de Negócio / Controle (Controller / Service):** Liste os **módulos ou controladores** responsáveis pelas regras (ex: `AutenticacaoController`, `PedidoService`, `ValidadorEstoque`).
* **Camada de Persistência (Model / Database):** Liste as **tabelas do banco de dados** (ex: `usuarios`, `pedidos`, `itens_pedido`) e o SGBD escolhido (ex: *PostgreSQL via Supabase*).
* **Setas de Comunicação:** Indique o fluxo da requisição (ex: `Tela Cadastro` $\rightarrow$ `POST /pedidos` $\rightarrow$ `PedidoController` $\rightarrow$ `INSERT INTO pedidos`).

---

### **Etapa 3: Rastreabilidade Protótipo $\rightarrow$ MVC $\rightarrow$ Banco de Dados (20 min)**

Escolha **2 funcionalidades principais (Requisitos Funcionais - RFs)** do protótipo de vocês e preencha a matriz abaixo mapeando o caminho completo de cada ação do usuário:

| Funcionalidade (RF) | Componente na Tela (**View**) | Regra / Validação (**Controller / Model**) | Ação no Banco (**Persistência**) |
| :--- | :--- | :--- | :--- |
| *Ex: [RF01] Registrar Pedido* | Botão `"Finalizar Compra"` na Tela de Carrinho | Verificar se `estoque >= qtd` e calcular valor total | `INSERT` em `pedido` + `UPDATE` em `produto` |
| **1.** *(Preencher)* | | | |
| **2.** *(Preencher)* | | | |

---

### **Etapa 4: Auditoria de Coesão e Acoplamento (10 min)**

Analise o cenário de erro abaixo e proponha a refatoração arquitetural:

> **Cenário Problema:** Um colega da equipe sugeriu colocar o código SQL (`SELECT * FROM usuarios...`) e o cálculo de multa diretamente dentro do evento `onClick` do botão da tela HTML/React.

1. Qual princípio arquitetural está sendo violado (**Alta Coesão** ou **Baixo Acoplamento**)? Explique o porquê.
2. Como a equipe resolveria esse problema separando as responsabilidades?

---

## 📊 Critérios de Avaliação

| Critério | Pontuação | O que será avaliado na entrega da equipe |
| :--- | :---: | :--- |
| **1. Justificativa Arquitetural** | **2,0 pts** | Coerência na escolha do estilo arquitetural considerando escopo, equipe e prazo (evitando *overengineering*). |
| **2. Diagrama de Camadas** | **3,0 pts** | Separação clara entre Front-end, Regras de Negócio/API e Banco de Dados, com fluxos bem identificados. |
| **3. Matriz de Rastreabilidade** | **3,0 pts** | Alinhamento exato entre os componentes do protótipo, controladores e tabelas do banco de dados. |
| **4. Análise de Coesão/Acoplamento** | **2,0 pts** | Identificação correta das violações de responsabilidades e proposta adequada de separação em camadas. |

---

## 📤 Instruções de Entrega

* Exportem o diagrama gerado como **PNG** ou **PDF** e anexem junto com as respostas das Etapas 1, 3 e 4 em um documento único (PDF ou Markdown `README.md` no repositório da equipe).
* Realizem o envio no **SIGAA** até o final da aula.