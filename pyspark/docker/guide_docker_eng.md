# Setting up a PySpark environment with Docker

> Practical guide to setting up a PySpark environment with Docker on Windows using VS Code and Jupyter Notebook.

## Prerequisites:

- Docker Desktop installed and running.
- Visual Studio Code (VS Code) installed.
- Dev Containers extension installed in VS Code.

## Project setup:

- Choose a PySpark image on Docker Hub. A recommendation as of April/2026 is the `apache/spark` image.
- Run the `docker pull apache/spark` command to download the PySpark image.
	- Choose the image tag that best matches your needs, for example, `apache/spark:4.0.2-scala2.13-java17-python3-r-ubuntu` for Spark version 4.0.2.
- Inside your project folder, create the following files:
	- Create the `docker-compose.yml` file with the following content:

	```yaml
		services:
			spark:
				build: .
					volumes:- .:/app
				stdin_open: true
				tty: true
	```

	- Create the `Dockerfile` file with the following content:

	```dockerfile
	FROM apache/spark:4.0.2-scala2.13-java17-python3-r-ubuntu (Adjust the tag as needed)
	USER root
	WORKDIR /app
	COPY requirements.txt .
	RUN pip3 install --no-cache-dir -r requirements.txt
	COPY . .
	CMD ["bash"]
	```

	- Create the `.dockerignore` file with the following content:

	```
	.git
	__pycache__
	*.pyc
	.env
	```
	- Create an empty `requirements.txt` file (if you know which Python libraries you need, add them here).
	- Build the Docker image using the command `docker build -t your-desired-name .` in the terminal.
	- Start the container using the command `docker-compose up -d` in the terminal.
	- Open VS Code and use the Dev Containers extension to connect to the running container.

---
End of the local PySpark environment setup guide. You can now start using PySpark for your data analysis and data science projects!
