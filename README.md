# docker

O projeto é formado por dois containers, um deles é baseado na imagem do node, e serve arquivos estáticos utilizando o framework expressjs, além de realizar a conexão com a base de dados. O segundo é a imagem no mongodb, que é usado para persistência de dados. 


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

Este é o docker-compose.yml

```yml
version: '2' # especifica a versão do Compose
services: # descrição dos serviços a serem construídos
  mongodb:
    image: mongo # nome da imagem a ser utilizada, caso não esteja contida localmente, será baixada a última versão do docker hub
    networks: # especificação das redes locais as quais este container (serviço) estará conectado
      - minha-rede 
  node:
    build: # contrução de imagem a partir do Dockerfile
      dockerfile: ./Dockerfile
      context: . # caminho do diretório que contém o Dockerfile
    image: merciof/ecommerce-livro # nome da imagem a ser construída
    container_name: rural-books # nome do container a ser subido
    ports:
      - "80:3000" # a porta 80 do servidor web estará associada a porta 3000 exposta no container
    networks: # especificação das redes as quais o serviço fará parte
      - minha-rede
    depends_on: # definição de dependência quanto ao serviço mongob
      - mongodb
networks: # definição da rede a ser criada e do driver da mesma
  minha-rede:
    driver: bridge 
```


A imagem do projeto foi subida no ducker hub: https://cloud.docker.com/u/merciof/repository/docker/merciof/ecommerce-livro

Para executar o projeto, alternativamente ao docker-compose, podem ser feitos os seguintes comandos:

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
Através da rota http://localhost:80/seed os dados são inseridos na base de dados. 

O projeto está on na aws no seguinte host: ec2-3-90-146-4.compute-1.amazonaws.com
