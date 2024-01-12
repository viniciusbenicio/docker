# Docker - Documentação

## Containers

Os containers no Docker operam como processos isolados dentro de um sistema operacional, garantindo a execução independente por meio do uso de namespaces para isolamento adequado.

1. **PID (Process ID):** Proporciona isolamento dos processos em execução dentro do container.
   
2. **NET (Network):** Oferece isolamento das interfaces de rede.
   
3. **IPC (Inter-Process Communication):** Assegura isolamento da comunicação entre processos e da memória compartilhada.
   
4. **MNT (Mount):** Fornece isolamento do sistema de arquivos e dos pontos de montagem.
   
5. **UTS (Unix Timesharing System):** Assegura isolamento do kernel, agindo como se o container fosse outro host.

## Instalação

Para instalar o Docker, siga as instruções disponíveis em [https://docs.docker.com/desktop/windows/install](https://docs.docker.com/desktop/windows/install).

## Docker Hub

O Docker Hub, acessível em [https://hub.docker.com/](https://hub.docker.com/), é um repositório centralizado de imagens Docker.

## Comandos

### Baixando uma imagem

```docker
docker pull ubuntu
```

### Exibindo containers em execução

```docker
docker ps 
docker container ls
```

### Verificando containers não mais em execução

```docker
docker ps -a 
docker container ls -a
```

Exemplo de saída:

```docker
CONTAINER ID   IMAGE                            COMMAND                  CREATED         STATUS                     PORTS     NAMES
7664337e71f7   ubuntu                           "/bin/bash"              3 seconds ago   Exited (0) 2 seconds ago             adoring_nobel
f2ff4e50e4cf   mcr.microsoft.com/mssql/server   "/opt/mssql/bin/perm…"   4 months ago    Exited (0) 12 days ago               sqlserver
```

### Executando um container em modo de execução contínua

```docker
docker run ubuntu sleep 1d
docker ps 
```

Exemplo de saída:

```docker
CONTAINER ID   IMAGE     COMMAND      CREATED          STATUS          PORTS     NAMES
6bd1b73d1d9c   ubuntu    "sleep 1d"   25 seconds ago   Up 24 seconds             affectionate_leavitt
```

### Parando e reiniciando um container

Para parar:

```docker
docker stop 6bd1b73d1d9c
```

Para reiniciar:

```docker
docker start 6bd1b73d1d9c
```

### Executando comandos em modo interativo

Acessando o terminal do container:

```docker
docker exec -it 6bd1b73d1d9c bash
```

### Pausando e retomando um container

Para pausar:

```docker
docker pause 6bd1b73d1d9c 
```

Para retomar:

```docker
docker unpause 6bd1b73d1d9c 
```

### Definindo o tempo de parada do container

```docker
docker stop -t=0 6bd1b73d1d9c
```

### Removendo um container

```docker
docker rm 6bd1b73d1d9c 
```

Esses comandos básicos ajudarão você a começar com o Docker, simplificando a administração e a manipulação de containers de maneira eficiente. Explore mais funcionalidades e possibilidades conforme necessário para o seu ambiente de desenvolvimento e implementação.

## Iniciando o Docker em Modo Interativo

Para iniciar o Docker em modo interativo, utilize o seguinte comando:

```docker
docker run -it ubuntu bash
```

Este comando cria e executa um container interativo baseado na imagem Ubuntu, permitindo o acesso ao terminal do container.

## Mapeando Portas

Ao executar um container, é possível mapear portas entre o host e o container usando as opções `-p` ou `-P`. A opção `-p` permite especificar manualmente a porta no host e a porta no container.

### Exemplo de Criação com Mapeamento de Porta Automático:

```docker
# Criando com mapeamento de porta automático
docker run -d -P dockersamples/static-site

docker ps
```

Resultado:

```
CONTAINER ID   IMAGE                      COMMAND                  CREATED         STATUS         PORTS                                               NAMES
9a4a24aa8d10   dockersamples/static-site "/bin/sh -c 'cd /usr…"   58 seconds ago  Up 57 seconds  0.0.0.0:32769->80/tcp, 0.0.0.0:32768->443/tcp   serene_hoover
```

Para verificar o mapeamento de porta:

```docker
# Mostrando o mapeamento de porta 
docker port 9a4a24aa8d10
```

Resultado:

```
80/tcp -> 0.0.0.0:32769
443/tcp -> 0.0.0.0:32768
```

![Port Mapping](https://github.com/viniciusbenicio/docker/assets/63131764/36865ef9-b87f-4d7c-9047-9897d02c0f11)

### Exemplo de Criação com Mapeamento de Porta Manual:

```docker
# Mapeando manualmente a porta 8080 do host para a porta 80 do container
docker run -d -p 8080:80 dockersamples/static-site
```

![Port Mapping Manual](https://github.com/viniciusbenicio/docker/assets/63131764/cc24d22a-02f6-43c8-9957-4fd134ce51ac)

## Imagens

Uma imagem Docker é um conjunto de camadas que, quando combinadas, formam uma imagem. Cada imagem possui um identificador único.

```docker
# Verificando as imagens baixadas
docker images

REPOSITORY                       TAG       IMAGE ID       CREATED        SIZE
mcr.microsoft.com/mssql/server   latest    683d523cd395   5 months ago   2.9GB

# Utilizando inspect para inspecionar informações detalhadas da imagem
docker inspect 683d523cd395

# Utilizando docker history para verificar as camadas que compõem a imagem Docker
docker history 683d523cd395
```

## Criando uma Imagem

A criação de uma imagem Docker envolve a definição de um Dockerfile, que contém as instruções para construir a imagem.

```docker
# Exemplo de Dockerfile para uma aplicação Node.js
FROM node:14
WORKDIR /app-node
COPY . .
RUN npm install
ENTRYPOINT npm start
```

Para construir a imagem:

```docker
# Construindo a imagem com a tag viniciusbenicio/app-node:1.0
docker build -t viniciusbenicio/app-node:1.0 .
```

Verificando a criação da imagem:

```docker
# Verificando as imagens
docker images

REPOSITORY                     TAG       IMAGE ID       CREATED              SIZE
viniciusbenicio/app-node       1.0       7ea93548f0bf   About a minute ago   914MB
```

Rodando a imagem:

```docker
# Rodando a imagem na porta 8080 do host
docker run -d -p 8080:3000 viniciusbenicio/app-node:1.0
```

![Running Image](https://github.com/viniciusbenicio/docker/assets/63131764/416d5b93-3703-4197-af54-95cbbb065ee9)

## Incrementando a Imagem

Adicionando mais detalhes à imagem, como exposição de uma porta específica.

```docker
# Modificando o Dockerfile para expor a porta 3000
FROM node:14
WORKDIR /app-node
EXPOSE 3000
COPY . .
RUN npm install
ENTRYPOINT npm start
```

Construindo e versionando a nova imagem:

```docker
# Construindo a imagem com a tag viniciusbenicio/app-node:1.1
docker build -t viniciusbenicio/app-node:1.1 .

# Definindo variáveis de ambiente para personalizar o build
FROM node:14
WORKDIR /app-node
ARG PORT_BUILD=6000
ENV PORT=$PORT_BUILD
EXPOSE $PORT_BUILD
RUN npm install
ENTRYPOINT npm start
```

## Subindo Imagem para Docker Hub

```docker
# Realizando login no Docker Hub
docker login -u seuLogin

# Subindo a imagem para o Docker Hub
docker push viniciusbenicio/app-node:1.0
```

Acesse [Docker Hub - ViniciusBenicio/App-Node](https://hub.docker.com/repository/docker/viniciusbenicio/app-node/general) para visualizar a imagem publicada.

Este guia proporciona uma visão abrangente, cobrindo desde a execução de containers até a criação, versionamento e upload de imagens no Docker Hub. Utilize estas informações conforme necessário para suas necessidades de desenvolvimento e implementação.
## Bind Mounts

Bind mounts são um mecanismo no Docker para compartilhar arquivos do host com o contêiner.

### Exemplo de Uso:

```docker
# Utilizando um bind mount para compartilhar um diretório do host com o contêiner
docker run -it -v D:\Projetos\Pessoal\docker\docker\volume:/app ubuntu bash

# Criando um arquivo dentro do contêiner
root@81f003098c72:/app# touch arquivo.txt
```

### Estrutura do diretório após o uso do Bind Mount:

![image](https://github.com/viniciusbenicio/docker/assets/63131764/49067a0b-21a2-4d28-b405-897af0eca474)

### Bind Mounts (+Semântico)

Outra maneira de criar um contêiner mapeando um diretório do host:

```docker
docker run -it --mount type=bind,source=D:\Projetos\Pessoal\docker\docker\volume,target=/app ubuntu bash
```

## Utilizando Volumes

O uso de volumes é a maneira recomendada pelo Docker para o compartilhamento de dados entre contêineres e persistência de dados.

### Verificando e Criando Volumes:

```docker
# Verificando os volumes existentes
docker volume ls

# Criando um volume
docker volume create meu-volume
```

### Exemplo de Uso de Volumes:

```docker
# Criando um contêiner e mapeando um volume recém-criado
docker run -it -v meu-volume/app ubuntu bash

# Criando um arquivo dentro do volume
root@a2503ed65504:/app# touch arquivo.txt
```

### Verificando a Localização do Arquivo no Volume:

```bash
# Localizando os volumes no host
cd \\wsl$\docker-desktop-data\data\docker\volumes

# Verificando o conteúdo do volume
ls \\wsl$\docker-desktop-data\data\docker\volumes\meu-volume\_data
```

Dessa forma, o Docker cria e gerencia volumes, garantindo a persistência dos dados mesmo após a remoção do contêiner. Recomenda-se o uso de volumes para cenários que exigem persistência e compartilhamento de dados entre contêineres. Para mais informações sobre volumes, consulte a [documentação oficial do Docker sobre Volumes](https://docs.docker.com/storage/volumes/).
## Utilizando Volumes - Outra Maneira de Criar um Volume

Uma abordagem alternativa para criar e usar volumes no Docker envolve o uso do parâmetro `--mount` para associar um volume a um contêiner.

```docker
# Criando um contêiner e montando um volume chamado "meu-volume" no diretório "/app" do contêiner
docker run -it --mount source=meu-volume,target=/app ubuntu bash

# Listando os volumes existentes
docker volume ls
DRIVER    VOLUME NAME
local     5f58af0a26c53a1ba90722ca71d8c3a43e3c05199358c7abbd0279b41cb16d41
local     meu-volume
```

## Conhecendo a Rede Bridge

O Docker é fornecido com três redes padrão que oferecem configurações específicas para o gerenciamento do tráfego de dados. Para visualizar essas redes, utilize o comando:

```docker
docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
1fbe9463993a   bridge    bridge    local
e319b96ca931   host      host      local
4af63613e38e   none      null      local
```

Cada contêiner iniciado no Docker é associado automaticamente à rede padrão, a menos que uma rede diferente seja especificada explicitamente. A rede padrão confere ao contêiner uma interface que faz bridge com a interface docker0 do host Docker. Essa interface recebe automaticamente o próximo endereço disponível na rede IP 172.17.0.0/16.

Todos os contêineres nesta rede podem se comunicar via protocolo TCP/IP. No entanto, descobrir o IP do contêiner de destino pode ser desafiador devido à atribuição automática de IPs. Para facilitar essa identificação, o Docker disponibiliza a opção "--link" durante a inicialização do contêiner.

```docker
# Obtendo informações detalhadas sobre um contêiner específico (exemplo usando um ID de contêiner)
docker inspect 34305d7460d0
[...]
```

Este comando fornece informações detalhadas sobre o contêiner, incluindo o IP atribuído, a rede associada, e outras configurações relacionadas à rede.

Ao explorar a documentação do Docker, recomendamos a leitura atenta da [documentação oficial sobre redes](https://docs.docker.com/network/) para uma compreensão mais aprofundada das opções de configuração e gerenciamento de redes no Docker.

## Criando uma rede bridge

### Comunicando os dois Containers via Hostname ubuntu01 - ubuntu02

Para criar uma rede bridge no Docker e comunicar dois containers através do hostname, siga os passos abaixo:

1. Crie a rede bridge:

   ```docker
   docker network create --driver bridge minha-bridge
   ```

2. Execute o primeiro container (ubuntu01) na rede bridge:

   ```docker
   docker run -it --name ubuntu01 --network minha-bridge ubuntu bash
   ```

3. No container ubuntu01, atualize o sistema e instale o pacote necessário para realizar o ping:

   ```bash
   apt-get update
   apt-get install iputils-ping -y
   ```

4. Execute o segundo container (ubuntu02) na mesma rede bridge:

   ```docker
   docker run -d --name ubuntu02 --network minha-bridge ubuntu sleep 1d
   ```

5. No container ubuntu01, teste a comunicação com ubuntu02 via ping:

   ```bash
   ping ubuntu02
   ```

   Você deverá ver uma saída indicando que a comunicação foi bem-sucedida.

### As redes none e host

#### Rede None:

Para criar um container sem configuração de rede (rede none), use o seguinte comando:

```docker
docker run -d --network none ubuntu sleep 1d
```

O comando acima cria um container baseado na imagem Ubuntu sem configurar nenhuma rede. O container terá apenas o endereço de loopback e não terá acesso à rede externa nem a outros containers.

#### Rede Host:

Para criar um container que compartilhe a pilha de rede do host, utilize o seguinte comando:

```docker
docker run -d --network host viniciusbenicio/app-node:1.0
```

Esse comando cria um container baseado na imagem "viniciusbenicio/app-node:1.0" e utiliza a pilha de rede do host. Todas as interfaces do host estarão disponíveis para o container, e o nome do host do container corresponderá ao nome do host no sistema host.

## Comunicando aplicação e banco

Para exemplificar a comunicação entre uma aplicação e um banco de dados, utilize os seguintes passos:

1. Baixe as imagens necessárias (MongoDB e Alura Books):

   ```docker
   docker pull mongo:4.4.6
   docker pull aluradocker/alura-books:1.0
   ```

2. Inicie o banco de dados MongoDB na rede bridge criada:

   ```docker
   docker run -d --network minha-bridge --name meu-mongo mongo:4.4.6
   ```

3. Inicie a aplicação Alura Books na mesma rede bridge e mapeie a porta 3000:

   ```docker
   docker run -d --network minha-bridge --name alurabooks -p 3000:3000 aluradocker/alura-books:1.0
   ```

## Conhecendo Docker Compose

Para simplificar o gerenciamento de vários containers, utilize o Docker Compose. Crie um arquivo `docker-compose.yml` com as seguintes informações:

```yaml
version: "3.9"
services: 
  mongodb:
    image: mongo:4.4.6
    container_name: meu-mongo
    networks:
      - compose-bridge

  alurabooks:
    image: aluradocker/alura-books:1.0
    container_name: alurabooks
    networks:
      - compose-bridge
    ports:
      - 3000:3000
    depends_on:
      - mongodb

networks:
  compose-bridge:
    driver: bridge
```

Navegue até o diretório onde está o arquivo `docker-compose.yml` e execute:

```docker
docker-compose up
```

Dessa forma, o Docker Compose criará e gerenciará os containers definidos no arquivo, simplificando a orquestração de aplicativos Docker complexos.
