# JVMAI — Hands-on Examples and Demos

Welcome — this repository contains the example projects and demo code used in the Community Day KC 2026 presentations. It's organized as a set of small, focused examples that show how to run machine learning models in JVM applications, export models from Python, and deploy simple model-as-service patterns.

This README is written assuming you are new to these tools. Follow the steps below exactly to get each demo running on Windows PowerShell. If you use macOS/Linux, commands are similar (replace backslashes and use `bash` instead of PowerShell where applicable).

Table of contents
- Overview of directories
- Prerequisites
- Quick start (run one demo now)
- Module-by-module detailed instructions
	- 01-java-native-ml
	- 02-onnx-in-jvm
	- 03-model-as-service
	- 04-spring-ai
	- 05-full-pipeline
- Troubleshooting
- Contact

Overview
--------
Top-level folders are small demos. Each demo contains a Java (Maven) project and, where relevant, a Python project for training or serving models.

Prerequisites
-------------
Install these tools before you start. Use the exact versions where possible to avoid surprises.

- Java JDK 17 or 21 (LTS recommended): download and install from Adoptium or Azul. Make sure `java -version` shows the installed JDK.
- Apache Maven (3.6+): install and ensure `mvn -v` works.
- Python 3.10+ (for the python demos and model export): ensure `python --version` works.
- Git (optional): to clone this repo if you haven't already.

If you don't have these installed on Windows, a quick install path:

PowerShell (run as Admin for installs):

```powershell
# Chocolatey (install if you want an easy package manager):
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# Then install packages (example):
choco install -y temurinjdk17 maven python git
```

Quick sanity checks after install (PowerShell):

```powershell
java -version
mvn -v
python --version
pip --version
```

Quick start — run one demo now
--------------------------------
To verify everything works, run the simple Java example in `04-spring-ai` (a Spring-based demo) or `02-onnx-in-jvm` (ONNX runtime). These commands assume you are in the repository root.

Example: build and run `02-onnx-in-jvm/java-onnx-inference` (fast smoke test)

```powershell
cd 02-onnx-in-jvm/java-onnx-inference
mvn -DskipTests package
# Run the app (if there's a main class) or run tests that demonstrate model inference
mvn -DskipTests exec:java -Dexec.mainClass=com.example.YourMainClass
```

Notes: replace `com.example.YourMainClass` with the actual main class from that module if present. If the module is a library, run the sample code or tests instead.

Module-by-module instructions
-----------------------------
Each module below includes step-by-step commands and what to expect. Open a new PowerShell window and follow the steps one module at a time.

01-java-native-ml
------------------
Purpose: Demonstrates building a Java app that uses native machine learning model artifacts (e.g., pre-trained model files packaged with the app).

How to run:

```powershell
cd 01-java-native-ml
# Build with Maven
mvn clean package -DskipTests

# If the module produces a runnable jar, run it from the target folder. Example:
cd target
java -jar java-native-ml-1.0-SNAPSHOT.jar

# If it's a Spring app, you can also use:
mvn spring-boot:run
```

What to look for: startup logs, messages about loading model files under `resources` or `target/classes/models` and a sample inference result printed to console or served via HTTP.

02-onnx-in-jvm
---------------
Purpose: Show how to run ONNX models from JVM using an ONNX runtime (Java binding) or a lightweight inference wrapper.

How to run:

```powershell
cd 02-onnx-in-jvm/java-onnx-inference
mvn clean package -DskipTests
# Run the example entrypoint if present. Many demos provide a main class or a JUnit test that executes a sample inference.
mvn -DskipTests exec:java -Dexec.mainClass=com.example.OnnxInferenceExample
```

Python training/export (optional):

```powershell
cd 02-onnx-in-jvm/python-train-export
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python train_export.py
```

What to look for: exported ONNX model file (often `.onnx`) and a short console demonstration of inference.

03-model-as-service
--------------------
Purpose: Compose a Java client and a simple Python model server to demonstrate a model-as-a-service pattern.

How to run the Python service (PowerShell):

```powershell
cd 03-model-as-service/python-ml-service
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python main.py
```

The service will typically listen on `http://localhost:5000` or `http://localhost:8000`. Check the console logs for the exact URL.

How to run the Java client:

```powershell
cd 03-model-as-service/java-spring-client
mvn clean package -DskipTests
mvn spring-boot:run
```

The Java client will make requests to the Python service and display responses. Look for HTTP request/response logs.

04-spring-ai
------------
Purpose: Spring-based examples integrating model inference with a web app or REST API.

How to run:

```powershell
cd 04-spring-ai
mvn clean package -DskipTests
mvn spring-boot:run
```

Open the browser at the address printed in the logs (commonly `http://localhost:8080`). The app may expose endpoints such as `/predict` or `/health`.

05-full-pipeline
-----------------
Purpose: A more complete pipeline including Docker compose, a Java pipeline app, and a Python model service to show an end-to-end flow.

If you have Docker Desktop installed and running (recommended for this module):

```powershell
cd 05-full-pipeline
docker-compose up --build
```

If you prefer to run services locally without Docker, run the Java and Python services individually following the instructions in their subfolders.

Common tasks and examples
-------------------------
- Build all Java projects recursively from the repo root:

```powershell
cd <repo-root>
mvn -T 1C -DskipTests clean package
```

- Search for runnable main classes:

```powershell
Get-ChildItem -Recurse -Filter "*Main*.java" | Select-Object FullName
```

Troubleshooting
---------------
- Java memory or classpath errors: add `-Xmx1g` to the `java` command or increase Maven memory via `MAVEN_OPTS`:

```powershell
$env:MAVEN_OPTS='-Xmx2g'
mvn clean package
```

- If Maven fails to download dependencies, ensure you have internet access and proxy settings configured. You can run `mvn -U clean package` to force updates.
- Python dependency errors: ensure you activated the virtual environment (`.\.venv\Scripts\Activate.ps1`) before installing requirements.
- Port conflicts: if a service won't start because a port is in use, either stop the conflicting service or change the port via application properties or environment variables.

Contact
-------
If you need help running these demos, you can reach out to the speaker:

- Email: suryarao.r@gmail.com
- Phone: 913-413-6752
- LinkedIn: https://www.linkedin.com/in/suryarayarao/

Extras and next steps
---------------------
- Want a guided script that builds and runs every demo automatically? Tell me which OS you prefer and I can add a `run-all` PowerShell script.
- Want Dockerfiles standardized or a one-click demo for the whole deck? I can add a `demo/` folder with scripts and a small README for CI-friendly runs.

---
Updated: May 28, 2026

