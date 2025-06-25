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
```


###  📂 2. Lendo dados (CSV, Parquet, JSON etc.)

```python
df = spark.read.option("header", True).csv("arquivo.csv")
```

### 🔍 3. Visualizando e manipulando dados

```python
df.show()
df.printSchema()
df.select("coluna1", "coluna2").filter("coluna1 > 100").show()
```

### 🧪 4. Transformações e agregações

```python
from pyspark.sql.functions import col, avg

df_filtered = df.filter(col("vendas") > 1000)
media = df_filtered.groupBy("categoria").agg(avg("vendas").alias("media_vendas"))
media.show()
```

### 📤 5. Exportando os resultados

```python
media.write.mode("overwrite").option("header", True).csv("saida/media_vendas")
```

### 💥 6. Encerrando a sessão

```python
spark.stop()
```

## ⚙️ Spark no mundo real

### Casos de uso:

- Processar milhões de linhas de logs (como você está fazendo)
- Limpar e transformar dados para dashboards (ETL)
- Machine Learning distribuído (com MLlib)
- Stream de dados com Kafka, Spark Streaming

---

## 🧠 Benefícios do Spark

- Processamento **distribuído** (usa vários núcleos/servidores)
- Suporte a **dados estruturados** e **não estruturados**
- Pode rodar localmente ou em clusters com **YARN**, **Kubernetes** ou **EMR (AWS)**

---

## 📌 Dicas práticas

- Teste localmente com arquivos pequenos (`master="local[*]"`)
- Use `cache()` ou `persist()` se precisar reaproveitar dados
- Evite usar `.collect()` em grandes volumes (puxa tudo para a memória)

---

## 🎥 Vídeos recomendados

- https://youtu.be/x9geexcW3UU?si=OXrxdOkyKsLK_5_I  
- https://youtu.be/CPYjUA2UNq8?si=kK48eiGkOJYv91P8  
- https://www.youtube.com/watch?v=VfpXMuwbQXc&list=PLjwVjYMyoFoFO-TvZuxmqe5nlfr8xG5lk  
- https://youtu.be/WwrX1YVmOyA?si=7OsGq0SWks_NUJcz  
