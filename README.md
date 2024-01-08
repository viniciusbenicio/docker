# Containers

1. Container funciona como processos dentro de um sistema operacional, e cada um deles consegue se isolar para que seja executado independente, eles contém namespaces.
    
    1.1 PID - Provê isolamento doss processo rodando dentro do container.
    
    1.2 NET - Provê isolamento das interfaces de rede.
    
    1.3 IPC - Provê isolamento da comunicação entre processos e memória compartilhada.
    
    1.4 MNT - Provê isolamentos do sistema de arquivos / pontos de montagem.
    
    1.5 UTS - Provê isolamento do kernel. Age como se container fosse outro host.
    

# INSTALAÇÃO

1. Acesse URL : https://docs.docker.com/desktop/windows/install para instalação. 

# Docker Hub

1. Docker Hub acessando a URL : https://hub.docker.com/ é um repositorio de imagem do docker.

# Comandos

1. Alguns comandos
    1. Baixando uma imagem.
    
    ```docker
    docker pull ubuntu
    ```
    
    2. Exibe quais containers estão rodando no momento
    
    ```docker
    docker ps 
    docker container ls
    ```
    
    3. Como verificar os container que não estão mais rodando no momento
    
    ```docker
    docker ps -a 
    docker container ls -a
    
    CONTAINER ID   IMAGE                            COMMAND                  CREATED         STATUS                     PORTS     NAMES
    7664337e71f7   ubuntu                           "/bin/bash"              3 seconds ago   Exited (0) 2 seconds ago             adoring_nobel
    f2ff4e50e4cf   mcr.microsoft.com/mssql/server   "/opt/mssql/bin/perm…"   4 months ago    Exited (0) 12 days ago               sqlserver
    ```
    
    4. Para o Docker ficar em execução necessário rodar o docker run com um processo para que não execute, e desligue.
    
    ```docker
    docker run ubuntu sleep 1d
    
    docker ps 
    CONTAINER ID   IMAGE     COMMAND      CREATED          STATUS          PORTS     NAMES
    6bd1b73d1d9c   ubuntu    "sleep 1d"   25 seconds ago   Up 24 seconds             affectionate_leavitt
    ```
