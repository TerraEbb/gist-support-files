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
Copy the overide.yaml that fond in the 
