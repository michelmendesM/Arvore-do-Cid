# Relatório de Desempenho: BST Simples vs. Árvore AVL

Este relatório apresenta a consolidação dos testes empíricos de execução das estruturas de dados **Árvore Binária de Busca (BST Simples)** e **Árvore AVL** em compiladores online, acompanhado da análise teórica de complexidade assintótica.

---

## 1. Resumo dos Resultados Empíricos

Os testes foram executados em três plataformas online distintas (`JDoodle`, `OneCompiler` e `OnlineGDB`), medindo o limite máximo de dados processados, o consumo de memória/tamanho de saída e o tempo total de execução.

### Tabela Comparativa de Desempenho

| Estrutura | Compilador | N (Qtd. Dados) | Limitação / Tamanho Output | Tempo de Execução (s) |
| :--- | :--- | :---: | :--- | :---: |
| **BST Simples** | JDoodle | 300 | Limite do site (24 KB) | 0,02s |
| **BST Simples** | OneCompiler | 7.032 | 1048 KB (Projeto) / 256 KB (Output) | 0,80s |
| **BST Simples** | OnlineGDB | ~6.500 | Limite do site (1024 KB) | 13,40s |
| **Árvore AVL** | JDoodle | 300 | Limite do site (24 KB) | 0,02s |
| **Árvore AVL** | OneCompiler | 7.106 | 1048 KB (Projeto) / 256 KB (Output) | 0,92s |
| **Árvore AVL** | OnlineGDB | 6.843 | Limite do site (1024 KB) | 14,06s |

---

## 2. Comparativo Visual (Representação Gráfica)

### Relação de Escala: Quantidade de Dados ($N$) por Plataforma

```
JDoodle     [ 300 ]
OneCompiler [=================================== 7.032 (BST) / 7.106 (AVL) ]
OnlineGDB   [================================= ~6.500 (BST) / 6.843 (AVL) ]
```

### Tempo de Execução no Limite da Plataforma (Segundos)

```
JDoodle     (N=300)   | 0.02s (BST) / 0.02s (AVL)
OneCompiler (N~7k)    | 0.80s (BST) / 0.92s (AVL)
OnlineGDB   (N~6.5k-6.8k)| 13.40s (BST) / 14.06s (AVL)
```

---

## 3. Arquivos de Saída Gerados

Os logs detalhados e medições brutas foram organizados na pasta `/resultados`:

* [`/resultados/bst_jdoodle_300.log`](./resultados/bst_jdoodle_300.log) — Log de saída do teste BST no JDoodle ($N=300$).
* [`/resultados/bst_onecompiler_7032.log`](./resultados/bst_onecompiler_7032.log) — Log de saída do teste BST no OneCompiler ($N=7032$).
* [`/resultados/bst_onlinegdb_6500.log`](./resultados/bst_onlinegdb_6500.log) — Log de saída do teste BST no OnlineGDB ($N \approx 6500$).
* [`/resultados/avl_jdoodle_300.log`](./resultados/avl_jdoodle_300.log) — Log de saída do teste AVL no JDoodle ($N=300$).
* [`/resultados/avl_onecompiler_7106.log`](./resultados/avl_onecompiler_7106.log) — Log de saída do teste AVL no OneCompiler ($N=7106$).
* [`/resultados/avl_onlinegdb_6843.log`](./resultados/avl_onlinegdb_6843.log) — Log de saída do teste AVL no OnlineGDB ($N=6843$).

---

## 4. Análise de Complexidade Assintótica (Big-O)

### Complexidade Teórica

| Operação | BST Simples (Médio) | BST Simples (Pior Caso) | Árvore AVL (Médio) | Árvore AVL (Pior Caso) |
| :--- | :---: | :---: | :---: | :---: |
| **Busca** | $\mathcal{O}(\log n)$ | $\mathcal{O}(n)$ | $\mathcal{O}(\log n)$ | $\mathcal{O}(\log n)$ |
| **Inserção** | $\mathcal{O}(\log n)$ | $\mathcal{O}(n)$ | $\mathcal{O}(\log n)$ | $\mathcal{O}(\log n)$ |
| **Remoção** | $\mathcal{O}(\log n)$ | $\mathcal{O}(n)$ | $\mathcal{O}(\log n)$ | $\mathcal{O}(\log n)$ |
| **Espaço (Memória)** | $\mathcal{O}(n)$ | $\mathcal{O}(n)$ | $\mathcal{O}(n)$ | $\mathcal{O}(n)$ |

### Discussão: Teoria vs. Prática

1. **Impacto do Rebalanceamento da AVL:**
   * **Teoria:** A AVL garante uma altura estritamente balanceada $h \le 1.44 \log_2(n)$, prevenindo a degeneração para $\mathcal{O}(n)$ que pode ocorrer na BST simples (quando os dados entram ordenados).
   * **Prática:** A AVL apresentou um tempo ligeiramente superior ao da BST no OneCompiler ($0,92\text{s}$ vs $0,80\text{s}$) e no OnlineGDB ($14,06\text{s}$ vs $13,40\text{s}$). Esse pequeno *overhead* ocorre devido ao custo computacional extra das operações de **rotação** para manter a árvore balanceada durante a inserção.

2. **Gargalo nos Ambientes de Compilação Online:**
   * Os resultados práticos não foram limitados pelo algoritmo em si, mas sim pelos **limites de buffer I/O (saída de texto)** e **memória alocada pelas plataformas**:
     * **JDoodle:** Limite severo de saída ($24\text{ KB}$), suportando apenas $300$ dados.
     * **OneCompiler:** Limite de saída de $256\text{ KB}$, cortando o processamento em $\approx 7.000$ inserções.
     * **OnlineGDB:** Diferenças drásticas de tempo ($13,4\text{s} - 14,06\text{s}$) ocorrem pelo modo como a plataforma captura o `stdout` e restringe o consumo de memória/CPU em contêineres compartilhados.