# Ellis-Alura-DevOps-GoogleCloud

## Imersão DevOps Alura Google Cloud

Aula 1 

> Github Repositories 

- https://github.com/guilhermeonrails/ellis?tab=readme-ov-file 


```
git clone https://github.com/guilhermeonrails/ellis.git
``` 

Virtual Environment:
``` 
python3 -m venv ./venv
```

Activate virtual Environment:
```
venv\Scripts\activate ( source venv/bin/activate - Codespace )
```

Install Dependencies:
```
pip install -r requirements.txt
```

Run the application:
```
uvicorn app:app --reload
```

#### Access the interactive documentation:

- http://127.0.0.1:8000/docs

## Docker

Dockerfile 
```dockerfile
FROM python:3.13.5-alpine3.22


# Define o diretório de trabalho dentro do container
WORKDIR /app


# Copia os arquivos de requisitos e instala as dependências
# Usamos no-cache-dir para evitar o cache do pip, reduzindo o tamanho da imagem
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt


# Copia o restante do código da aplicação para o diretório de trabalho
COPY . .


# Expõe a porta que a aplicação FastAPI irá rodar ( padrão é 8000 )
EXPOSE 8000


# Comando para rodar a aplicação usando uvicorn
# O host 0.0.0 permite que a aplicação seja acessível externamente ao container
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000", "--reload"]
```

# .dockerignore
```dockerignore
venv 
__pycache__  
```

Docker Library ( Python )

- https://hub.docker.com/_/python 
https://github.com/docker-library/python/blob/3fae0a14ac171f46e47d7ce41567e40524af5bcc/3.13/alpine3.22/Dockerfile 

#### Terminal Command 
```
docker build -t api . 
docker images 
docker run -p 8000:8000 api 
```

Access the interactive documentation:

- https://localhost:8000/docs ( local ) 


Aula 2

#### Terminal Command
```yaml  
docker-compose.yaml
services:
  # Serviço da sua aplicação FastAPI
 
  app:
    build: . # Constroi a imagem a partir do Dockerfile na raiz do projeto
    conteiner_name: api # Nome do container
    ports:
      - "8000:8000" # Mapeia a porta 8000 do host para a porta 8000 do container
    volumes:
      - .:/app # Monta o diretório atual ( onde está o seu código ) em /app dentro do container
               # Isso é ótimo para desenvolvimento, pois as alterações no código
               # são refletidas automaticamente no container devido ao --reload do uvicorn  
```   

Initialize the Docker Compose:
```bash 
docker compose up
```

Access the interactive documentation:

- localhost:8000/docs ( Local ) 

## Workflow ( Github Actions ) 

Docker Image 
```yaml
name: Docker Image CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:

  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v4
    - name: Build the Docker image
      run: docker build --file ellis/Dockerfile ellis --tag api:$(date +%s)
```

Aula 3 

> Build project on Google Cloud

Project Name ( alura-projeto-imersao )

> Install Google Cloud CLI ( gcloud CLI ) 

- https://cloud.google.com/sdk/docs/install 


#### Terminal Command
```bash
gcloud auth login 
gcloud config set project PROJEC_ID ( alura-projeto-imersao-... ) 
gcloud run deploy - -port=8000
```
```
Source code location (C:\Users\usuario\Desktop\Imersao DevOps Alura Google Cloud\ellis): Enter 
```
> Service name (ellis): api 
```
The following APIs are not enabled on project [alura-projeto-imersao-...]:     
        artifactregistry.googleapis.com
        cloudbuild.googleapis.com
        run.googleapis.com
```
> Do you want enable these APIs to continue (this will take a few minutes)? (Y/n)?  y
```
Enabling APIs on project [alura-projeto-imersao-...]...
Operation "operations/acf.p2-8258..." finished successfully.
Please specify a region:
 [1] africa-south1
 [2] asia-east1   
 [3] asia-east2
 [4] asia-northeast1
 [5] asia-northeast2
 [6] asia-northeast3
 [7] asia-south1
 [8] asia-south2
 [9] asia-southeast1
 [10] asia-southeast2
 [11] australia-southeast1
 [12] australia-southeast2
 [13] europe-central2
 [14] europe-north1
 [15] europe-north2
 [16] europe-southwest1
 [17] europe-west1
 [18] europe-west10
 [19] europe-west12
 [20] europe-west2
 [21] europe-west3
 [22] europe-west4
 [23] europe-west6
 [24] europe-west8
 [25] europe-west9
 [26] me-central1
 [27] me-central2
 [28] me-west1
 [29] northamerica-northeast1
 [30] northamerica-northeast2
 [31] northamerica-south1
 [32] southamerica-east1
 [33] southamerica-west1
 [34] us-central1
 [35] us-east1
 [36] us-east4
 [37] us-east5
 [38] us-south1
 [39] us-west1
 [40] us-west2
 [41] us-west3
 [42] us-west4
 [43] cancel
```

> Please enter numeric choice or text value (must exactly match list item):  32
```
To make this the default region, run `gcloud config set run/region southamerica-east1`.

Deploying from source requires an Artifact Registry Docker repository to store built containers. A repository named 
[cloud-run-source-deploy] in region [southamerica-east1] will be created.
```

> Do you want to continue (Y/n)?  y

> Allow unauthenticated invocations to [api] (y/N)? y 

```
Artifact registry
https://console.cloud.google.com/artifacts...


Image 
https://console.cloud.google.com/artifacts/docker/alura-projeto-imersao-...


cloud run
https://console.cloud.google.com/run...
```

```
Building using Dockerfile and deploying container to Cloud Run service [api] in project [alura-projeto-imersao-...] 
region [southamerica-east1]
OK Building and deploying new service... Done.
  OK Creating Container Repository...
  OK Uploading sources...
  OK Building Container... Logs are available at [https://console.cloud.google.com/cloud-build/builds;region=southamer
  ica-east1/a194e...].
  OK Creating Revision...
  OK Routing traffic...
  OK Setting IAM Policy...
Done.
Service [api] revision [api-00001-rx8] has been deployed and is serving 100 percent of traffic.
Service URL: https://api-825832030758.southamerica-east1.run.app 
```

- https://api-825832030758.southamerica-east1.run.app/docs 

