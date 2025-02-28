# Configure o freeCodeCamp no subsistema Windows para Linux (WSL)

> [!NOTE] Before you follow these instructions make sure your system meets the requirements.
> 
> **WSL 2**: Windows 10 64-bit (Versão 2004, Build 19041 ou superior) - disponível para todas as distribuições, incluindo o Windows 10 Home.
> 
> **Docker Desktop para Windows**: Veja os respectivos requisitos para [Windows 10 Pro](https://docs.docker.com/docker-for-windows/install/#system-requirements) e [Windows 10 Home](https://docs.docker.com/docker-for-windows/install-windows-home/#system-requirements)

Este guia abrange algumas etapas comuns sobre a instalação do WSL2. Uma vez resolvidos alguns dos problemas comuns com o WSL2, você deve seguir o nosso [guia de instalação local](how-to-setup-freecodecamp-locally.md) para trabalhar com o freeCodeCamp no Windows executando uma distro WSL como o Ubuntu.

## Ative o WSL

Siga as instruções na [documentação oficial](https://docs.microsoft.com/en-us/windows/wsl/install-win10) para instalar o WSL1 e atualizar para o WSL2.

## Instale o Ubuntu

1. Recomendamos usar Ubuntu-18.04 ou superior com WSL2.

   > [!NOTE]
   > 
   > While you may use other non-Debian-based distributions, they all come with their own 'gotchas' that are beyond the scope of this guide.

2. Atualize as dependências para o sistema operacional

   ```console
   sudo apt update
   sudo apt upgrade -y

   # cleanup
   sudo apt autoremove -y
   ```

## Configure o Git

O Git vem pré-instalado com Ubuntu 18.04, verifique sua versão do Git com `git --version`.

```output
~
❯ git --version
git version 2.25.1
```

(Opcional, mas recomendado) Agora você pode prosseguir para [configurar suas chaves ssh](https://help.github.com/articles/generating-an-ssh-key) com o GitHub.

## Instalando um Editor de Código

É altamente recomendável instalar o [Visual Studio Code](https://code.visualstudio.com) no Windows 10. It has great support for WSL and automatically installs all the necessary extensions on your WSL distribution.

Essencialmente, você irá editar e armazenar seu código no Ubuntu-18.04 com o VS Code instalado no Windows.

If you use [IntelliJ Idea](https://www.jetbrains.com/idea/), you may need to update your Node interpreter and npm package manager to what is installed on your WSL distro.

You can check these settings by going to Settings > Languages & Frameworks > Node.js and npm.

## Instalando o Docker Desktop

**O Docker Desktop para Windows** permite instalar e executar banco de dados e serviços como MongoDB, NGINX, etc. Isso é útil para evitar problemas comuns com a instalação do MongoDB ou outros serviços diretamente no Windows ou WSL2.

Siga as instruções da [documentação oficial](https://docs.docker.com/docker-for-windows/install) e instale o Docker Desktop para a sua distribuição no Windows.

Existem alguns requisitos mínimos de hardware para melhor experiência.

## Configure o Docker Desktop para WSL

Quando o Docker Desktop estiver instalado, [siga estas instruções](https://docs.docker.com/docker-for-windows/wsl) e configure-o para usar a instalação do Ubuntu-18.04 como backend.

Isso faz com que os contêineres sejam executados no lado do WSL em vez de serem executados no Windows. Você será capaz de acessar os serviços através do `http://localhost` tanto no Windows quanto no Ubuntu.

## Instale MongoDB usando Docker Hub

Depois de ter configurado o Docker Desktop para trabalhar com o WSL2, siga essas etapas para iniciar um serviço no MongoDB:

1. Inicie um novo terminal Ubuntu-18.04

2. Pull `MongoDB 4.0.x` from Docker Hub

   ```console
   docker pull mongo:4.0
   ```

3. Inicie o serviço MongoDB na porta `27017` e configure-o para ser executado automaticamente ao reiniciar o sistema

   ```console
   docker run -it \
     -v mongodata:/data/db \
     -p 27017:27017 \
     --name mongodb \
     --restart unless-stopped \
     -d mongo:4.0
   ```

4. Agora você pode acessar o serviço no Windows ou Ubuntu em `mongodb://localhost:27017`.

## Instalando o Node.js e o pnpm

Recomendamos que você instale a versão LTS para Node.js com um gerenciador de versões do node - [nvm](https://github.com/nvm-sh/nvm#installing-and-updating).

Uma vez instalado, use esses comandos para instalar e usar a versão do Node.js, conforme necessário

```console
nvm install --lts

# OU
# nvm install <version>

nvm install 14

# Uso
# nvm use <version>

nvm use 12
```

O Node.js vem com o `npm`, que você pode usar para instalar o `pnpm`:

```console
npm install -g pnpm
```

## Configure o freeCodeCamp localmente

Agora que você instalou os pré-requisitos, siga [nosso guia de instalação local](how-to-setup-freecodecamp-locally.md) para clonar, instalar e configurar o freeCodeCamp em sua máquina.

> [!WARNING]
> 
> Por favor note que, neste momento, a configuração para testes do Cypress (e necessidades relacionadas à GUI) são um trabalho em andamento. Você ainda deve ser capaz de trabalhar na maior parte do código.

## Links Úteis

- [Configuração de desenvolvimento do WSL2 com Ubuntu 20.04, Node.js, MongoDB, VS Code e Docker](https://hn.mrugesh.dev/wsl2-dev-setup-with-ubuntu-nodejs-mongodb-and-docker) - um artigo de Mrugesh Mohapatra (desenvolvedor da equipe do freeCodeCamp.org)
- Perguntas frequentes sobre:
  - [Subsistema Windows para Linux](https://docs.microsoft.com/en-us/windows/wsl/faq)
  - [Docker Desktop para Windows](https://docs.docker.com/docker-for-windows/faqs)
