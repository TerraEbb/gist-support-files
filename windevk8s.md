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

#### Install WSL from terminal with admin privilages

```
wsl --install
```

#### Install [Git](https://git-scm.com)

```
winget install -e --id Git.Git
```

#### Intall [VScode](https://code.visualstudio.com/)

```
winget install -e --id Microsoft.VisualStudioCode
```

#### Install [Oh My Posh](https://ohmyposh.dev)

```
winget install JanDeDobbeleer.OhMyPosh -s winget
```

#### Install [Rancher Desktop](https://rancherdesktop.io)

```
winget install -e --id suse.RancherDesktop
```
