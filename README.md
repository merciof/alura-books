# docker
Projeto do curso de Docker

Porta do servidor web no play with docker: 32768

Este Dockerfile constrói um container baseado no node que irá servir os arquivos estáticos (HTML,CSS) e também realizará a comunicação com o container mongo (base de dados).

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
version: '3'
services:
  mongodb:
    image: mongo
    networks:
      - minha-rede
  node:
    build:
      dockerfile: ./Dockerfile    
      context: .
    image: merciof/ecommerce-livro
    container_name: rural-books
    ports:
      - 3000
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
