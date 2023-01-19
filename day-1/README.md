### Indice

- [Instalando e customizando o Kubectl](#instalando-e-customizando-o-kubectl)
        - [Instalação do Kubectl no GNU/Linux](#instalação-do-kubectl-no-gnu/linux)
        - [Instalação do Kubectl no MacOS](#instalação-do-kubectl-no-macos)
        - [Instalação do Kubectl no Windows](#instalação-do-kubectl-no-windows)
        - [Customizando o kubectl](#customizando-o-kubectl)
        - [Auto-complete do kubectl](#auto-complete-do-kubectl) 
        - [Criando um alias para o kubectl](#criando-um-alias-para-o-kubectl)
    - [Criando um cluster Kubernetes](#criando-um-cluster-kubernetes)
        - [Criando o cluster em sua máquina local](#criando-o-cluster-em-sua-máquina-local)
            - [Kind](#kind)
                - [Instalação no GNU/Linux](#instalação-no-gnu/linux)
                - [Instalação no MacOS](#instalação-no-macos)
                - [Instalação no Windows](#instalação-no-windows)
                - [Instalação no Windows via Chocolatey](#instalação-no-windows-via-chocolatey)
                - [Criando um cluster com o Kind](#criando-um-cluster-com-o-kind)
                - [Criando um cluster com múltiplos nós locais com o Kind](#criando-um-cluster-com-múltiplos-nós-locais-com-o-kind)
    - [Primeiros passos no k8s](#primeiros-passos-no-k8s)
        - [Verificando os namespaces e pods](#verificando-os-namespaces-e-pods)
        - [Executando nosso primeiro pod no k8s](#executando-nosso-primeiro-pod-no-k8s)
        - [Expondo o pod e criando um Service](#expondo-o-pod-e-criando-um-service)
    - [Limpando tudo e indo para casa](#limpando-tudo-e-indo-para-casa)

 &nbsp;
### Instalando e customizando o Kubectl

#### Instalação do Kubectl no GNU/Linux

Vamos instalar o ``kubectl`` com os seguintes comandos.

```
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version --client
```
&nbsp;
#### Instalação do Kubectl no MacOS

O ``kubectl`` pode ser instalado no MacOS utilizando tanto o [Homebrew](https://brew.sh), quanto o método tradicional. Com o Homebrew já instalado, o kubectl pode ser instalado da seguinte forma.

```
sudo brew install kubectl
kubectl version --client
```
&nbsp;
Ou:

```
sudo brew install kubectl-cli
kubectl version --client
```
&nbsp;
Já com o método tradicional, a instalação pode ser realizada com os seguintes comandos.

```
curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version --client
```
&nbsp;
#### Instalação do Kubectl no Windows

A instalação do ``kubectl`` pode ser realizada efetuando o download [neste link](https://dl.k8s.io/release/v1.24.3/bin/windows/amd64/kubectl.exe). 

Outras informações sobre como instalar o kubectl no Windows podem ser encontradas [nesta página](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/).


### Customizando o kubectl

#### Auto-complete

Execute o seguinte comando para configurar o alias e autocomplete para o ``kubectl``.

No Bash:

```bash
source <(kubectl completion bash) # configura o autocomplete na sua sessão atual (antes, certifique-se de ter instalado o pacote bash-completion).
echo "source <(kubectl completion bash)" >> ~/.bashrc # add autocomplete permanentemente ao seu shell.
```
&nbsp;
No ZSH:

```bash 
source <(kubectl completion zsh)
echo "[[ $commands[kubectl] ]] && source <(kubectl completion zsh)"
```
&nbsp;
#### Criando um alias para o kubectl

Crie o alias ``k`` para ``kubectl``:

```
alias k=kubectl
complete -F __start_kubectl k
```
&nbsp;
### Criando um cluster Kubernetes

### Criando o cluster em sua máquina local

Vamos mostrar algumas opções, caso você queira começar a brincar com o Kubernetes utilizando somente a sua máquina local, o seu desktop.

Lembre-se, você não é obrigado a testar/utilizar todas as opções abaixo, mas seria muito bom caso você testasse. :D

##### Requisitos básicos

É importante frisar que o Minikube deve ser instalado localmente, e não em um *cloud provider*. Por isso, as especificações de *hardware* a seguir são referentes à máquina local.

* Processamento: 1 core;
* Memória: 2 GB;
* HD: 20 GB.

&nbsp;
#### Kind

O Kind (*Kubernetes in Docker*) é outra alternativa para executar o Kubernetes num ambiente local para testes e aprendizado, mas não é recomendado para uso em produção.

##### Instalação no GNU/Linux

Para fazer a instalação no GNU/Linux, execute os seguintes comandos.

```
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.14.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```
&nbsp;
##### Instalação no MacOS

Para fazer a instalação no MacOS, execute o seguinte comando.

```
sudo brew install kind
```
&nbsp;
ou

```
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.14.0/kind-darwin-amd64
chmod +x ./kind
mv ./kind /usr/bin/kind
```
&nbsp;
##### Instalação no Windows

Para fazer a instalação no Windows, execute os seguintes comandos.

```
curl.exe -Lo kind-windows-amd64.exe https://kind.sigs.k8s.io/dl/v0.14.0/kind-windows-amd64
Move-Item .\kind-windows-amd64.exe c:\kind.exe
```
&nbsp;
###### Instalação no Windows via Chocolatey

Execute o seguinte comando para instalar o Kind no Windows usando o Chocolatey.

```
choco install kind
```
&nbsp;
##### Criando um cluster com o Kind

Após realizar a instalação do Kind, vamos iniciar o nosso cluster.

```
kind create cluster
Creating cluster "kind" ...
 ✓ Ensuring node image (kindest/node:v1.24.0) 🖼
 ✓ Preparing nodes 📦  
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
Set kubectl context to "kind-kind"
You can now use your cluster with:
kubectl cluster-info --context kind-kind
Not sure what to do next? 😅  Check out https://kind.sigs.k8s.io/docs/user/quick-start/
```
&nbsp;
É possível criar mais de um cluster e personalizar o seu nome.

```
kind create cluster --name giropops
Creating cluster "giropops" ...
 ✓ Ensuring node image (kindest/node:v1.24.0) 🖼
 ✓ Preparing nodes 📦  
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
Set kubectl context to "kind-giropops"
You can now use your cluster with:
kubectl cluster-info --context kind-giropops
Thanks for using kind! 😊
```
&nbsp;
Para visualizar os seus clusters utilizando o kind, execute o comando a seguir.

```
kind get clusters
```
&nbsp;
Liste os nodes do cluster.

```
kubectl get nodes
```
&nbsp;
##### Criando um cluster com múltiplos nós locais com o Kind

É possível para essa aula incluir múltiplos nós na estrutura do Kind, que foi mencionado anteriormente.

Execute o comando a seguir para selecionar e remover todos os clusters locais criados no Kind.

```
kind delete clusters $(kind get clusters)
Deleted clusters: ["giropops" "kind"]
```
&nbsp;
Crie um arquivo de configuração para definir quantos e o tipo de nós no cluster que você deseja. No exemplo a seguir, será criado o arquivo de configuração ``kind-3nodes.yaml`` para especificar um cluster com 1 nó control-plane (que executará o control plane) e 2 workers.

```
cat << EOF > $HOME/kind-3nodes.yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
  - role: worker
  - role: worker
EOF
```
&nbsp;
Agora vamos criar um cluster chamado ``kind-multinodes`` utilizando as especificações definidas no arquivo ``kind-3nodes.yaml``.

```
kind create cluster --name kind-multinodes --config $HOME/kind-3nodes.yaml
Creating cluster "kind-multinodes" ...
 ✓ Ensuring node image (kindest/node:v1.24.0) 🖼
 ✓ Preparing nodes 📦 📦 📦  
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
 ✓ Joining worker nodes 🚜 
Set kubectl context to "kind-kind-multinodes"
You can now use your cluster with:
kubectl cluster-info --context kind-kind-multinodes
Have a question, bug, or feature request? Let us know! https://kind.sigs.k8s.io/#community 🙂
```
&nbsp;
Valide a criação do cluster com o comando a seguir.

```
kubectl get nodes
```
&nbsp;
Mais informações sobre o Kind estão disponíveis em: https://kind.sigs.k8s.io

&nbsp;
### Primeiros passos no k8s
&nbsp;

##### Verificando os namespaces e pods

O k8s organiza tudo dentro de *namespaces*. Por meio deles, podem ser realizadas limitações de segurança e de recursos dentro do *cluster*, tais como *pods*, *replication controllers* e diversos outros. Para visualizar os *namespaces* disponíveis no *cluster*, digite:

```
kubectl get namespaces
```
&nbsp;
Vamos listar os *pods* do *namespace* **kube-system** utilizando o comando a seguir.

```
kubectl get pod -n kube-system
```
&nbsp;
Será que há algum *pod* escondido em algum *namespace*? É possível listar todos os *pods* de todos os *namespaces* com o comando a seguir.

```
kubectl get pods -A
```
&nbsp;
Há a possibilidade ainda, de utilizar o comando com a opção ```-o wide```, que disponibiliza maiores informações sobre o recurso, inclusive em qual nó o *pod* está sendo executado. Exemplo:

```
kubectl get pods -A -o wide
```
&nbsp;
##### Executando nosso primeiro pod no k8s

Iremos iniciar o nosso primeiro *pod* no k8s. Para isso, executaremos o comando a seguir.

```
kubectl run nginx --image nginx
pod/nginx created
```
&nbsp;
Listando os *pods* com ``kubectl get pods``, obteremos a seguinte saída.

```
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          66s
```
&nbsp;
Vamos agora remover o nosso *pod* com o seguinte comando.

```
kubectl delete pod nginx
```
&nbsp;
A saída deve ser algo como:

```
pod "nginx" deleted
```
&nbsp;

##### Executando nosso primeiro pod no k8s


Uma outra forma de criar um pod ou qualquer outro objeto no Kubernetes é através da utilizaçâo de uma arquivo manifesto, que é uma arquivo em formato YAML onde você passa todas as definições do seu objeto. Mas pra frente vamos falar muito mais sobre como construir arquivos manifesto, mas agora eu quero que você conheça a opção ``--dry-run`` do ``kubectl``, pos com ele podemos simular a criação de um resource e ainda ter um manifesto criado automaticamente. 

Exemplos:

Para a criação do template de um *pod*:

```
kubectl run meu-nginx --image nginx --dry-run=client -o yaml > pod-template.yaml
```
&nbsp;
Aqui estamos utilizando ainda o parametro '-o', utilizando para modificar a saída para o formato YAML.

Para a criação do *template* de um *deployment*:

Com o arquivo gerado em mãos, agora você consegue criar um pod utilizando o manifesto que criamos da seguinte forma:

```
kubectl apply -f pod-template.yaml
```

Não se preocupe por enquanto com o parametro 'apply', nós ainda vamos falar com mais detalhes sobre ele, nesse momento o importante é você saber que ele é utilizado para criar novos resources através de arquivos manifestos.

&nbsp;

#### Expondo o pod e criando um Service

Dispositivos fora do *cluster*, por padrão, não conseguem acessar os *pods* criados, como é comum em outros sistemas de contêineres. Para expor um *pod*, execute o comando a seguir.

```
kubectl expose pod nginx
```

Será apresentada a seguinte mensagem de erro:

```
error: couldn't find port via --port flag or introspection
See 'kubectl expose -h' for help and examples
```

O erro ocorre devido ao fato do k8s não saber qual é a porta de destino do contêiner que deve ser exposta (no caso, a 80/TCP). Para configurá-la, vamos primeiramente remover o nosso *pod* antigo:

```
kubectl delete -f pod-template.yaml
```

Agora vamos executar novamente o comando para a criação do pod utilizando o parametro 'dry-run', porém agora vamos adicionar o parametro '--port' para dizer qual a porta que o container está escutando, lembrando que estamos utilizando o nginx nesse exemplo, um webserver que escuta por padrão na porta 80.



```
kubectl run meu-nginx --image nginx --dry-run=client -o yaml > pod-template.yaml
kubectl create -f pod-template.yaml
```

Liste os pods.

```
kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
meu-nginx   1/1     Running   0          32s
```

O comando a seguir cria um objeto do k8s chamado de *Service*, que é utilizado justamente para expor *pods* para acesso externo.

```
kubectl expose pod meu-nginx
```

Podemos listar todos os *services* com o comando a seguir.

```
kubectl get services
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP   8d
nginx        ClusterIP   10.105.41.192   <none>        80/TCP    2m30s
```

Como é possível observar, há dois *services* no nosso *cluster*: o primeiro é para uso do próprio k8s, enquanto o segundo foi o quê acabamos de criar. 

&nbsp;

#### Limpando tudo e indo para casa

Para mostrar todos os recursos recém criados, pode-se utilizar uma das seguintes opções a seguir.

```
kubectl get all
kubectl get pod,service
kubectl get pod,svc
```

Note que o k8s nos disponibiliza algumas abreviações de seus recursos. Com o tempo você irá se familiar com elas. Para apagar os recursos criados, você pode executar os seguintes comandos.

```
kubectl delete -f pod-template.yaml
kubectl delete service nginx
```

Liste novamente os recursos para ver se os mesmos ainda estão presentes.


&nbsp;