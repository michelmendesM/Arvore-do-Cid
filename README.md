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
O conjunto de dados se baseia nos pilotos da Fórmula 1 desde sua criação em 1950 até 2024. O dataset reúne informações do resultado das corridas e tempos de voltas das sessões de Qualificações (Q1, Q2, Q3). O formato original é CSV, contendo mais de 30.000* registros compostos.

### 1.2 Fonte * numero exato
Os dados foram extraídos da base do projeto Ergast Developer API, disponível no Kaggle:* Fórmula 1 World Championship (1950-2024). https://www.kaggle.com/datasets/rohanrao/formula-1-world-championship-1950-2020?resource=download

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

### 1.4 Justificativa da escolha
O dataset tem o volume de 37.254 registros. Também, a ordem cronológica dos dados da Fórmula 1 permite criar um cenário onde a performance sequencial causará a degradação da BST Simples, gerando o contraste perfeito com o balanceamento constante da AVL.

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

CenárioEntradaResultado EsperadoStatus1Inserção AleatóriaLeitura de lotes incrementais de dados do CSV (ex: 1.000, 5.000 e até ~7.100 registros, respeitando os limites operacionais dos ambientes online).Medição do tempo de resposta, altura final das árvores, número de comparações e contagem de rotações (AVL).2Inserção OrdenadaLeitura do arquivo CSV ordenado cronologicamente e de forma crescente em lotes progressivos.Medição de tempo de execução, altura e comparações. Comparação do comportamento linear do BST $O(n)$ versus a estabilidade da AVL $O(\log n)$.☑3Buscas e RemoçõesOperações intercaladas de busca e remoção sobre o conjunto de dados carregado.Verificação da estabilidade estrutural e integridade dos ponteiros da árvore AVL e BST sob estresse de manipulação.

### 3.4 Testes de desempenho (Para Entrega 2)
Metodologia de Testes e Desempenho
Ambiente de Execução e Compiladores:
Os testes de desempenho foram conduzidos utilizando três principais compiladores online baseados em navegador JDoodle, OneCompiler e OnlineGDB, os quais possuem características de gerenciamento de recursos e execução distintas.

Ferramenta de Medição de Tempo:
O tempo de execução foi registrado com o auxílio do software de cronometragem LiveSplit (livesplit.org).

Tamanhos de Entrada:
Os testes foram estruturados de forma incremental, avaliando o comportamento dos algoritmos sob diferentes cargas de trabalho até alcançar o limite de processamento dos ambientes, com lotes de testes de 10.000 registros, mesmo não atingindo o número alocado.

Estrutura dos Dados de Entrada:
O conjunto de dados utilizado nos testes contemplava os seguintes atributos por registro:

chaveComposta (string): Junção de Temporada + Circuito + NomePiloto + TipoSessao
temporada (int): Ano da corrida
nomePiloto (string): Nome do piloto
circuito (string): Nome da pista
tipoSessao (char): 'C' para Corrida, 'Q' para Qualificação
posicaoFinal (int): Posição final na sessão
tempoVolta: Tempo registrado na sessão


### 3.5 Resultados obtidos (Para Entrega 2)
Para avaliar o desempenho prático frente à complexidade assintótica teórica, executei testes comparativos entre a Árvore Binária de Busca (BST) simples e a Árvore AVL utilizando ambientes de compilação online (JDoodle, OneCompiler e OnlineGDB), cujos logs completos estão armazenados na pasta /resultados. Devido a restrições de alocação de memória e tempo limite de execução dessas plataformas web para arquivos grandes, os testes foram conduzidos com lotes de dados de até ~7.100 registros.No OneCompiler, a BST processou 7.032 dados em 0,80s e a AVL processou 7.106 dados em 0,92s (ambas consumindo cerca de 1.048 KB de memória). Teoricamente, enquanto a BST degrada para $O(n)$ em inserções ordenadas, a AVL garante o limite logarítmico $O(\log n)$ por meio de rotações. Praticamente, para este volume de ~7.000 registros, a BST obteve um tempo ligeiramente inferior devido à ausência do overhead de cálculo de balanceamento da AVL. No entanto, as variações de desempenho entre as ferramentas (como os tempos maiores no OnlineGDB) evidenciam que o comportamento do ambiente virtual de execução impactou os testes locais.

---

## 4. Como Executar

### 4.1 Pré-requisitos (Para Entrega 2)
Linguagem: C
Versão: Versão 26H2 (Windows 11 2026 Update) ||
Chrome: 156.8078.80
Dependencias necessárias: Nenhuma

### 4.2 Instruções (Para Entrega 2)
src (código) <https://github.com/michelmendesM/Arvore-do-Cid/tree/main/src>
dataset (dados utilizados) <https://github.com/michelmendesM/Arvore-do-Cid/tree/main/dataset>

Há uma linha bem abaixo do código fonte marcando onde
colocar os dados, os dados precisam ser da lista
equivalente (código fonte BST simples com dados BST)

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
