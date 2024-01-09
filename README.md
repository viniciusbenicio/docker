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

![Untitled](Docker%201d07e06972bc49c5b90858783dca76cc/Untitled.png)

```docker
# Definindo a versão do NODE para 14, Informando diretorio padrão /app-node e rodando o npm install e em seguida npm start após que container for criado

FROM node:14
WORKDIR /app-node
COPY . .
RUN npm install
ENTRYPOINT npm start

```

[Dockerfile](Docker%201d07e06972bc49c5b90858783dca76cc/Dockerfile.txt)

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

![Untitled](Docker%201d07e06972bc49c5b90858783dca76cc/Untitled%201.png)