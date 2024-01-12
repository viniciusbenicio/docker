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

## Conhecendo a rede Bridge

O docker é disponibilizado com três redes por padrão. Essas redes oferecem configurações específicas para gerenciamento do tráfego de dados. Para visualizar essas interfaces, basta utilizar o comando abaixo:

```docker
docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
1fbe9463993a   bridge    bridge    local
e319b96ca931   host      host      local
4af63613e38e   none      null      local
```

Cada container iniciado no docker é associado a uma rede específica. Essa é a rede padrão para qualquer container, a menos que associemos, explicitamente, outra rede a ele. A rede confere ao container uma interface que faz bridge com a interface docker0 do docker host. Essa interface recebe, automaticamente, o próximo endereço disponível na rede IP 172.17.0.0/16.

Todos os containers que estão nessa rede poderão se comunicar via protocolo TCP/IP. Se você souber qual endereço IP do container deseja conectar, é possível enviar tráfego para ele. Afinal, estão todos na mesma rede IP (172.17.0.0/16).

Um detalhe a se observar: como os IPs são cedidos automaticamente, não é tarefa trivial descobrir qual IP do container de destino. Para ajudar nessa localização, o docker disponibiliza, na inicialização de um container, a opção “–link“.

```docker
docker inspect 34305d7460d0
[
    {
        "Id": "34305d7460d0914a2f30af613553766eface15d21339d6547bd16e2f8b3904fe",
        "Created": "2024-01-11T23:08:19.620643429Z",
        "Path": "bash",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 1791,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2024-01-11T23:08:19.899928767Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:174c8c134b2a94b5bb0b37d9a2b6ba0663d82d23ebf62bd51f74a2fd457333da",
        "ResolvConfPath": "/var/lib/docker/containers/34305d7460d0914a2f30af613553766eface15d21339d6547bd16e2f8b3904fe/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/34305d7460d0914a2f30af613553766eface15d21339d6547bd16e2f8b3904fe/hostname",
        "HostsPath": "/var/lib/docker/containers/34305d7460d0914a2f30af613553766eface15d21339d6547bd16e2f8b3904fe/hosts",
        "LogPath": "/var/lib/docker/containers/34305d7460d0914a2f30af613553766eface15d21339d6547bd16e2f8b3904fe/34305d7460d0914a2f30af613553766eface15d21339d6547bd16e2f8b3904fe-json.log",
        "Name": "/blissful_meitner",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "ConsoleSize": [
                30,
                120
            ],
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": [],
            "BlkioDeviceWriteBps": [],
            "BlkioDeviceReadIOps": [],
            "BlkioDeviceWriteIOps": [],
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/50aea45836a7471bc685d382b3ef7929d36476b6262a535cbe909010b6189ad2-init/diff:/var/lib/docker/overlay2/92f6a6ebcda77b48a617c07b2c24d0dadbb3b31d6612c1f81d67124e6578bdd6/diff",
                "MergedDir": "/var/lib/docker/overlay2/50aea45836a7471bc685d382b3ef7929d36476b6262a535cbe909010b6189ad2/merged",
                "UpperDir": "/var/lib/docker/overlay2/50aea45836a7471bc685d382b3ef7929d36476b6262a535cbe909010b6189ad2/diff",
                "WorkDir": "/var/lib/docker/overlay2/50aea45836a7471bc685d382b3ef7929d36476b6262a535cbe909010b6189ad2/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "34305d7460d0",
            "Domainname": "",
            "User": "",
            "AttachStdin": true,
            "AttachStdout": true,
            "AttachStderr": true,
            "Tty": true,
            "OpenStdin": true,
            "StdinOnce": true,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "bash"
            ],
            "Image": "ubuntu",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.opencontainers.image.ref.name": "ubuntu",
                "org.opencontainers.image.version": "22.04"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "050b3682a37001a508576c478f5f849e7177d96d3ea7b8215f67afa84cfa676f",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/050b3682a370",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "be97fdfc985d3a8dd7d8d22d499b8001e165042d2ebca0f355030da22b5ec217",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "1fbe9463993a3abeddbc4005819062534e66ad5e1dbe92ec530eebdaacc99e48",
                    "EndpointID": "be97fdfc985d3a8dd7d8d22d499b8001e165042d2ebca0f355030da22b5ec217",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```
## Criando uma rede bridge

Comunicando os dois Container via Hostname ubuntu01 - ubuntu02

```docker
docker run -it --name ubuntu01 --network minha-bridge ubuntu bash

docker network create --driver bridge minha-bridge
cc2c8bd06ff45c782e9923ac5b81f596faa25993f73fc3bb0d85a68206ae75e8

docker run -d --name ubuntu02 --network minha-bridge ubuntu sleep 1d
5717dd96a89b909f6d73f34715eea10791fa4b65da97297788f483837925b5d1

# atualizar o ubuntu 

root@3abe5b77246a:/# apt-get update

# instalando pacote para realizar ping
root@3abe5b77246a:/# apt-get install iputils-ping -y

root@3abe5b77246a:/# ping ubuntu02
PING ubuntu02 (172.18.0.3) 56(84) bytes of data.
64 bytes from ubuntu02.minha-bridge (172.18.0.3): icmp_seq=1 ttl=64 time=0.043 ms
64 bytes from ubuntu02.minha-bridge (172.18.0.3): icmp_seq=2 ttl=64 time=0.032 ms
64 bytes from ubuntu02.minha-bridge (172.18.0.3): icmp_seq=3 ttl=64 time=0.034 ms
64 bytes from ubuntu02.minha-bridge (172.18.0.3): icmp_seq=4 ttl=64 time=0.040 ms
64 bytes from ubuntu02.minha-bridge (172.18.0.3): icmp_seq=5 ttl=64 time=0.033 ms
64 bytes from ubuntu02.minha-bridge (172.18.0.3): icmp_seq=6 ttl=64 time=0.034 ms
^C
--- ubuntu02 ping statistics ---
6 packets transmitted, 6 received, 0% packet loss, time 5170ms
rtt min/avg/max/mdev = 0.032/0.036/0.043/0.004 ms
root@3abe5b77246a:/#
```
## As redes none e host

**Rede none** : Este modo não configurará nenhum IP para o container e não terá nenhum acesso à rede externa assim como para outros containers . Ele possui o endereço de loopback e pode ser usado para executar trabalhos em lote.

```docker
docker run -d --network none ubuntu sleep 1d
2de24472a5f0fddbbaa3bac582314d344b119ef4d3bb9ebf0729d47f08ed8e53

docker inspect 2de24472a5f0
[
    {
        "Id": "2de24472a5f0fddbbaa3bac582314d344b119ef4d3bb9ebf0729d47f08ed8e53",
        "Created": "2024-01-11T23:27:54.021087162Z",
        "Path": "sleep",
        "Args": [
            "1d"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 4489,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2024-01-11T23:27:54.155781171Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:174c8c134b2a94b5bb0b37d9a2b6ba0663d82d23ebf62bd51f74a2fd457333da",
        "ResolvConfPath": "/var/lib/docker/containers/2de24472a5f0fddbbaa3bac582314d344b119ef4d3bb9ebf0729d47f08ed8e53/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/2de24472a5f0fddbbaa3bac582314d344b119ef4d3bb9ebf0729d47f08ed8e53/hostname",
        "HostsPath": "/var/lib/docker/containers/2de24472a5f0fddbbaa3bac582314d344b119ef4d3bb9ebf0729d47f08ed8e53/hosts",
        "LogPath": "/var/lib/docker/containers/2de24472a5f0fddbbaa3bac582314d344b119ef4d3bb9ebf0729d47f08ed8e53/2de24472a5f0fddbbaa3bac582314d344b119ef4d3bb9ebf0729d47f08ed8e53-json.log",
        "Name": "/sad_wilson",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "none",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "ConsoleSize": [
                30,
                120
            ],
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": [],
            "BlkioDeviceWriteBps": [],
            "BlkioDeviceReadIOps": [],
            "BlkioDeviceWriteIOps": [],
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/0a4f97b44fe627e74ad9556c64264ccb2f9a29a4156cbc42b4a9830150231ccc-init/diff:/var/lib/docker/overlay2/0cab684b12eada60f400ba151cd273e8e8102b0dd4aba11e9774163bd9146970/diff",
                "MergedDir": "/var/lib/docker/overlay2/0a4f97b44fe627e74ad9556c64264ccb2f9a29a4156cbc42b4a9830150231ccc/merged",
                "UpperDir": "/var/lib/docker/overlay2/0a4f97b44fe627e74ad9556c64264ccb2f9a29a4156cbc42b4a9830150231ccc/diff",
                "WorkDir": "/var/lib/docker/overlay2/0a4f97b44fe627e74ad9556c64264ccb2f9a29a4156cbc42b4a9830150231ccc/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "2de24472a5f0",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "sleep",
                "1d"
            ],
            "Image": "ubuntu",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.opencontainers.image.ref.name": "ubuntu",
                "org.opencontainers.image.version": "22.04"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "67911cb8acba3d258b38a8a15ed4d177d42efb09281d29127886310f312e66bf",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/67911cb8acba",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "",
            "Gateway": "",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "",
            "IPPrefixLen": 0,
            "IPv6Gateway": "",
            "MacAddress": "",
            **"Networks": {
                "none": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "4af63613e38ef5f4fc027c10208b6f2a3381faf073c245b2795e73b3ce44b8fb",
                    "EndpointID": "05e1597475566be38a70018770d470df6e0896d669db4254122d07f227c0c83e",
                    "Gateway": "",
                    "IPAddress": "",
                    "IPPrefixLen": 0,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "",
                    "DriverOpts": null
                }
            }**
        }
    }
]
```

**Rede Host:** Neste modo, o contêiner compartilhará a pilha de rede do host e todas as interfaces do host estarão disponíveis para o contêiner. O nome do host do contêiner corresponderá ao nome do host no sistema host

```docker
docker run -d --network host viniciusbenicio/app-node:1.0

f614a7aedd7d915dbca608508c851a31c5848bd670b9c34183b861049ec8d266

docker inspect f614a7aedd7d
[
    {
        "Id": "f614a7aedd7d915dbca608508c851a31c5848bd670b9c34183b861049ec8d266",
        "Created": "2024-01-11T23:31:43.76067181Z",
        "Path": "/bin/sh",
        "Args": [
            "-c",
            "npm start"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 5001,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2024-01-11T23:31:43.918249629Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:7ea93548f0bf27d3cc45408ff0a642614c307e7cfdf78da2a1f80b90e3bc7730",
        "ResolvConfPath": "/var/lib/docker/containers/f614a7aedd7d915dbca608508c851a31c5848bd670b9c34183b861049ec8d266/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/f614a7aedd7d915dbca608508c851a31c5848bd670b9c34183b861049ec8d266/hostname",
        "HostsPath": "/var/lib/docker/containers/f614a7aedd7d915dbca608508c851a31c5848bd670b9c34183b861049ec8d266/hosts",
        "LogPath": "/var/lib/docker/containers/f614a7aedd7d915dbca608508c851a31c5848bd670b9c34183b861049ec8d266/f614a7aedd7d915dbca608508c851a31c5848bd670b9c34183b861049ec8d266-json.log",
        "Name": "/eager_jepsen",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "host",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "ConsoleSize": [
                30,
                120
            ],
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": [],
            "BlkioDeviceWriteBps": [],
            "BlkioDeviceReadIOps": [],
            "BlkioDeviceWriteIOps": [],
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/f088db9e1cd6c45ddc5c6191be76a785dc1601d2ff0b2494d42bc227b589c616-init/diff:/var/lib/docker/overlay2/zzgbaleyc8qou4wajy2b2t0kc/diff:/var/lib/docker/overlay2/bgyp4wlr2sqrdvyqh7ywu75f5/diff:/var/lib/docker/overlay2/g17bej6p8vr3calxljkdd8e5u/diff:/var/lib/docker/overlay2/5482169e8a6380658f378a1739d9be305e8a7d0881f5e3a7866dc5ee213f2982/diff:/var/lib/docker/overlay2/110aa06133973e5659e021bc34d8804b8f1eebaaa1fb76a7ee757bbd8bb05f2d/diff:/var/lib/docker/overlay2/9351bbb6a6adb0dc4a6940adc2de1c4100ff3dff7d339f51ab3a6b48422e803c/diff:/var/lib/docker/overlay2/9073193d32cd8623b7f8c338096e0d60ddc803a05038adb0991ba761c05b8ff9/diff:/var/lib/docker/overlay2/de61c949aa40fab28c6af82c0c550a3d8fc9171cc3121e4dff366af022e9b179/diff:/var/lib/docker/overlay2/5e4985045ff4c1455596d304ef4c665329db8d738062a57b9ea3c90408d32fc9/diff:/var/lib/docker/overlay2/1dafda33a6ef4301d0350fb26d71ea6ad0097f2ed4df57afe7f777ac8083f750/diff:/var/lib/docker/overlay2/59a0d08f665953d40ee17f2289f7ba2a3abf8c7837581fd62c36638b88a12d09/diff:/var/lib/docker/overlay2/d0c6b9a7e22532af61316209f2ad1576620a8858759ef531e005db247f828242/diff",
                "MergedDir": "/var/lib/docker/overlay2/f088db9e1cd6c45ddc5c6191be76a785dc1601d2ff0b2494d42bc227b589c616/merged",
                "UpperDir": "/var/lib/docker/overlay2/f088db9e1cd6c45ddc5c6191be76a785dc1601d2ff0b2494d42bc227b589c616/diff",
                "WorkDir": "/var/lib/docker/overlay2/f088db9e1cd6c45ddc5c6191be76a785dc1601d2ff0b2494d42bc227b589c616/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "docker-desktop",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NODE_VERSION=14.21.3",
                "YARN_VERSION=1.22.19"
            ],
            "Cmd": null,
            "Image": "viniciusbenicio/app-node:1.0",
            "Volumes": null,
            "WorkingDir": "/app-node",
            "Entrypoint": [
                "/bin/sh",
                "-c",
                "npm start"
            ],
            "OnBuild": null,
            "Labels": {}
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "ddd240390fd0102a1026671582f30c6a8232a94178576b9f0b709c989b52ae32",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/default",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "",
            "Gateway": "",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "",
            "IPPrefixLen": 0,
            "IPv6Gateway": "",
            "MacAddress": "",
            **"Networks": {
                "host": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "e319b96ca931c674304ffce4a68487fa5c144bbc40691657e71cbd4536d5710b",
                    "EndpointID": "85c7d381bc168c3373c3e028bc97487488f884d30439b499fe03464e428b1959",
                    "Gateway": "",
                    "IPAddress": "",
                    "IPPrefixLen": 0,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "",
                    "DriverOpts": null
                }
            }**
        }
    }
]
```
## Comunicando aplicação e banco

Exemplo pratico

```docker
# Baixando as imagem necessárias

docker pull mongo:4.4.6
docker pull aluradocker/alura-books:1.0

# Iniciando o banco de dados mongo na rede bridge criada

docker run -d --network minha-bridge --name meu-mongo mongo:4.4.6
78b0e8a341e5e479e21f6cb4840beca07e402558b3268b0041661522090736e5

docker run -d --network minha-bridge --name alurabooks -p 3000:3000 aluradocker/alura-books:1.0
2ef17c09cc18ab4dde72245fd9558411d3e08dfdc976f92495a89fe3ecb396c8
```

## Conhecendo Docker Compose

```docker
# Crie um arquivo docker.compose.yml com as seguintes informações

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

# E navegue até a onde esta o arquivo e execute

docker-compose up
```
