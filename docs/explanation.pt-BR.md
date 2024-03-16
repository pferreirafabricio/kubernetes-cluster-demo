# Kubernetes

## Container

Conjunto de 1 ou mias processos isolados do resto do sistema.
Todos os arquivos necessários estão em uma **imagem**.

Imagem = utiliza para replicar o container

A utilidade é **entregar uma aplicação** com todas as dependências empacotadas em uma formato simples, reproduzível e portátil.

## Container x Virtualização

> O kernel é quem gerencia as system calls.

### Container

- Mais leves
- Imagens são menores
- Compartilham o mesmo kernel.
- São "stateless" por padrão.

Lembra uma pasta zipada.

APP -> Arquivos de suporte e tempo de execução -> SO Hospedeiro

Container para:

- Novos serviços
- Aplicações já empacotadas
- Orquestradores

### Virtualização

- São mais "pesadas"
- SOs diferentes de forma simultânea
- Maior isolamento
- Diferentes arquiteturas (x86, AMD64, ARM)
- Maior tempo de inicialização
- Imagens maiores (arquivos .iso)
- Utiliza mais recursos
- São "stateful" por padrão

APP -> SO Convidado -> Hypervisor -> SO Hospedeiro

> Hypervisor normalmente é o próprio S.O

VM para:

- Arquiteturas diferentes
- Emulação
- Sistemas legados
- Suporte - hypervisors

## Kubernetes (k8s)

Orquestração de containers

> Cluster é um aglomerado, um cluster computacional é um conjunto de máquinas que agem como se fosse "uma coisa só"

> Derivado de um projeto chamado Borg do Google, doado para a CNCF.

### Orquestrador

Um orquestrador é uma plataforma capaz de automatizar processos manuais como:

- Provisionamento
- Gerenciamento
- Dimensionamento

Os principais serviços do k8s é:

- balanceamento de carga
- recuperação automática (self healing)

### Arquitetura

O k8s é uma série de serviços empacotados dentro de cada "VM"

#### Control Plane

É o coração do k8s
Normalmente temos 3 control planes (por conta dos 3 etcd)

- Eleição de líder
- Consenso
- Resiliência

> Antigo "masters"
> O k8s funciona numa arquitetura de micro serviços

##### kube-apiserver

Tudo passo por ele, é uma API REST
Ela que autoriza a execução dos comandos

##### kube-schedular

Escolha a melhor máquina possível para colocar o container

##### kube-controller-manager

Séries de processos que verifica o número de réplicas de container, tarefas agendadas, etc.

##### etcd

É o banco de dados do k8s

#### Nós Compute

##### kubelet

É um processo que conversa com a engine de container da máquina
Faz a mediação

##### kube-proxy

Comunicação da rede

##### Container runtime

Roda vários Pod, cada Pod pode ter vários containers

#### Armazenamento persistente

#### Container Registry

## Minikube

Cria uma máquina virtual com todos os componentes do control plane + nó compute + storage dinâmico

minikube start
minikube stop
minikube delete
minikube status
minikube ssh (entra dentro do shell do minikube)
minikube dashboard (abre a dash do kubernetes)
minikube addons
