### teste
~~~~sh
# Testar conexão Databricks com Storage Account
%sh
nslookup stgbigdatadev01us.dfs.core.windows.net
~~~~

~~~~Python
# Listar Diretórios
dbutils.fs.ls('/')
display(dbutils.fs.ls('dbfs:/mnt/adls-container04/userdata/'))
~~~~

~~~~sql
create table usuario
nome string,
idade int;
~~~~

~~~~sql
-- Dropar database Databricks que não está vazia
%sql
DROP DATABASE work_eficiencia CASCADE
~~~~

~~~~sql
-- Criar database (schema) Databricks em local específico no Storage Account
%sql
CREATE DATABASE work_eficiencia
LOCATION 'abfss://container04@stgbigdataprd1.dfs.core.windows.net/userdb/root/work_eficiencia'
~~~~

~~~~python
# Tamanho em bytes e particoes de tabelas por Database Databricks
import pandas as pd

database = 'work_datascience'

tables = spark.sql("show tables from work_datascience").toPandas()
dftotal = pd.DataFrame(columns=['path','name','size','modificationTime'])
for i in range (0,tables.shape[0]-1):
# for i in range (0,3):
  tbl = tables.loc[i,'tableName']
  try:
      a = dbutils.fs.ls(f"/mnt/adls-container04/userdb/root/{database}/{tbl}/")
      dftotal = dftotal.append(pd.DataFrame(a))
  except:
      None
dftotal['tabela'] = dftotal['path'].str.replace('dbfs:/mnt/adls-container04/userdb/root/work_datascience/', '').str.split('/', expand=True)[0]
dftotal = dftotal[['tabela', 'size']].groupby('tabela').agg({'count', 'sum'}).reset_index()
dftotal.columns = [dftotal.columns.droplevel(0)]
dftotal['count'] = dftotal['count']-1
dftotal.columns = ['table', 'bytes', 'partitions']
dftotal
~~~~

#### Transformar coluna Pyspark em List
~~~~python
# Transformar a coluna "tableName" da tabela "tables" em List
tableList = [x["tableName"] for x in tables.rdd.collect()]
tableList
~~~~
