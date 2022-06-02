### teste
~~~~sh
# Testar conexão Databricks com Storage Account
%sh
nslookup stgbigdatadev01us.dfs.core.windows.net
~~~~

~~~~Python
# Listar Diretórios
dbutils.fs.ls('/')
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
