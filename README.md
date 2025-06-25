# Apache Spark

Vamos mergulhar no **Apache Spark**, desde os conceitos teóricos até como usá-lo na prática com Python (PySpark). Você já tem uma boa base em dados, então vou te explicar de forma clara, mas completa.

---

## 🔥 O que é o Apache Spark?

O **Apache Spark** é um mecanismo de processamento de dados de código aberto, projetado para ser:

- **Rápido** (até 100x mais rápido que o Hadoop MapReduce)
- **Escalável** (funciona em clusters locais ou na nuvem)
- **Versátil** (funciona com batch, streaming, machine learning e SQL)

---

## 🧠 Conceitos Teóricos

### 1. **Componentes principais**

| Componente      | Função                                            |
| --------------- | ------------------------------------------------ |
| **Spark Core**  | Gerencia tarefas básicas: agendamento, memória, falhas. |
| **Spark SQL**   | Permite usar SQL para consultar dados.           |
| **Spark Streaming** | Processa dados em tempo real.                   |
| **MLlib**       | Biblioteca de machine learning.                   |
| **GraphX**      | Trabalha com grafos.                              |

---

### 2. **RDD vs DataFrame vs Dataset**

| Tipo                  | Explicação                                                                                 |
| --------------------- | ----------------------------------------------------------------------------------------- |
| **RDD** (Resilient Distributed Dataset) | É a base de dados distribuídos do Spark. Imutável, tolerante a falhas, mas de mais baixo nível. |
| **DataFrame**          | Tabela distribuída com colunas nomeadas. Mais eficiente e fácil de usar. Baseado em SQL.   |
| **Dataset**            | Combina os benefícios de RDDs (forte tipagem) com DataFrames (otimização). Mais usado em Scala/Java. |

---

### 3. **Lazy Evaluation**

Spark **não executa imediatamente** as operações. Ele monta um plano de execução e só executa quando uma "ação" é chamada, como `.show()`, `.collect()`, `.write()`. Isso melhora o desempenho.

---

## 🛠️ Prática com PySpark

### 🧱 1. Criando uma sessão Spark

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("Meu Projeto Spark") \
    .getOrCreate()
