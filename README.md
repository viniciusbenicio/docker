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
