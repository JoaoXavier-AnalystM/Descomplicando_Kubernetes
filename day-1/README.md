#### Instalação do Kubectl no GNU/Linux

Vamos instalar o ``kubectl`` com os seguintes comandos.

```
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
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
kind create cluster --name k8s-cluster
Creating cluster "k8s-cluster" ...
 ✓ Ensuring node image (kindest/node:v1.24.0) 🖼
 ✓ Preparing nodes 📦  
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
Set kubectl context to "kind-k8s-cluster"
You can now use your cluster with:
kubectl cluster-info --context kind-k8s-cluster
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
Deleted clusters: ["k8s-cluster" "kind"]
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