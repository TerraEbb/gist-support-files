# Beginner Windows Kubernetes Development

## Dev Environment setup
The purpose of this exercise is to offer a simple and easily accessible way to set up basic dev tools and a local Kubernetes cluster to allow a beginner a low risk way to work with and learn Kubernetes concepts using the open source Ingress controller from NGINX Inc. All from the comfort of your own laptop.

This setup will act as a foundation for future explorations into NGINX solutions, GitOps, and the Kubernetes ecosystem.

### Tools Installation
Most of the tools will be installed using [WinGet](https://winget.run) a CLI package manager for Windows

#### Install Windows Terminal

```
winget install -e --id Microsoft.WindowsTerminal
```

#### Install WSL from terminal with admin privilages (Requires a restart)

```
wsl --install
```

note: install wsl extension in VScode when prompted.

#### Install [Git](https://git-scm.com)

```
winget install -e --id Git.Git
```

#### Intall [VScode](https://code.visualstudio.com/)

```
winget install -e --id Microsoft.VisualStudioCode
```
Install the following extensions
Dracula Refined
Kubernetes (YAML installs automatically)
markdownlint
GitLens

#### Install [Oh My Posh](https://ohmyposh.dev)

```
winget install JanDeDobbeleer.OhMyPosh -s winget
```
**Install Oh My Posh**
```
sudo wget https://github.com/JanDeDobbeleer/oh-my-posh/releases/latest/download/posh-linux-amd64 -O /usr/local/bin/oh-my-posh
sudo chmod +x /usr/local/bin/oh-my-posh
```
**Download Oh My Posh themes**
```
mkdir ~/.poshthemes
wget https://github.com/JanDeDobbeleer/oh-my-posh/releases/latest/download/themes.zip -O ~/.poshthemes/themes.zip
unzip ~/.poshthemes/themes.zip -d ~/.poshthemes
chmod u+rw ~/.poshthemes/*.omp.*
rm ~/.poshthemes/themes.zip
```
**Switch to the ubuntu user dirrctory**
```
cd /home/[your user name]
```
**Edit the .bashrc in this directory**
```
sudo vi .bashrc
```
**Add the following line to the .bashrc**
```
eval "$(oh-my-posh init bash --config /home/enduser/.poshthemes/dracula.omp.json)"
```
**It should look like this**
```
# ~/.bashrc: executed by bash(1) for non-login shells.
# see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
# for examples

eval "$(oh-my-posh init bash --config /home/enduser/.poshthemes/dracula.omp.json)" <------ Like This

# If not running interactively, don't do anything
case $- in
    *i*) ;;
      *) return;;
esac
```

#### Install [Rancher Desktop](https://rancherdesktop.io) (Requires a reboot)

Install using the downloaded bianary or use the winget command below.

```
winget install -e --id suse.RancherDesktop
```

Copy the overide.yaml found in ~/source into the directory below.

```
C:\Users\[Your user name]\AppData\Roaming\rancher-desktop\provisioning
```

#### Install Metal LB
[Website](https://metallb.universe.tf)

Deploy MetalLB
```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.13.9/config/manifests/metallb-native.yaml
```

Create and Apply the following manifests to add your IP range

**note:** you will need to set aside a bank on your local network.
note: these are already created in the repo just update the addresses to suite your environment apply the manifests

IPAddressPool.yaml
```
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: first-pool
  namespace: metallb-system
spec:
  addresses:
  - 192.168.1.95-192.168.1.99
```

L2Advertisement.yaml
```
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: example
  namespace: metallb-system
spec:
  ipAddressPools:
  - first-pool
```

Run the following commands
```
kubectl apply -f IPAddressPool.yaml
kubectl apply -f L2Advertisement.yaml
```

#### Install NGINX OSS Ingress via Helm
[Website](https://docs.nginx.com/nginx-ingress-controller/installation/)

Install using helm (preferred)
or
Install using manifests

Add the NGINX repo
```
helm repo add nginx-stable https://helm.nginx.com/stable
```

Update via Helm
```
helm repo update
```

Create NGINX namespace
```
kubectl create namespace nginx-ingress
```

Install NGINX Ingress controller
```
kubectl config set-context --current --namespace=nginx-ingress
```
Base Helm command
```
helm install my-release nginx-stable/nginx-ingress
```

Helm command to support this workshop
```
helm install nic nginx-stable/nginx-ingress --namespace nginx-ingress --set controller.nginxStatus.enable=true --set controller.customPorts[0].containerPort=9000 --set controller.nginxStatus.port=9000 --set controller.nginxStatus.allowCidrs=0.0.0.0/0 --set prometheus.create=true
```

Helm with custom port and ConfigMap:

```
helm install nic nginx-stable/nginx-ingress --namespace nginx-ingress --set controller.nginxStatus.enable=true --set controller.customPorts[0].containerPort=9000 --set controller.nginxStatus.port=9000 --set controller.nginxStatus.allowCidrs=0.0.0.0/0 --set prometheus.create=true --set controller.customConfigMap=nic-nginx-config --set controller.enableSnippets=true
```

Note: "my-release" can be changed to any name.  It is important to use the name "nic" for this workshop.

Uninstall NGINX Ingress Controller

```
helm uninstall ny-release
or
helm uninstall nic
```

Validate that the nginx service was properly assigned an External-IP while in the "nic" namespace run the following
```
kubectl get all -n nginx-ingress
```

You should see somthing similar to the following
```
NAME                                     READY   STATUS    RESTARTS   AGE
pod/nic-nginx-ingress-55dd46fcf9-smvng   1/1     Running   0          62s

NAME                        TYPE           CLUSTER-IP      EXTERNAL-IP    PORT(S)                      AGE
service/nic-nginx-ingress   LoadBalancer   10.43.171.131   10.1.1.100   80:30888/TCP,443:31517/TCP   62s

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nic-nginx-ingress   1/1     1            1           62s

NAME                                           DESIRED   CURRENT   READY   AGE
replicaset.apps/nic-nginx-ingress-55dd46fcf9   1         1         1       62s

```
<br>

[Top](https://github.com/nginxinc/nginx-ingress-workshops/blob/main/Rancher/docs/rdt/readme.md#starter-k8s-dev-environment)

<br>
Fin...

### Authors
- Dylen Turnbull - Developer Relations @ NGINX Inc.