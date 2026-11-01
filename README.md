# MST_EstudoComparativo

Estudo comparativo de algoritmos de **Árvore Geradora Mínima (MST)** — Prim, Kruskal e Backtracking — focado em estruturas de dados, desempenho, uso de memória e escalabilidade.

Projeto acadêmico desenvolvido em **Java**, sem dependências externas, que gera grafos aleatórios ponderados, executa benchmarks controlados e produz relatórios com gráficos comparativos.

---

## 🎯 O que o projeto se propõe a fazer

O objetivo central deste projeto é responder a perguntas como:

- Qual algoritmo de MST é mais rápido na prática?
- Como o tamanho e a densidade do grafo afetam o desempenho de cada algoritmo?
- Existe um trade-off entre tempo de execução e consumo de memória?
- O resultado (peso total da MST) é o mesmo independente do algoritmo usado? (validação de corretude)
- Por que algoritmos de força bruta (Backtracking) não escalam para grafos grandes?

Para isso, o projeto:

1. **Gera grafos** aleatórios, ponderados, não direcionados e conexos, com tamanhos de 5 a 1000 vértices e três níveis de densidade (esparso 10%, médio 50%, denso 90%).
2. **Executa os três algoritmos** sobre os mesmos grafos, repetindo cada configuração várias vezes e com aquecimento da JVM (warm-up) para reduzir ruído de medição.
3. **Mede** tempo de execução, consumo de memória, estados explorados (backtracking), podas realizadas e peso total da MST encontrada.
4. **Valida a corretude**: verifica se todos os algoritmos encontram o mesmo peso mínimo (o problema tem solução ótima única).
5. **Exporta** os dados brutos para CSV e gera gráficos SVG comparativos.

---

## 🧠 Algoritmos comparados

| Algoritmo | Estratégia | Estrutura de dados principal | Complexidade teórica | Escala bem? |
|-----------|------------|------------------------------|----------------------|-------------|
| **Prim (Lista de Adjacência)** | Guloso: cresce a árvore a partir de um vértice, sempre escolhendo a aresta mais barata que conecta um novo vértice | Fila de prioridade (Min Priority Queue) + lista de adjacência | O(E log V) | ✅ Sim — melhor em grafos densos e grandes |
| **Kruskal** | Guloso: ordena todas as arestas por peso e adiciona as mais baratas que não formam ciclo | Union-Find (Disjoint Set Union) | O(E log E) | ✅ Sim — simples, menos memória, ligeiramente mais lento em densos |
| **Backtracking** | Força bruta com podas: testa combinações de arestas recursivamente até encontrar a MST de menor peso | Union-Find sem compressão de caminho (permite rollback) | Exponencial | ❌ Não — inviável além de ~15 vértices |

### Implementações incluídas

- `src/algorithms/greedy/prim/PrimAdjList.java` — Prim com lista de adjacência e fila de prioridade mínima
- `src/algorithms/greedy/kruskal/Kruskal.java` — Kruskal com ordenação de arestas e Union-Find
- `src/algorithms/backtracking/MSTBacktracking.java` — Backtracking com podas por ciclo, arestas insuficientes e limite inferior otimista

### Estruturas de dados auxiliares

- `src/datastructures/MinPriorityQueue.java` — Fila de prioridade mínima usada pelo Prim
- `src/datastructures/UnionFind.java` — Conjuntos disjuntos usados pelo Kruskal e Backtracking

---

## 📁 Estrutura do projeto

```
MST_EstudoComparativo/
├── src/
│   ├── Main.java                      # Ponto de entrada (benchmark / analyze / help)
│   ├── algorithms/
│   │   ├── common/
│   │   │   ├── MSTAlgorithm.java      # Interface comum dos algoritmos
│   │   │   └── MSTResult.java         # Resultado da MST (arestas + peso total)
│   │   ├── greedy/
│   │   │   ├── prim/
│   │   │   │   └── PrimAdjList.java   # Prim com lista de adjacência
│   │   │   └── kruskal/
│   │   │       └── Kruskal.java       # Kruskal com Union-Find
│   │   └── backtracking/
│   │       └── MSTBacktracking.java   # Backtracking com podas
│   ├── datastructures/
│   │   ├── MinPriorityQueue.java      # Fila de prioridade mínima
│   │   └── UnionFind.java             # Union-Find (Detecta ciclos)
│   ├── graph/
│   │   ├── Graph.java                 # Representação do grafo ponderado
│   │   ├── Edge.java                  # Aresta com origem, destino e peso
│   │   ├── GraphGenerator.java        # Geração de grafos aleatórios conexos
│   │   └── GraphLoader.java           # Carregamento de grafos
│   ├── benchmark/
│   │   ├── Benchmark.java             # Motor de execução dos experimentos
│   │   ├── BenchmarkConfig.java       # Configurações do benchmark (tamanhos, densidades, repetições, seed)
│   │   └── Metrics.java               # Métricas coletadas por execução
│   ├── analysis/
│   │   ├── ResultProcessor.java       # Processa CSV bruto e gera resumo
│   │   └── ChartGenerator.java        # Gera gráficos SVG comparativos
│   └── utils/
│       ├── CSVWriter.java             # Exporta resultados para CSV
│       ├── MemoryMonitor.java         # Monitora uso de memória
│       ├── MSTValidator.java          # Valida corretude da MST
│       └── Timer.java                 # Mede tempo de execução
├── results/
│   ├── resultado_benchmark.csv        # Dados brutos de benchmark (exemplo já gerado)
│   ├── processed/
│   │   └── summary.csv                # Resumo processado (gerado pelo comando analyze)
│   └── graphs/
│       └── *.svg                      # Gráficos comparativos (gerados pelo comando analyze)
├── report/
│   ├── projeto.md                     # Descrição do projeto
│   └── conclusao.md                   # Conclusão detalhada com tabelas de resultados
├── .gitignore
├── LICENSE                            # MIT License
└── README.md                          # Este arquivo
---

## ▶️ Como executar

### Pré-requisitos

- **Java Development Kit (JDK) 8 ou superior** instalado
- Não há dependências externas (bibliotecas de terceiros). O projeto usa apenas a JDK padrão.

### 1. Compilar o projeto

Abra o terminal na raiz do projeto e compile todos os arquivos `.java`:

```bash
# No Windows (PowerShell)
javac -d out src/**/*.java src/*.java

# No Linux/macOS
javac -d out src/**/*.java src/*.java
```

Isso gerará os arquivos `.class` na pasta `out/`.

### 2. Executar o benchmark (gerar dados brutos)

```bash
# No Windows (PowerShell)
java -cp out Main benchmark

# No Linux/macOS
java -cp out Main benchmark
```

O que acontece:

- Aquece a JVM com 2 execuções de teste
- Executa Prim e Kruskal em grafos de 10, 50, 100, 500 e 1000 vértices
- Executa Backtracking apenas em grafos pequenos (5, 6, 8, 10, 12, 15 vértices)
- Testa três densidades: 10% (esparso), 50% (médio), 90% (denso)
- Repete cada configuração 10 vezes com seed fixa (42) para reprodutibilidade
- Salva os dados brutos em `results/resultado_benchmark.csv`

### 3. Analisar resultados (gerar resumo e gráficos)

```bash
# No Windows (PowerShell)
java -cp out Main analyze

# No Linux/macOS
java -cp out Main analyze
```

O que acontece:

- Processa o CSV bruto (`results/resultado_benchmark.csv`)
- Gera o resumo em `results/processed/summary.csv`
- Gera gráficos SVG comparativos em `results/graphs/`

### 4. Exibir ajuda

```bash
java -cp out Main help
```

Comandos disponíveis:

| Comando | Descrição |
|---------|-----------|
| `benchmark` | Executa os experimentos e gera o CSV bruto |
| `analyze` | Processa os resultados brutos e gera gráficos SVG |
| `help` | Exibe esta mensagem |

## 📊 Como interpretar os resultados

Após executar o benchmark e a análise, você encontrará:

1. **`results/resultado_benchmark.csv`** — dados brutos com algoritmo, vértices, arestas, densidade, tempo (ns), memória (bytes), estados explorados, podas, peso da MST e se houve sucesso.
2. **`results/processed/summary.csv`** — resumo agregado por configuração.
3. **`results/graphs/`** — gráficos SVG comparativos de tempo, memória e outros indicadores.

### Principais conclusões esperadas (baseadas nos testes já executados)

- **Corretude**: os três algoritmos encontram exatamente o mesmo peso total de MST em todas as instâncias testadas.
- **Desempenho**: Prim (Lista de Adjacência) tende a ser o mais rápido a partir de ~50 vértices, chegando a ser ~2–3× mais rápido que Kruskal em grafos densos.
- **Memória**: Kruskal consome menos memória, especialmente em grafos densos, pois o Prim mantém uma fila de prioridade que pode guardar várias cópias da mesma aresta.
- **Backtracking**: inviável para grafos maiores que ~15 vértices devido ao crescimento exponencial do espaço de busca.

Para ver a análise completa com tabelas detalhadas, consulte `report/conclusao.md`.

---

## ⚙️ Personalizando o benchmark

Você pode ajustar as configurações editando a classe `BenchmarkConfig.java` ou o método `runBenchmark()` em `Main.java`:

- `vertexCounts` — tamanhos de grafo para Prim e Kruskal (padrão: 10, 50, 100, 500, 1000)
- `backtrackingVertexCounts` — tamanhos para Backtracking (padrão: 5, 6, 8, 10, 12, 15)
- `densities` — densidades testadas (padrão: 0.1, 0.5, 0.9)
- `repetitions` — quantidade de repetições por configuração (padrão: 10)
- `warmupRuns` — execuções de aquecimento da JVM (padrão: 2)
- `seed` — semente aleatória para reprodutibilidade (padrão: 42)
- `minWeight` / `maxWeight` — intervalo de pesos das arestas (padrão: 1.0 a 100.0)
- `outputFile` — caminho do CSV de saída (padrão: `resultado_benchmark.csv`)

---

## 📄 Licença

Este projeto está licenciado sob a **MIT License**. Consulte o arquivo `LICENSE` para mais detalhes.

---

## 👨‍💻 Autores: 

Projeto feito em conjunto por mim, Arthur Marques e Antonio Winnicios para a disciplina de Técnicas de Análise de Algoritmos.


