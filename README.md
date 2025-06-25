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





# **Apache** Hadoop

Vamos agora entender **o que é o Hadoop**, desde a teoria até a prática, com foco em como ele funciona, para que serve e como você pode usá-lo (inclusive comparando com o Spark quando for útil).

---

## 🧱 O que é o Apache Hadoop?

O **Apache Hadoop** é uma **plataforma open-source para processamento e armazenamento distribuído de grandes volumes de dados**. Foi projetado para escalar de um único servidor até milhares de máquinas.

---

## 🧠 Teoria: Componentes do Hadoop

### 1. **HDFS (Hadoop Distributed File System)**

- Sistema de arquivos distribuído: divide os arquivos em blocos (geralmente 128 MB ou 256 MB).
- Armazena cópias dos blocos em **vários nós** para garantir **tolerância a falhas**.

### 2. **YARN (Yet Another Resource Negotiator)**

- Gerencia os **recursos** e **agendamento** das tarefas.
- Atua como um “sistema operacional” do cluster.

### 3. **MapReduce**

- Modelo de programação do Hadoop para **processar dados em paralelo**.
- **Map** → transforma os dados (ex: filtrar, agrupar).
- **Reduce** → agrega os resultados (ex: somar, contar).

### 4. **Hadoop Common**

- Biblioteca com utilitários usados por outros módulos Hadoop.

---

## 📦 Exemplo de arquitetura Hadoop

+---------------------+
| Cliente / Usuário |
+---------------------+
|
v
+---------------------+
| YARN (JobTracker)|
+---------------------+
|
v
+---------------------+
| MapReduce |
+---------------------+
|
v
+---------------------+
| HDFS |
+--------+------------+
|
+------+-------+
| Bloco 1 |
| Bloco 2 |
| Bloco 3 |
+--------------+


---

## 🛠️ Como usar Hadoop na prática

### 💻 1. Instalar Hadoop (modo local para estudos)

- Pode usar uma VM ou Docker
- Modo *pseudo-distribuído* (simula um cluster em uma máquina)

### 📁 2. Subir arquivos no HDFS

```bash
hdfs dfs -mkdir /dados
hdfs dfs -put local_arquivo.csv /dados/
```

### 🗺️ 3. Rodar um job MapReduce (modo clássico)

```bash
hadoop jar wordcount.jar org.apache.hadoop.examples.WordCount /dados /saida
```

---

Mas hoje em dia, você provavelmente vai usar ferramentas que rodam sobre o Hadoop, como:

| Ferramenta | Para quê serve                      |
| ---------- | --------------------------------- |
| **Hive**   | SQL sobre Hadoop                  |
| **Pig**    | Script sobre Hadoop               |
| **Spark**  | Processamento rápido (roda sobre HDFS/YARN) |
| **HBase**  | Banco NoSQL                      |
| **Sqoop**  | Importar/exportar dados entre bancos e HDFS |

---

## 🧪 Exemplo simples: Word Count com Hadoop Streaming

Você pode usar **Python com MapReduce** assim:

### mapper.py

```python
import sys
for linha in sys.stdin:
    for palavra in linha.strip().split():
        print(f"{palavra}\t1")
```


### reducer.py

```python
import sys
from collections import defaultdict

contagem = defaultdict(int)
for linha in sys.stdin:
    palavra, valor = linha.strip().split("\t")
    contagem[palavra] += int(valor)

for palavra, total in contagem.items():
    print(f"{palavra}\t{total}")
```

### Rodar com Hadoop:

```bash
hadoop jar hadoop-streaming.jar \
  -input /dados \
  -output /saida \
  -mapper mapper.py \
  -reducer reducer.py
```

## 📊 Quando usar Hadoop?

| Situação                        | Hadoop | Spark |
| ------------------------------ | ------ | ----- |
| Processar dados MUITO grandes (PB) | ✅    | ✅    |
| Processamento mais rápido       | 🚫    | ✅    |
| Streaming de dados             | 🚫    | ✅    |
| Custo de cluster menor         | ✅    | 🚫    |
| Machine Learning               | 🚫    | ✅    |

---

## 🚀 Resumo das Vantagens do Hadoop

- Armazenamento **distribuído e seguro**
- Alta **tolerância a falhas**
- Integração com muitas ferramentas Big Data
- **Open-source** e amplamente adotado

---

## 🧩 Curiosidade: Hadoop x Spark

- Spark pode rodar sobre o Hadoop usando HDFS para armazenamento e YARN como orquestrador.
- Ou seja, eles não são concorrentes diretos, e sim complementares.

---

## 🎥 Vídeos recomendados

- https://youtu.be/pcAdeZ5fLB8?si=z1TzOSMEzbLda232  
- https://www.youtube.com/watch?v=NmzNlov-HOI&list=PLFJLvHW_AAoCgaQLMu9tgYyOtc0m3-tDV  

