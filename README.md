# docker
Projeto do curso de Docker

Porta do servidor web no play with docker: 32768

Este Dockerfile constrói um container baseado no node que irá servir os arquivos estáticos (HTML,CSS) e também realizará a comunicação com o container mongo (base de dados).

```yml
FROM node:latest # constrói a partir da última versãp do node disponibilizada no dockerhub, caso ela não esteja presente localmente 
MAINTAINER merciof 
ENV NODE_ENV=development
COPY . /var/www # copia os arquivos para o diretório /var/www do container a ser construído
WORKDIR /var/www # define o diretório de trabalho como /var/www
RUN npm install # executa o comando npm install e instala as dependências definidas no arquivo package.json
ENTRYPOINT ["npm", "start"] # comando executado ao container ser levantado, ele executa o arquivo server.js 
EXPOSE 3000 # expõe a porta 3000
```
