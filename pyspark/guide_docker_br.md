# Configuração do ambiente PySpark com Docker

> Guia prático para configurar um ambiente PySpark com Docker no Windows com VS Code e Jupyter Notebook.

## Pré-requisitos:

- Docker Desktop instalado e em execução.
- Visual Studio Code (VS Code) instalado.
- Extensão Dev - Containers instalada no VS Code.

## Configuração do projeto:

- Escolha uma imagem do Pyspark no Docker Hub, uma recomendação em Abril/2026 é a imagem `apache/spark`
- Execute o comnado `docker pull apache/spark` para baixar a imagem do PySpark.
  - Escolha a tag da imagem que melhor se encaixe com suas necessidades, por exemplo, `apache/spark:4.0.2-scala2.13-java17-python3-r-ubuntu` para a versão 4.0.2 do Spark.
- Dentro da pasta do seu projeto, crie os seguintes arquivos:
  - Crie o arquivo `docker-compose.yml` com o seguinte conteúdo:

  ```yaml
    services:
      spark:
        build: .
          volumes:- .:/app
        stdin_open: true
        tty: true
  ```

  - Crie o arquivo `Dockerfile` com o seguinte conteúdo:

  ```dockerfile
  FROM apache/spark:4.0.2-scala2.13-java17-python3-r-ubuntu (Ajuste a tag conforme necessário)
  USER root
  WORKDIR /app
  COPY requirements.txt .
  RUN pip3 install --no-cache-dir -r requirements.txt
  COPY . .
  CMD ["bash"]
  ```

  - Crie o aquivo `.dockerignore` com o seguinte conteúdo:

  ```
  .git
  __pycache__
  *.pyc
  .env
  ```
  - Crie o arquivo `requirements.txt` vazio (se souber quais bibliotecas Python você precisa, adicione-as aqui).
  - Crie a imagem do Docker usando o comando `docker build -t nome-que-voce-deseja .` no terminal.
  - Inicie o contêiner usando o comando `docker-compose up -d` no terminal.
  - Abra o VS Code e use a extensão Dev - Containers para se conectar ao contêiner em execução.

--- 
Fim do guia de configuração do ambiente PySpark local. Agora você pode começar a usar o PySpark para suas análises de dados e projetos de ciência de dados!