# 🖥️ Trabalho Prático: Sistema de Controle de Laboratório de Informática

**Instituição:** Instituto Federal Catarinense (IFC) — Campus Ibirama  
**Disciplina:** Fundamentos de Informática  
**Professor:** Prof. Msc. Rodrigo Azevedo  

---

## 🎯 Objetivo
Desenvolver uma planilha estruturada no formato de **sistema de gestão** para controlar o inventário de peças, ferramentas e o fluxo de empréstimos de um Laboratório de Informática, aplicando regras de validação de dados, busca automática, lógica condicional e visualização de indicadores.

---

## 📋 Contexto do Projeto
Você foi contratado como técnico responsável pelo Laboratório de Informática da instituição. Atualmente, cabos, placas, memórias e ferramentas estão sendo perdidos ou faltando durante as aulas práticas porque não há registro de quem retirou cada item nem alerta quando o estoque está acabando. 

Sua missão é construir um **Sistema de Controle em Planilha** funcional, intuitivo e à prova de erros de digitação.

---

## 🗂️ Arquitetura da Planilha (Abas Obrigatórias)

O arquivo deve conter **3 abas principais**, organizadas e coloridas na barra inferior:

### 1. `CADASTROS` (Tabela Mestre de Equipamentos)
Cadastre no mínimo **12 itens de hardware/laboratório** (ex: *Cabo HDMI, Cabo de Rede Cat6, Arduino Uno, Multímetro Digital, Memória RAM DDR4 8GB, SSD 240GB, Chave Phillips, Alicate Crimpador, Fonte ATX, Pasta Térmica, Pendrive Bootável, Placa de Rede USB*).

| Coluna | Descrição / Regra de Preenchimento |
| :--- | :--- |
| **Código** | Identificador único (ex: `LAB-01`, `LAB-02`...). |
| **Equipamento** | Nome e especificação básica do item. |
| **Categoria** | Classificada via **Lista Suspensa**: `Cabos`, `Componentes`, `Ferramentas`, `Periféricos`. |
| **Estoque Inicial** | Quantidade existente no início do semestre (valor numérico inteiro). |
| **Estoque Mínimo** | Quantidade limite antes de disparar o alerta de reposição. |
| **Estoque Atual** | *Calculado automaticamente por fórmula (`SOMASES`)*. |
| **Status do Estoque** | *Calculado automaticamente por fórmula (`SE` aninhado)*. |

---

### 2. `MOVIMENTACOES` (Registro de Entradas, Saídas e Empréstimos)
Crie um histórico contendo no mínimo **20 movimentações fictícias** testando todos os tipos de operação:

| Coluna | Descrição / Regra de Preenchimento |
| :--- | :--- |
| **Data** | Data da operação no formato `DD/MM/AAAA`. |
| **Código do Item** | Selecionado via **Validação de Dados (Lista Suspensa)** vinculada aos códigos da aba `CADASTROS`. |
| **Nome do Equipamento** | Preenchido **automaticamente** via fórmula (`PROCV` ou `PROCX`) ao selecionar o código. |
| **Operação** | Lista suspensa com 4 opções exatas: `Entrada`, `Saída`, `Empréstimo` e `Devolução`. |
| **Quantidade** | Número inteiro maior que zero (aplicar Validação de Dados para bloquear valores $\le 0$). |
| **Responsável** | Nome do aluno ou professor que retirou/devolveu o item. |

---

### 3. `DASHBOARD` (Painel de Controle e Consulta)
Esta é a tela inicial do sistema (*oculte as linhas de grade para dar aparência de software*). Ela deve conter:

* **Menu de Navegação:** Botões ou formas com **Hyperlinks** internos que levem diretamente às abas `CADASTROS` e `MOVIMENTACOES`.
* **Cartões de Indicadores (KPIs):**
  * Total de tipos de equipamentos cadastrados (`=CONT.VALORES(...)`).
  * Quantidade de itens com estoque crítico ou esgotado (`=CONT.SE(...)`).
  * Saldo total de itens emprestados no momento (Total de `Empréstimo` menos total de `Devolução`).
* **Buscador Rápido de Equipamento:** Uma caixa destacada onde o usuário escolhe um **Código** na lista suspensa e o sistema exibe automaticamente o **Nome**, o **Estoque Atual** e o **Status** daquele item usando `PROCV`.
* **Visualização Gráfica:** Um gráfico de colunas ou barras mostrando a quantidade em estoque por equipamento ou por categoria.

---

## ⚙️ Regras de Lógica e Fórmulas Exigidas

1. **Cálculo de Estoque Atual (`CADASTROS`):**
   Deve somar o *Estoque Inicial* + `Entrada` + `Devolução` e subtrair `Saída` + `Empréstimo` referentes exclusivamente ao código daquela linha:
   ```excel
   =D2 + SOMASES(MOVIMENTACOES!$E$2:$E$500; MOVIMENTACOES!$B$2:$B$500; A2; MOVIMENTACOES!$D$2:$D$500; "Entrada")
       + SOMASES(MOVIMENTACOES!$E$2:$E$500; MOVIMENTACOES!$B$2:$B$500; A2; MOVIMENTACOES!$D$2:$D$500; "Devolução")
       - SOMASES(MOVIMENTACOES!$E$2:$E$500; MOVIMENTACOES!$B$2:$B$500; A2; MOVIMENTACOES!$D$2:$D$500; "Saída")
       - SOMASES(MOVIMENTACOES!$E$2:$E$500; MOVIMENTACOES!$B$2:$B$500; A2; MOVIMENTACOES!$D$2:$D$500; "Empréstimo")
   ```

2. **Alerta de Status (`CADASTROS`):**
   Utilize a função `SE` aninhada para exibir:
   * `"ESGOTADO"` quando o Estoque Atual for igual a `0`.
   * `"REPOR ESTOQUE"` quando o Estoque Atual for menor ou igual ao Estoque Mínimo.
   * `"DISPONÍVEL"` nos demais casos.
   ```excel
   =SE(F2=0; "ESGOTADO"; SE(F2<=E2; "REPOR ESTOQUE"; "DISPONÍVEL"))
   ```

3. **Tratamento de Erros (`SEERRO`):**
   Todas as fórmulas de busca (`PROCV`/`PROCX`) devem estar envolvidas por `SEERRO` para não exibirem `#N/D` caso a célula de código esteja vazia:
   ```excel
   =SEERRO(PROCV(B2; CADASTROS!$A$2:$B$50; 2; FALSO); "")
   ```

4. **Formatação Condicional:**
   Aplique regras de cores automáticas na coluna de Status e no Buscador do Dashboard:
   * 🔴 **Vermelho:** `ESGOTADO`
   * 🟡 **Amarelo / Laranja:** `REPOR ESTOQUE`
   * 🟢 **Verde:** `DISPONÍVEL`

---

## 🌟 Desafio Bônus (+1,0 ponto extra)
Implemente um **Controle de Pendências por Responsável**: na aba `DASHBOARD`, crie uma pequena tabela listando 5 alunos/professores fictícios e utilize `SOMASES` para calcular automaticamente quantos itens cada pessoa ainda tem emprestados em seu nome (`Empréstimos` menos `Devoluções` daquela pessoa), destacando em vermelho quem estiver com itens pendentes.

---

## 📊 Critérios de Avaliação (Total: 10,0 pontos)

| Critério | Peso | O que será avaliado |
| :--- | :---: | :--- |
| **Precisão das Fórmulas** | **3,5 pts** | Uso correto de `SOMASES`, `PROCV`/`PROCX`, `SE`, `SEERRO`, `CONT.SE` e travamento com `$`. |
| **Integridade dos Dados** | **2,0 pts** | Uso adequado de Validação de Dados (listas suspensas e restrição de valores inválidos). |
| **Dashboard e Visual** | **2,5 pts** | Clareza da tela inicial, funcionamento do Buscador Rápido, gráfico legível e Formatação Condicional. |
| **Testes e Usabilidade** | **2,0 pts** | Coerência dos dados fictícios inseridos, navegação por links e ausência de erros (`#VALOR!`, `#N/D`, `#REF!`). |

---

## 📤 Instruções de Entrega
* **Formato:** Arquivo original (`.xlsx`) ou link editável do Google Sheets (com permissão de visualização/edição liberada para o professor).
* **Submissão:** Enviar no ambiente virtual da disciplina até a data limite estipulada em aula.