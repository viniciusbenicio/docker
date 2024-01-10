# Containers

Os containers funcionam como processos dentro de um sistema operacional, cada um capaz de se isolar para execução independente, utilizando namespaces para garantir o isolamento adequado.

1. **PID (Process ID):** Provê isolamento dos processos em execução dentro do container.
   
2. **NET (Network):** Provê isolamento das interfaces de rede.
   
3. **IPC (Inter-Process Communication):** Provê isolamento da comunicação entre processos e da memória compartilhada.
   
4. **MNT (Mount):** Provê isolamento do sistema de arquivos e dos pontos de montagem.
   
5. **UTS (Unix Timesharing System):** Provê isolamento do kernel, agindo como se o container fosse outro host.

# Instalação

1. Acesse a URL: [https://docs.docker.com/desktop/windows/install](https://docs.docker.com/desktop/windows/install) para obter as instruções de instalação.

# Docker Hub

1. O Docker Hub, acessado pela URL: [https://hub.docker.com/](https://hub.docker.com/), é um repositório de imagens do Docker.

# Comandos

## Baixando uma imagem

```docker
docker pull ubuntu
```

## Exibindo containers em execução

```docker
docker ps 
docker container ls
```

## Verificando containers que não estão mais em execução

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

## Executando um container em modo de execução contínua

```docker
docker run ubuntu sleep 1d
docker ps 
```

Exemplo de saída:

```docker
CONTAINER ID   IMAGE     COMMAND      CREATED          STATUS          PORTS     NAMES
6bd1b73d1d9c   ubuntu    "sleep 1d"   25 seconds ago   Up 24 seconds             affectionate_leavitt
```

## Parando e reiniciando um container

Para parar:

```docker
docker stop 6bd1b73d1d9c
```

Para reiniciar:

```docker
docker start 6bd1b73d1d9c
```

## Executando comandos em modo interativo

Acessando o terminal do container:

```docker
docker exec -it 6bd1b73d1d9c bash
```

## Pausando e retomando um container

Para pausar:

```docker
docker pause 6bd1b73d1d9c 
```

Para retomar:

```docker
docker unpause 6bd1b73d1d9c 
```

## Definindo o tempo de parada do container

```docker
docker stop -t=0 6bd1b73d1d9c
```

## Removendo um container

```docker
docker rm 6bd1b73d1d9c 
```

## Iniciando o Docker em modo interativo

```docker
docker run -it ubuntu bash
```
## Mapeando Portas

Para mapear portas ao executar um container, você pode utilizar a opção `-p` ou `-P`. A opção `-p` permite mapear uma porta específica no host para uma porta específica no container.

### Exemplo de criação com mapeamento de porta automático:

```docker
# Criando com mapeamento da porta
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


### Exemplo de criação com mapeamento de porta manual:

```docker
# O parâmetro p deve ser minúsculo, -p seguido pela porta desejada (8080 no exemplo)
docker run -d -p 8080:80 dockersamples/static-site
```

![Port Mapping Manual](https://github.com/viniciusbenicio/docker/assets/63131764/cc24d22a-02f6-43c8-9957-4fd134ce51ac)

## Imagens

Uma imagem é nada mais que um conjuto de camadas quando juntamos formamos uma imagem, cada imagem tem seu identificador.

```docker
# Verificando as imagem baixadas

docker images

REPOSITORY                       TAG       IMAGE ID       CREATED        SIZE
mcr.microsoft.com/mssql/server   latest    683d523cd395   5 months ago   2.9GB

# Pode utilizar inspect para especionar informações detalhada da imagem passando o id da imagem
# depois do Inspect

docker inspect 683d523cd395

# Podemos utilizar docker history para verificar as camada que formam a imagem do docker

docker history 683d523cd395
IMAGE          CREATED        CREATED BY                                      SIZE      COMMENT
683d523cd395   5 months ago   /bin/sh -c #(nop)  CMD ["/opt/mssql/bin/sqls…   0B
<missing>      5 months ago   /bin/sh -c #(nop)  ENTRYPOINT ["/opt/mssql/b…   0B
<missing>      5 months ago   /bin/sh -c #(nop)  USER mssql                   0B
<missing>      5 months ago   /bin/sh -c EXTRA_APT_PACKAGES="" /tmp/instal…   178MB
<missing>      5 months ago   /bin/sh -c tar -h -xf install.tar               1.33GB
<missing>      5 months ago   /bin/sh -c #(nop) COPY file:1fb15b06393b36bc…   1.33GB
<missing>      5 months ago   /bin/sh -c #(nop)  ENV CONFIG_EDGE_BUILD=       0B
<missing>      5 months ago   /bin/sh -c #(nop)  ENV MSSQL_RPC_PORT=135       0B
<missing>      5 months ago   /bin/sh -c #(nop)  EXPOSE 1433                  0B
<missing>      5 months ago   /bin/sh -c #(nop)  LABEL vendor=Microsoft co…   0B
<missing>      6 months ago   /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B
<missing>      6 months ago   /bin/sh -c #(nop) ADD file:12f97b7b044d0d116…   72.8MB
<missing>      6 months ago   /bin/sh -c #(nop)  LABEL org.opencontainers.…   0B
<missing>      6 months ago   /bin/sh -c #(nop)  LABEL org.opencontainers.…   0B
<missing>      6 months ago   /bin/sh -c #(nop)  ARG LAUNCHPAD_BUILD_ARCH     0B
<missing>      6 months ago   /bin/sh -c #(nop)  ARG RELEASE                  0B

```
## Criando uma imagem

Seguir um processo conforme imagem 

![Untitled](https://github.com/viniciusbenicio/docker/assets/63131764/280385fc-97a7-41c6-88b5-2598054f6182)


```docker
# Definindo a versão do NODE para 14, Informando diretorio padrão /app-node e rodando o npm install e em seguida npm start após que container for criado

FROM node:14
WORKDIR /app-node
COPY . .
RUN npm install
ENTRYPOINT npm start

```

[[Dockerfile](Docker%201d07e06972bc49c5b90858783dca76cc/Dockerfile.txt)](https://github.com/viniciusbenicio/docker/blob/main/app-exemplo/Dockerfile)

```docker
# Parti do Dockerfile acima podemos gerar nossa imagem

docker build -t viniciusbenicio/app-node:1.0 .

# Verificando a criação da imagem

docker images
REPOSITORY                       TAG       IMAGE ID       CREATED              SIZE
viniciusbenicio/app-node         1.0       7ea93548f0bf   About a minute ago   914MB

# Rodando a imagem 
docker run -d -p 8080:3000 viniciusbenicio/app-node:1.0

3477fd1f8c1e7180fac8609759fb6fe4e1c4ed14d4b1a4df0842dca823cfb400
```

![Untitled 1](https://github.com/viniciusbenicio/docker/assets/63131764/416d5b93-3703-4197-af54-95cbbb065ee9)

## Incrementando a imagem

```docker
# Deixando explicito a porta que aplicação está ou deve rodar

FROM node:14
WORKDIR /app-node
EXPOSE 3000
COPY . .
RUN npm install
ENTRYPOINT npm start

# Alteramos o Dockerfile agora devemos gerar uma nova build da imagem 

docker build -t viniciusbenicio/app-node:1.1 .

# Definindo variaveis de ambiente para definir porta para construir a imagem e env para o 
# container, assim sendo possível definir portas no build da imagem

FROM node:14
WORKDIR /app-node
ARG PORT_BUILD=6000
ENV PORT=$PORT_BUILD
EXPOSE $PORT_BUILD
RUN npm install
ENTRYPOINT npm start
```
## Subindo imagem para DockerHUB

### 

```docker
# Realizar login no CLI do docker

docker login -u seulogin
# em seguida irá solicitar sua senha após digitar seu login do Docker HUB
Password:

# Após autenticar irá ver a seguinte mensagem
#Login Succeeded
#Logging in with your password grants your terminal complete access to your account.
#For better security, log in with a limited-privilege personal access token. Learn more at https://docs.docker.com/go/access-tokens/

# Para subir a imagem digite os comando seguintes

# para verificar suas imagens
docker images

REPOSITORY                       TAG       IMAGE ID       CREATED        SIZE
viniciusbenicio/app-node         1.2       94eb56b99b56   22 hours ago   912MB
viniciusbenicio/app-node         1.1       0c3703f3b4fc   22 hours ago   912MB
viniciusbenicio/app-node         1.0       7ea93548f0bf   23 hours ago   914MB

# Com a imagem que deseja subir no Docker HUB faça o seguinte

docker push viniciusbenicio/app-node:1.0

The push refers to repository [docker.io/viniciusbenicio/app-node]
95effa81783b: Pushed
ad500a08f940: Pushed
80b7027d7837: Pushed
0d5f5a015e5d: Mounted from library/node
3c777d951de2: Mounted from library/node
f8a91dd5fc84: Mounted from library/node
cb81227abde5: Mounted from library/node
e01a454893a9: Mounted from library/node
c45660adde37: Mounted from library/node
fe0fb3ab4a0f: Mounted from library/node
f1186e5061f2: Mounted from library/node
b2dba7477754: Mounted from library/node
1.0: digest: sha256:c0dc6db3ab8fed30eb7d1eb9c9706bd23bbf6527726bb69671e0b0f19a6cfa8d size: 2840
```

Link da imagem no DockerHUB https://hub.docker.com/repository/docker/viniciusbenicio/app-node/general

## Bind mounts

Bind mounts são um **mecanismo de compartilhamento de arquivos do host com o contêiner**.

```docker
# Utilizando um volume do meu host para o docker

docker run -it -v D:\Projetos\Pessoal\docker\docker\volume:/app ubuntu bash

# Criado o repositorio app
root@81f003098c72:/# ls
app  bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

root@81f003098c72:/# cd app

# Criando um arquivo 
root@81f003098c72:/app# touch arquivo.txt

root@81f003098c72:/app# ls
arquivo.txt
```

![image](https://github.com/viniciusbenicio/docker/assets/63131764/49067a0b-21a2-4d28-b405-897af0eca474)

## Bind mounts (+Semântico) 

Outra maneira também de criar o Container mapeando um host

```docker
docker run -it --mount type=bind,source=D:\Projetos\Pessoal\docker\docker\volume,target=/app ubuntu bash

root@dc22456979e5:/# ls
app  bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@dc22456979e5:/# cd app
root@dc22456979e5:/app# ls
arquivo.txt
```

## Utilizando Volumes

Essa é a maneira mais recomendada de utilizar, segundo a documentação do Docker  

[Volumes](https://docs.docker.com/storage/volumes/)

```docker
# Verificando os volumes

docker volume ls
DRIVER    VOLUME NAME

# Criando um volume

docker volume create meu-volume

# Volume criado
meu-volume

# Criando um Container e ja mapeando nosso volume criado

docker run -it -v meu-volume/app ubuntu bash

root@a2503ed65504:/# ls
app  bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

root@a2503ed65504:/# cd app

# Criando um arquivo no volume

root@a2503ed65504:/app# touch arquivo.txt

# Verificando o arquivo

root@a2503ed65504:/app# ls
arquivo.txt

# Mas aonde está o arquivo?

cd \\wsl$\docker-desktop-data\data\docker\volumes

ls

Directory: \\wsl$\docker-desktop-data\data\docker\volumes

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----          1/9/2024   9:40 PM                5f58af0a26c53a1ba90722ca71d8c3a43e3c05199358c7abbd0279b41cb16d41
d-----          1/9/2024   9:38 PM                meu-volume
------          1/9/2024   9:40 PM          32768 metadata.db
-----l          1/9/2024   8:39 PM              0 backingFsBlockDev

Directory: \\wsl$\docker-desktop-data\data\docker\volumes\meu-volume\_data

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
------          1/9/2024   9:41 PM              0 arquivo.txt
```

## Utilizando Volumes - Outra maneira de criar um volume

```docker
docker run -it --mount source=meu-volume,target=/app ubuntu bash

docker volume ls
DRIVER    VOLUME NAME
local     5f58af0a26c53a1ba90722ca71d8c3a43e3c05199358c7abbd0279b41cb16d41
local     meu-volume
```

