# Local PySpark Environment Setup

> Practical guide to set up a local PySpark environment on Windows using VS Code, uv, and Jupyter Notebook.

## Prerequisites:

- Java 17 **Temurin 17** at: https://adoptium.net
	- During installation, make sure to check the "Set JAVA_HOME variable" option to configure the environment variable automatically, and select "Entire feature will be installed on local hard drive" to ensure Java is fully installed.
	- After installation, open a terminal (if you already have one open, close and reopen it) and run `java -version` to verify that Java was installed correctly.

- Hadoop + winutils:
	- Create the folder `C:\hadoop\bin`
	- Download `winutils.exe` for Hadoop 3.x from: https://github.com/cdarlint/winutils in the `hadoop-3.3.5/bin` folder
	- Place `winutils.exe` in `C:\hadoop\bin`
	- Set the `HADOOP_HOME` environment variable to `C:\hadoop`

- Disable the Microsoft Store Python alias:
	- Go to Settings > Apps > Advanced app settings > App execution aliases > disable the python alias

- Install `uv`
	- Open a terminal and run `winget install astral-sh.uv` to install `uv` using Windows Package Manager (winget).
	- After installation, run `uv --version` to verify that `uv` was installed correctly.

## Project setup:

- Choose one of the options below to create a new project or initialize a project in the current folder:
	- Option 1:
		- `uv init project-name` to create a new project with the specified name.
		- `cd project-name` to enter the project folder.
	- Option 2:
		- `uv init .` to initialize a project in the current folder.
- Create a `.python-version` file with the content `3.11.9` to specify the Python version used in the project.
- In the terminal, run `uv add pyspark==3.5.3 delta-spark==3.2.1 ipykernel`
- Create a `spark_session.py` file at the project root with the following content to configure the Spark session:

```python
import os
import sys
from pyspark.sql import SparkSession

# Ensures Spark uses the Python from the current virtual environment
os.environ["PYSPARK_PYTHON"] = sys.executable
os.environ["PYSPARK_DRIVER_PYTHON"] = sys.executable

def get_spark(app_name: str = "my-project") -> SparkSession:
		return (
				SparkSession.builder
				.master("local[*]")
				.appName(app_name)
				.getOrCreate()
		)
```
- Importing the `get_spark` function in a Jupyter notebook:
	- Notebook at the project root:
		```python
		from spark_session import get_spark
		spark = get_spark()
		```
	- Notebook in a subfolder (for example: notebooks):
		```python
		import sys
		sys.path.append("..")  # Adds the root folder to the import path
		from spark_session import get_spark
		spark = get_spark()
		```
**Note:** At the beginning of every notebook, make sure to import the `get_spark` function.

---

End of the local PySpark environment setup guide. You can now start using PySpark for your data analysis and data science projects!
