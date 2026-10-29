# Projeto Árvores - Fórmula 1
Entrega 1 de trabalho da disciplina Estruturas de Dados II — UNICID Prof. Cid Rodrigues de Andrade

## 👥 Integrantes do Grupo
| Nome completo | RA |
| :--- | :--- |
| Kaique Milano Martinez | 43100147 |
| Michel Mendes de Moraes | 43594077 |
| Jonathan Felipe Veloso Dos Santos | 42379423 |

---

## 1. Dataset

### 1.1 Descrição * valar numero exato
O conjunto de dados se baseia nos pilotos da Fórmula 1 desde sua criação em 1950 até 2024. O dataset reúne informações do resultado das corridas e tempos de voltas das sessões de Qualificações (Q1, Q2, Q3). O formato original é CSV, contendo mais de 50.000* registros compostos.

### 1.2 Fonte * numero exato
Os dados foram extraídos da base do projeto Ergast Developer API, disponível no Kaggle:* Fórmula 1 World Championship (1950-2024).

### 1.3 Estrutura dos dados 
* **chaveComposta (string):** Junção de Temporada + Circuito + NomePiloto + TipoSessao
* **temporada (int):** Ano da corrida
* **nomePiloto (string):** Nome do piloto
* **circuito (string):** Nome da pista
* **tipoSessao (char):** 'C' para Corrida, 'Q' para Qualificação
* **posicaoFinal (int):** Posição final na sessão
* **tempoVolta (float):** Tempo em segundos
  
**Ordem de Comparação e Tratamento de Duplicatas:**

* **Comparador:** É definida explicitamente pela função `strcmp()` (em ordem alfabetica) aplicada sobre a `chaveComposta`.
* **Tratamento de Chaves Repetidas:** Quando ocorrer uma chave idêntica a um nó já existente, a árvore não criará novos nós. O nó existente possuirá um ponteiro `next`, funcionando como uma lista encadeada simples interna, empilhando todos os tempos de volta referentes àquela mesma chave.

### 1.4 Justificativa da escolha * numero exato
O dataset ultrapassa o volume mínimo de 50.000* registros. Também, a ordem cronológica dos dados da Fórmula 1 permite criar um cenário onde a performance sequencial causará a degradação da BST Simples, gerando o contraste perfeito com o balanceamento constante da AVL.

---

## 2. Estrutura(s) de Árvore Escolhida(s)

### 2.1 Estrutura(s)
BST Simples e Árvore AVL.

### 2.2 Justificativa técnica
As duas foram escolhidas para demonstrar comportamentos diferentes no dataset. A BST Simples não possui mecanismos de autoajuste, permitindo a demonstração do pior caso de O(n) quando dados ordenados por temporada são inseridos. A AVL foi escolhida para a manutenção teórica do O(log n) sob qualquer cenário, corrigindo desníveis de altura através de rotações mecânicas durante a carga de milhares de registros.

### 2.3 Operações implementadas (Para Entrega 2)


### 2.4 Complexidade

**BST Simples**

| Operação | Melhor caso | Caso médio | Pior caso |
| :--- | :--- | :--- | :--- |
| **Inserção** | O(log n) | O(log n) | O(n) |
| **Busca** | O(log n) | O(log n) | O(n) |
| **Remoção** | O(log n) | O(log n) | O(n) |

**Árvore AVL**

| Operação | Melhor caso | Caso médio | Pior caso |
| :--- | :--- | :--- | :--- |
| **Inserção** | O(log n) | O(log n) | O(log n) |
| **Busca** | O(log n) | O(log n) | O(log n) |
| **Remoção** | O(log n) | O(log n) | O(log n) |

---

## 3. Plano de Testes

### 3.1 Objetivo dos testes* especificar
O objetivo será comparar o desempenho absoluto e a estabilidade estrutural das árvores. Os testes ocorrerão repetidas vezes na mesma máquina para garantir o *baseline*, alterando entre inserções aleatórias e estritamente ordenadas para provar a falha da estrutura não balanceada.

### 3.2 Cenários de teste

| # | Cenário | Entrada | Resultado esperado | Status |
| :--- | :--- | :--- | :--- | :--- |
| **1** | **Inserção Aleatória** | Leitura do arquivo CSV aleatório, executando em valores crescentes: 5.000, 10.000, 50.000 e até o fim do dataset. | Tempo de resposta imediato, altura final das árvores, número de comparações e contagem de rotações (AVL). | ☐ |
| **2** | **Inserção Ordenada** | Leitura do arquivo CSV ordenado cronologicamente e crescente: 5.000, 10.000, 50.000 e até o fim do dataset. | Medir tempo de execução, altura e comparações. O gráfico traçará Tempo de execução vs Tamanho, tentando aproximar a BST de O(n) e a AVL a manter O(log n). | ☐ |
| **3** | **Buscas e Remoções Intercaladas** | 50.000 operações exatas, sendo 80% de buscas e 20% de remoções. As chaves serão sorteadas com semente fixa para repetição. | Medição das operações intercaladas com relógio de alta resolução para conferir a estabilidade da árvore AVL sob stress de escrita e leitura. | ☐ |

### 3.3 Casos extremos (edge cases)
* **Inserção Crescente (Pior caso BST):** Dados de Fórmula 1 inseridos cronologicamente de 1950 a 2024 forçando o formato de lista.
* **Comportamento esperado:** A BST não balanceada perderá a sua característica de ramificação, degradando-se para o formato de uma lista linear (O(n)).
* **Chaves Duplicadas:** Inserção de dados que possuem a mesma chave.
* **Comportamento esperado:** A árvore não criará nós repetidos nem ocorrerá erros. Mas sim ela armazenará os registros subsequentes dentro de uma lista encadeada interna pertencente ao nó original.
* **Chave Inexistente (profundidade máxima):** Tentar percorrer a árvore inteira buscando um piloto fictício para forçar a busca de profundidade máxima.
* **Comportamento esperado:** Forçará o algoritmo a percorrer os nós da árvore até atingir a profundidade máxima antes de retornar para o usuário que há falha.
* **Remoção da Raiz:** Solicitar a exclusão do nó principal logo após o carregamento completo dos milhares de dados.
* **Comportamento esperado:** A árvore deverá escolher corretamente o próximo dado ou o anterior(+1 e -1) para assumir a raiz, atualizando os ponteiros e, no caso da AVL, executando as rotações em cascata para restaurar o balanceamento.

### 3.4 Testes de desempenho (Para Entrega 2)


### 3.5 Resultados obtidos (Para Entrega 2)


---

## 4. Como Executar

### 4.1 Pré-requisitos (Para Entrega 2)


### 4.2 Instruções (Para Entrega 2)


### 4.3 Estrutura do repositório 
* `/src` → código-fonte
* `/dataset` → dataset utilizado
* `/testes` → scripts e casos de teste
* `/resultados` → saídas e relatórios de desempenho
* `TemplateReadme´ → word que baseamos 
* `README.md` → documentação principal

---

## 5. Referências
* Slides do professor
* Apostila de estruturas de dados
* Base de dados: [Fórmula 1 World Championship (1950-2024) - Kaggle](https://www.kaggle.com/datasets/rohanrao/formula-1-world-championship-1950-2020)
