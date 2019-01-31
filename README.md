# docker
Projeto do curso de Docker

Porta do servidor web no play with docker: 32768

Este Dockerfile constrói um container baseado no node que irá servir os arquivos estáticos (HTML, CSS) e também realizará a comunicação com o container mongo (base de dados).

```Dockerfile
FROM node:latest # constrói a partir da última versãp do node disponibilizada no dockerhub, caso ela não esteja presente localmente 
MAINTAINER merciof 
ENV NODE_ENV=development
COPY . /var/www # copia os arquivos para o diretório /var/www do container a ser construído
WORKDIR /var/www # define o diretório de trabalho como /var/www
RUN npm install # executa o comando npm install e instala as dependências definidas no arquivo package.json
ENTRYPOINT ["npm", "start"] # comando executado ao container ser levantado, ele executa o arquivo server.js 
EXPOSE 3000 # expõe a porta 3000
```

```yml
version: '2' # especifica a versão do Compose
services: # descrição dos serviços a serem construídos
  mongodb:
    image: mongo # nome da imagem a ser utilizada, será baixada a última versão do docker hub
    networks: # especificação das redes locais as quais este container (serviço) estará conectado
      - minha-rede 
  node:
    build: # contrução de imagem a partir de Dockerfile
      dockerfile: ./Dockerfile
      context: . 
    image: merciof/ecommerce-livro # nome da imagem a ser construída
    container_name: rural-books #n nome do container a ser subido
    ports:
      - "80:3000" # a porta 80 do servidor web estará associada a porta 3000 exposta no container
    networks: 
      - minha-rede
    depends_on:
      - mongodb
networks:
  minha-rede:
    driver: bridge
```


docker-compose build
mongodb uses an image, skipping
Building node
Step 1/8 : FROM node:latest
latest: Pulling from library/node
ab1fc7e4bf91: Pull complete
35fba333ff52: Pull complete
f0cb1fa13079: Pull complete
3d1dd648b5ad: Pull complete
49f7a0920ce1: Pull complete
1d199f738c5f: Pull complete
4be79c9aac1d: Pull complete
38138903a70b: Pull complete
Digest: sha256:36aeeb280a85cff4e888c49bf0dfdb86084e6158ae8fd106e397542edaa08d0a
Status: Downloaded newer image for node:latest
 ---> 8c67bfd7b95b
Step 2/8 : MAINTAINER merciof
 ---> Running in 919823b12bb4
Removing intermediate container 919823b12bb4
 ---> 2594b0ecab04
Step 3/8 : ENV NODE_ENV=development
 ---> Running in 0e0f6bc87a20
Removing intermediate container 0e0f6bc87a20
 ---> a6161181e491
Step 4/8 : COPY . /var/www
 ---> 44cc2065b295
Step 5/8 : WORKDIR /var/www
 ---> Running in 9f415c399fe9
Removing intermediate container 9f415c399fe9
 ---> 4545996f84ab
Step 6/8 : RUN npm install
 ---> Running in 561c5f7e74ed
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN projeto-docker@1.0.0 No repository field.
npm WARN projeto-docker@1.0.0 No license field.

added 134 packages from 109 contributors and audited 235 packages in 14.669s
found 0 vulnerabilities

Removing intermediate container 561c5f7e74ed
 ---> 68a9c6c21383
Step 7/8 : ENTRYPOINT ["npm", "start"]
 ---> Running in 962910ea7517
Removing intermediate container 962910ea7517
 ---> 7ddd12933670
Step 8/8 : EXPOSE 3000
 ---> Running in 2df958a7ec96
Removing intermediate container 2df958a7ec96
 ---> 2cc86de69f55
Successfully built 2cc86de69f55
Successfully tagged merciof/ecommerce-book:latest
ubuntu uses an image, skipping

************************************************************************************

docker-compose build
mongodb uses an image, skipping
Building node
Step 1/8 : FROM node:latest
latest: Pulling from library/node
ab1fc7e4bf91: Pull complete
35fba333ff52: Pull complete
f0cb1fa13079: Pull complete
3d1dd648b5ad: Pull complete
49f7a0920ce1: Pull complete
1d199f738c5f: Pull complete
4be79c9aac1d: Pull complete
38138903a70b: Pull complete
Digest: sha256:36aeeb280a85cff4e888c49bf0dfdb86084e6158ae8fd106e397542edaa08d0a
Status: Downloaded newer image for node:latest
 ---> 8c67bfd7b95b
Step 2/8 : MAINTAINER merciof
 ---> Running in 919823b12bb4
Removing intermediate container 919823b12bb4
 ---> 2594b0ecab04
Step 3/8 : ENV NODE_ENV=development
 ---> Running in 0e0f6bc87a20
Removing intermediate container 0e0f6bc87a20
 ---> a6161181e491
Step 4/8 : COPY . /var/www
 ---> 44cc2065b295
Step 5/8 : WORKDIR /var/www
 ---> Running in 9f415c399fe9
Removing intermediate container 9f415c399fe9
 ---> 4545996f84ab
Step 6/8 : RUN npm install
 ---> Running in 561c5f7e74ed
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN projeto-docker@1.0.0 No repository field.
npm WARN projeto-docker@1.0.0 No license field.

added 134 packages from 109 contributors and audited 235 packages in 14.669s
found 0 vulnerabilities

Removing intermediate container 561c5f7e74ed
 ---> 68a9c6c21383
Step 7/8 : ENTRYPOINT ["npm", "start"]
 ---> Running in 962910ea7517
Removing intermediate container 962910ea7517
 ---> 7ddd12933670
Step 8/8 : EXPOSE 3000
 ---> Running in 2df958a7ec96
Removing intermediate container 2df958a7ec96
 ---> 2cc86de69f55
Successfully built 2cc86de69f55
Successfully tagged merciof/ecommerce-book:latest
ubuntu uses an image, skipping

***********************************************

docker-compose up -d
Creating network "docker-books_minha-rede" with driver "bridge"
Pulling mongodb (mongo:)...
latest: Pulling from library/mongo
7b722c1070cd: Pull complete
5fbf74db61f1: Pull complete
ed41cb72e5c9: Pull complete
7ea47a67709e: Pull complete
778aebe6fb26: Pull complete
3b4b1e0b80ed: Pull complete
844ccc42fe76: Pull complete
eab01fe8ebf8: Pull complete
e5758d5381b1: Pull complete
a795f1f35522: Pull complete
67bc6388d1cd: Pull complete
89b55f4f3473: Pull complete
10886b20b4fc: Pull complete
Digest: sha256:a7c1784c83536a3c686ec6f0a1c570ad5756b94a1183af88c07df82c5b64663c
Status: Downloaded newer image for mongo:latest
Creating mongodb ... done
Creating rural-books ... done



A imagem do projeto foi subida no ducker hub: https://cloud.docker.com/u/merciof/repository/docker/merciof/ecommerce-livro

Para executar o projeto, sem o docker-compose, podem ser feitos os seguintes comandos:

Criaçao da rede local 'minha-rede' para a comunicação entre o containeres:

```bash
sudo docker network create --driver bridge minha-rede 
```

Sobe um container do mongodb para servir a base de dados: 

```bash
docker run -d --name mongodb --network minha-rede mongo
```

Em seguida, o segundo container pode ser subido com o comando: 

```bash
# a flag -d faz o container rodar separadamente do terminal 
# -p associa a porta 80 do host a porta 3000 exposta no container
docker run -d -p 80:3000 --network minha-rede merciof/ecommerce-livro 
```
Através da rota http://localhost:80/seed dados são inseridos na base de dados. 

O projeto está on na aws no seguinte host: ec2-3-90-146-4.compute-1.amazonaws.com
