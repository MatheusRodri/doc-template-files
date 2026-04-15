# Configuração do ambiente PySpark local

> Guia prático para configurar um ambiente PySpark local no Windows com VS Code, uv e Jupyter Notebook.

## Pré-requisitos:

- Java 17 **Temurin 17** em: https://adoptium.net
  - Durante a instalação, certifique-se de marcar a opção "Set JAVA_HOME variable" para configurar a variável de ambiente automaticamente e selecionar a opção "Entire feature will be installed on local hard drive" para garantir que o Java seja instalado completamente.
  - Após a instalação, abra um terminal (Se tiver um terminal aberto, feche e reabra) e execute `java -version` para verificar se o Java foi instalado corretamente.
  
- Hadoop + winutils em:
  - Crie a pasta `C:\hadoop\bin`
  - Baixe o `winutils.exe` para o Hadoop 3.x em: https://github.com/cdarlint/winutils na pasta `hadoop-3.3.5/bin`
  - Coloque o `winutils.exe` na pasta `C:\hadoop\bin`
  - Configure a variável de ambiente `HADOOP_HOME` para `C:\hadoop`

- Desabilitar o alias do Python da Microsoft Store:
  - Vá para Configurações > Aplicativos > Configurações avançadas dos aplicativos > Aliases de execução de app > desative o alias do python

- Instalar o `uv`
  - Abra um terminal e execute `winget install astral-sh.uv` para instalar o `uv` usando o Windows Package Manager (winget).
  - Após a instalação, execute `uv --version` para verificar se o `uv` foi instalado corretamente.

## Configuração do projeto:

- Escolha uma das opções para criar um novo projeto ou inicializar um projeto na pasta atual:
    - Opção 1:
      - `uv init nome-do-projeto` para criar um novo projeto com o nome especificado.
      - `cd nome-do-projeto` para entrar na pasta do projeto.
    - Opção 2:
      - `uv init .` para inicializar um projeto na pasta atual.
- Crie o arquivo .python-version com o conteúdo `3.11.9` para especificar a versão do Python a ser usada no projeto.
- No terminal digite `uv add pyspark==3.5.3 delta-spark==3.2.1 ipykernel`
- Crie o arquivo `spark_session.py` na raiz do projeto com o seguinte conteúdo para configurar a sessão do Spark:

```python
import os
import sys
from pyspark.sql import SparkSession

# Garante que o Spark use o Python do ambiente virtual atual
os.environ["PYSPARK_PYTHON"] = sys.executable
os.environ["PYSPARK_DRIVER_PYTHON"] = sys.executable

def get_spark(app_name: str = "meu-projeto") -> SparkSession:
    return (
        SparkSession.builder
        .master("local[*]")
        .appName(app_name)
        .getOrCreate()
    )
```
- Importando a função `get_spark` em um notebook Jupyter:
    - Notebook na raiz do projeto:
      ```python
      from spark_session import get_spark
      spark = get_spark()
      ```
    - Notebook em uma subpasta (ex: notebooks):
      ```python
      import sys
      sys.path.append("..")  # Adiciona a pasta raiz ao caminho de importação
      from spark_session import get_spark
      spark = get_spark()
      ```
**Observação:** Todo início de notebook, certifique-se de importar a função `get_spark` 
---
Fim do guia de configuração do ambiente PySpark local. Agora você pode começar a usar o PySpark para suas análises de dados e projetos de ciência de dados!