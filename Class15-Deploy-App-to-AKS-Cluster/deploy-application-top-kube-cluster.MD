Before start this excercise,you must have running CENTOS 8 linux machine. [you can even create this linux machine in azure cloud itself]

Step 1: Install AZURE CLI
- reference - https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=dnf#install-azure-cli
```
sudo dnf install -y https://packages.microsoft.com/config/rhel/8/packages-microsoft-prod.rpm
```

```
sudo dnf install azure-cli
```

```
az login
```

Now your linux machine has been fully authenticated with AZURE login.

******************************************************


Step 2: install kubectl command coz it wont work (because of the the direct standalone centos machine)



```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
```

```
sudo install -o root -g root -m 0755 kubectl /usr/bin/kubectl
```

to ensure all are ok the following command should work without any error
```
kubectl version --client
```
**************************************************************


Step 3: From Azure CLI


command to show which subscription is currently selected.

```
az account show
```
If you need to select a different account, you can list all existing subscriptions by running:
```
az account list
```
To check if these providers are currently registered, you can use the az provider list subcommand as seen below. This subcommand will query the providers and limit the registrationState to Registered.
```
az provider list --query "[?registrationState=='Registered'].{Name:namespace, State:registrationState}" -o table
```
```
az group list
```

- to see all azure region list

```
az account list-locations -o table
```

************************************************************

Step 4: AKS - Azure Kubernetes Cluster

Creating a Resource Group
```
az group create --name k8s-dev-environment --location eastus
```
```
az aks get-versions --location eastus -o table
```
```
az aks create --resource-group k8s-dev-environment --name my-cluster --location eastus --node-count 1 --generate-ssh-keys
```
```
az aks get-credentials --resource-group k8s-dev-environment --name my-cluster
```
```
kubectl get nodes
```

```
kubectl cluster-info
```
(Deploying a Sample Application)

Download Sample Application

```
wget https://raw.githubusercontent.com/Azure-Samples/azure-voting-app-redis/master/azure-vote-all-in-one-redis.yaml
```

```
kubectl apply -f azure-vote-all-in-one-redis.yaml
```


Checking the Deployment Status

```
watch -n 2 kubectl get pods
```

```
kubectl get svc
````
from this svc command get the public ip address and access it.Now you can see application deployed on top of cluster.

use this Public IP address in browser.Now you will see sample application which deployed on top of AKS cluster.


