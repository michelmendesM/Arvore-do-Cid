# Projeto Árvores - Fórmula 1
Entrega do trabalho da disciplina Estruturas de Dados II — UNICID Prof. Cid Rodrigues de Andrade

## 👥 Integrantes do Grupo
| Nome completo | RA |
| :--- | :--- |
| Kaique Milano Martinez | 43100147 |
| Michel Mendes de Moraes | 43594077 |
| Jonathan Felipe Veloso Dos Santos | 42379423 |

---

## 1. Dataset

### 1.1 Descrição 
O conjunto de dados baseia-se nos pilotos da Fórmula 1 desde sua criação em 1950 até 2024. O dataset reúne informações do resultado das corridas e os tempos das sessões de Qualificação. O formato original é CSV, contendo o volume exato de 37.254 registros compostos.

### 1.2 Fonte 
Os dados foram extraídos da base do projeto Ergast Developer API, disponível no Kaggle:* Fórmula 1 World Championship (1950-2024). [Fórmula 1 World Championship (1950-2024)](https://www.kaggle.com/datasets/rohanrao/formula-1-world-championship-1950-2020).

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



## 2. Estrutura(s) de Árvore Escolhida(s)

### 2.1 Estrutura(s)
BST Simples e Árvore AVL.

### 2.2 Justificativa técnica
As duas estruturas foram escolhidas para demonstrar desempenhos matemáticos opostos sob o mesmo dataset. A BST Simples não possui mecanismos de autoajuste, permitindo a demonstração do seu pior cenário algorítmico, degradando-se para O(n) quando dados estritamente ordenados são inseridos. A AVL foi escolhida para demonstrar a manutenção teórica da eficiência em O(log n) sob qualquer cenário, corrigindo desníveis de altura através das suas rotações mecânicas durante a carga massiva dos milhares de registros.

### 2.3 Operações implementadas (Para Entrega 2)

* **Inserção:** Adição de novos registros iterando o arquivo CSV em memória. Na BST Simples, os nós são alocados seguindo a regra estrita de roteamento binário ("menor à esquerda, maior à direita"). Na árvore AVL, a inserção engloba também a verificação de desnível e o reajuste automático da estrutura no caminho de volta da recursão.
* **Remoção:** Exclusão de um nó específico a partir da busca pela chaveComposta, com tratamento estrutural para os três casos clássicos (nó folha, nó com um filho, e nó com dois filhos utilizando o menor valor da subárvore direita). Na árvore AVL, a operação inclui o rebalanceamento em cascata até à raiz após a exclusão.
* **Busca:** Pesquisa binária utilizando a função strcmp() para roteamento. A função encontra o nó exato e imprime todo o conteúdo daquela chave, percorrendo a sua estrutura interna de dados.
* **Percursos (pré-ordem, em ordem, pós-ordem):** Funções recursivas para navegação integral da árvore. O percurso *Em ordem* (In-Order) destaca-se neste projeto, pois extrai todo o banco de dados estruturado em estrita ordem alfabética (lexicográfica).
* **Balanceamento (Árvore AVL):** Cálculo dinâmico da altura dos nós e do Fator de Balanceamento (FB). Inclui as funções de manobra mecânica para as quatro situações de desequilíbrio: Rotação Simples à Direita (LL), Rotação Simples à Esquerda (RR) e as Rotações Duplas (LR e RL), disparadas sempre que o FB for menor que -1 ou maior que 1.
* **Outra: Tratamento de Chaves Duplicadas (Lista Encadeada Interna):** Mecanismo de resolução de colisões O(1). Quando uma inserção detecta uma chave idêntica já existente, a árvore aborta a criação de um novo nó. Em vez disso, os novos dados (ex: Posição do Campeonato) são empilhados no ponteiro next (no topo da lista) dentro da "gaveta" do nó original já existente na árvore.

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

### 3.1 Objetivo dos testes
O objetivo será comparar o desempenho absoluto e a estabilidade estrutural das árvores. Os testes ocorrerão repetidas vezes na mesma máquina, alterando entre inserções aleatórias e estritamente ordenadas para provar a falha da estrutura não balanceada.

### 3.2 Cenários de teste

| # | Cenário | Entrada | Resultado esperado | Status |
| :--- | :--- | :--- | :--- | :--- |
| **1** | **Inserção Aleatória** | Leitura do arquivo CSV aleatório, executando em valores crescentes: 5.000, 10.000, 37.254. | Medição do tempo exato de resposta em segundos (utilizando o software de cronometragem LiveSplit), registro da altura final das árvores, número de comparações e contagem de rotações efetuadas pela AVL. | ☐ |
| **2** | **Inserção Ordenada** | Leitura do arquivo CSV ordenado cronologicamente e crescente: 5.000, 10.000, 37.254. | Medição do tempo de execução em segundos, altura e comparações. O objetivo prático é comparar a degradação linear da BST, provando seu pior caso O(n), versus a estabilidade estrutural da AVL mantendo O(log n). | ☐ |
| **3** | **Buscas e Remoções** | 37.254 operações exatas, sendo 80% de buscas e 20% de remoções. As chaves serão sorteadas com semente fixa para repetição. | Verificação da estabilidade estrutural (especialmente o rebalanceamento pós-remoção da AVL) e integridade dos ponteiros das árvores AVL e BST sob estresse de manipulação. | ☐ |

#### 3.3 Casos extremos (edge cases)

* **Inserção Crescente (Pior caso BST):** Dados de Fórmula 1 inseridos cronologicamente de 1950 a 2024 forçando o formato de lista.
  * **Comportamento esperado:** A BST não balanceada perderá a sua característica de ramificação, degradando-se para o formato de uma lista linear (O(n)).
* **Chaves Duplicadas:** Inserção de dados que possuem a mesma chave `Ano_Circuito_Piloto_Sessao`.
  * **Comportamento esperado:** A árvore não criará nós repetidos nem ocorrerão erros. Ela armazenará os registros subsequentes dentro de uma lista encadeada interna pertencente ao nó original (Empilhando os Status daquela sessão).
* **Busca Falha (Árvore Vazia ou Chave Inexistente):** Tentar percorrer a árvore buscando um piloto fictício ou realizar busca antes da raiz possuir dados.
  * **Comportamento esperado:** O algoritmo deverá tratar a ausência de dados em segurança, percorrendo até à profundidade máxima ou travando na raiz nula, retornando falha sem quebrar o sistema (sem Segmentation Fault).
* **Remoção da Raiz:** Solicitar a exclusão do nó principal logo após o carregamento completo.
  * **Comportamento esperado:** A árvore deverá escolher corretamente o predecessor/sucessor (+1 e -1: próximo dado ou o anterior) para assumir a raiz, atualizando os ponteiros e executando as rotações em cascata (AVL).
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
Os códigos-fonte e os datasets podem ser encontrados nos seguintes diretórios:
* **src (código):** https://github.com/michelmendesM/Arvore-do-Cid/tree/main/src
* **dataset (dados utilizados):** https://github.com/michelmendesM/Arvore-do-Cid/tree/main/dataset

Há uma linha bem abaixo do código fonte marcando onde
colocar os dados, os dados precisam ser da lista
equivalente (código fonte BST simples com dados BST)

### 4.3 Estrutura do repositório 
* `/src` → código-fonte
* `/dataset` → dataset utilizado
* `/testes` → scripts e casos de teste
* `/resultados` → saídas e relatórios de desempenho
* `README.md` → documentação principal


## 5. Referências
* Slides do professor
* Apostila de estruturas de dados
* Base de dados: [Fórmula 1 World Championship (1950-2024) - Kaggle](https://www.kaggle.com/datasets/rohanrao/formula-1-world-championship-1950-2020)
