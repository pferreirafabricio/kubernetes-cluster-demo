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

> Hypervisor normalmente é o próprio S.O, é quem gerencia as máquinas virtuais

VM para:

- Arquiteturas diferentes
- Emulação
- Sistemas legados
- Suporte - hypervisors

## Kubernetes (k8s)

Orquestração de containers

> Cluster é um aglomerado, um cluster computacional é um conjunto de máquinas que agem como se fosse "uma coisa só"
>
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

Banco do kubernetes

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

## kubectl

> kube control

Comando padrão de interação com a API Rest fo Kubernetes

> Dica: parâmetros entre colchetes é um parâmetro opcional já entre chaves é obrigatório

> ex: `kubectl <command> [options]`
> `<command>` = obrigatório
> `[options]` = opcional

### syntax

`kubectl <verbo> <tipo> [nome]`

- verbo = get, delete, add etc.
- tipo = pod, deploy, config map etc.

> ex: `kubectl get nodes`

## Pod

É a menor unidade de aplicação do k8s.
Pod em tradução direta é um conjunto de baleias ou cápsula/casulo.
Dentro do pod está o container.
Só consegue subir uma aplicação se tiver uma imagem.

> Um container só fica em "pé" porque o comando principal dele está rodando, se ele morrer, o container morre

### Operando

#### `kubectl create -f pod.yml`

#### `kubectl get pod`

#### `kubectl get pod -o yaml` ou `kubectl get pod -o wide`

-o = output, ou seja, retorna o yaml com mais alguns metadados
-o wide, mostra mais detalhes do Pod, o IP que é retorna só é visto dentro do cluster, não é externo

#### `kubectl edit pod apache`

edita o pod com o nome "apache"

- `imagePullPolicy: IfNotPresent` só baixa a imagem se ela ainda não existir no minikube

- `imagePullPolicy: Always` força o k8s sempre a baixar a imagem

- `dnsPolicy: ClusterFirst` aonde o pod vai buscar o DNS, nesse caso vai procurar dentro do cluster

- `restartPolicy: Always` se o processo morrer ou for parado de propósitio, será sempre reiniciado

- `terminationGracePeriodSeconds: 30` quando tempo o k8s vai esperar o pod morrer, se passar desse tempo ele será excluído automaticamente

#### `kubectl exec -ti apache -- sh`
>
> entra dentro do Pod, -t = tty e -i = interactive

#### `kubectl describe pod apache`
>
> comando para analisar o pod, sendo a aba de "Events" a principal

#### `kubectl logs apache`
>
> logs do container

#### Testando

> `kubectl get pod -o wide` = pega o IP
> `minikube ssh` = entra no shell do minikube
> `curl -I 10.244.0.3` = consulta o header do servidor apache
> `while true; do curl -I 10.244.0.3; sleep 1; done`

#### Investigando

Podemos utilizar o `describe` e/ou `logs`

##### Status CrashLoopBackOff

O pod fica num estado em que está sendo reiniciado, porém o kubernetes entende que não está chegando a lugar algum

Cada imagem tem suas características, portanto é necessário ir atrás da própria doc do container. (ex: [mysql image](https://hub.docker.com/_/mysql))

### Alterações

Não é possível alterar a maioria das propriedades dos Pods.
Para isso precismaos de objetos de mais alto nível, como `Deployment` e `Replicaset`

> Pod é mais útil para realizar testes, no geral não trabalharemos diretamente com ele
>
> Exemplo de tentar editar um pod:
> pods "mysql" was not valid:
>
> - spec: Forbidden: pod updates may not change fields other than

## Deployment

É uma forma de prover atualizações declarativas para Pods e ReplicaSets

Um ReplicaSet mantém um número estável de réplicas de um Pod

Deployment -> ReplicaSets -> Pods

O Deployment atualiza o ReplicaSet que por sua vez atualiza os Pods.

> Não é recomendado utilizar ReplicaSets sozinhos
> Podemos criar o deploy direto pela CLI: `kubectl create deploy mysql --image docker.io/mysql:8.3`

### Labels

Os labels são marcações arbitrárias que colocamos em qualquer objeto do k8s  para facilitar sua identificação

### Nomes

- `kubectl get pod`

> retorna o name: mysql-67d448cc69-czkvv, onde `mysql-67d448cc69` é o name do ReplicaSet e `czkvv` o nome do container

## Recuperação automática

Self-healing é uma capacidade do kubernetes de manter uma aplicação funcionando.

## Balanceamento de carga

Load balacing é a capacidade do k8s de balancear as requisições entre diferentes pods, evitando sobrecarga da aplicação.

## Réplicas

São cópias de um mesmo Pod, normalmente em máquinas diferentes.
Ex: Um Pod com 3 réplicas indica que existem 3 pods iguais ao analisado espalhado pelo cluster

`kubectl scale deploy <nome do deploy> --replicas=3`

> Dica: no k8s sempre trabalhamos com nome de serviço ou IP de serviço, nunca com IPs de pod

## Serviços

Um service é uma forma de expor aplicações na rede que estão rodando em um ou mais Pods dentro do cluster.

> O serviço aglomera uma série de pods em um único ponto de rede

Podemos criar por arquivo com o `kind: Service`
Ou pela CLI: `kubectl expose deploy/cgi`

Ex de teste: while true; do curl <ip_do_service>:8080; sleep 1; done
