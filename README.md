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








# Apache NiFi

Vamos falar agora sobre o **Apache NiFi**, uma ferramenta super útil principalmente para quem trabalha com **dados em movimento (streaming ou ETL)**.

---

## 🌊 O que é o Apache NiFi?

O **Apache NiFi** é uma **ferramenta gráfica para automação de fluxo de dados**. Ele permite que você **movimente, transforme, integre e monitore dados entre sistemas**, de forma simples e visual.

---

## 🧠 Conceito Teórico

### 🧩 O que o NiFi faz?

- Coleta dados de múltiplas fontes
- Faz transformações nos dados
- Envia os dados para outros sistemas (bancos, APIs, nuvens, etc.)
- Tudo isso com um **fluxo visual (drag-and-drop)**

---

### ⚙️ Componentes principais

| Componente | Função |
| --- | --- |
| **Processor** | Bloco que executa uma tarefa (ex: `GetFile`, `PutSQL`, `UpdateAttribute`) |
| **FlowFile** | É o "pacote de dados" que circula nos fluxos. Tem conteúdo + atributos |
| **Connection** | Liga os Processors e controla o fluxo (buffers, prioridades) |
| **Controller Service** | Configura conexões externas (bancos, APIs etc.) |
| **Process Group** | Um agrupamento de Processors (tipo uma "caixinha" de ETL) |
| **Templates** | Fluxos que podem ser exportados/importados |

---

### 📍 Como funciona o fluxo

1. **Entrada**: Coleta arquivos, dados de APIs, bancos, Kafka, etc.
2. **Processamento**: Filtra, converte, transforma, adiciona campos.
3. **Saída**: Envia os dados para bancos, S3, outro sistema, etc.

---

## 🧪 Exemplo prático simples

### 🎯 Objetivo:

Mover dados de um arquivo CSV local para um banco de dados MySQL.

### ⚙️ Processadores usados:

1. `GetFile` – Lê o arquivo CSV
2. `UpdateAttribute` – Renomeia o arquivo (opcional)
3. `ConvertRecord` – Converte de CSV para JSON ou Avro
4. `PutDatabaseRecord` – Insere os dados no banco MySQL

---

## 🧑‍💻 Como usar o NiFi na prática

### 🛠️ 1. Instalação

Você pode rodar localmente:

```bash
bash
CopiarEditar
wget https://downloads.apache.org/nifi/current/nifi-*-bin.zip
unzip nifi-*.zip
cd nifi-*/bin
./nifi.sh start

```

Acesse no navegador:

```
bash
CopiarEditar
http://localhost:8080/nifi

```

---

### 🧱 2. Montando um fluxo

Na interface gráfica:

1. Arraste o `GetFile`
2. Configure o caminho da pasta de entrada
3. Conecte ao `ConvertRecord` (para tratar o CSV)
4. Conecte ao `PutDatabaseRecord` (conexão com banco)
5. Configure a conexão JDBC no Controller Services

---

### 📈 Monitoramento

- Cada Processador mostra: quantidade de arquivos, bytes processados, taxa de erro
- Logs detalhados em cada FlowFile

---

### 🔐 Segurança

- Criptografia (TLS)
- Controle de acesso (usuários, grupos, permissões)
- Versionamento e auditoria de fluxos

---

## 🧩 Conectores prontos

O NiFi já vem com mais de **280 processadores**. Exemplos:

| Fonte/Destino | Processador |
| --- | --- |
| Arquivos locais | `GetFile`, `PutFile` |
| APIs REST | `InvokeHTTP` |
| Banco de dados | `ExecuteSQL`, `PutDatabase` |
| Kafka | `ConsumeKafka`, `PublishKafka` |
| S3, GCS, Azure Blob | `FetchS3Object`, `PutGCSObject` |
| Email | `GetSMTP`, `PutEmail` |

---

## 🧠 Quando usar o NiFi?

Use quando você quer:

✅ Criar ETLs visuais

✅ Fazer ingestão de dados em tempo real

✅ Monitorar o fluxo de dados com facilidade

✅ Orquestrar processos com vários sistemas

✅ Integrar APIs, bancos e arquivos sem código complexo

---

## ⚡ Comparativo com outras ferramentas

| Ferramenta | Foco principal | Visual? | Tempo real? |
| --- | --- | --- | --- |
| Apache NiFi | Fluxo de dados | ✅ | ✅ |
| Airflow | Orquestração de tarefas | ❌ (mais técnico) | ❌ (batch) |
| Apache Spark | Processamento em memória | ❌ | ✅ (com Structured Streaming) |
| Talend, Pentaho | ETL tradicional | ✅ | ❌ |

---

## 💡 Projeto prático sugerido

**Pipeline com NiFi:**

- Entrada: pasta com arquivos `.csv` de vendas
- Processamento: filtrar colunas e converter para JSON
- Saída: inserir os dados em um banco MySQL ou PostgreSQL

https://www.youtube.com/watch?v=UJG0zj1rPbY&list=PLeblJhqzZe1rszn0z3wqE5ETilASaJoUX

https://youtu.be/wDn7GwF-UYQ?si=uXQOvdYFQnZU1M1p

https://youtu.be/xPtUcHn3qFI?si=7YGtSyoqbxPDHurF
