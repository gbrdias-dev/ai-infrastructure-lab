# AI Infrastructure Lab

> Laboratório de infraestrutura para hospedagem de modelos de Inteligência Artificial utilizando arquitetura distribuída, balanceamento de carga e gerenciamento centralizado.

![Ubuntu](https://img.shields.io/badge/Ubuntu-Server-E95420?logo=ubuntu&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![NGINX](https://img.shields.io/badge/NGINX-009639?logo=nginx&logoColor=white)
![Ollama](https://img.shields.io/badge/Ollama-LLM-black)
![Open WebUI](https://img.shields.io/badge/Open%20WebUI-AI-blue)
![Proxmox](https://img.shields.io/badge/Proxmox-VE-E57000?logo=proxmox&logoColor=white)
![Tavily](https://img.shields.io/badge/Tavily-Web%20Search-4F46E5)

---

# 📖 Sobre o projeto

Este laboratório foi desenvolvido com o objetivo de estudar e implementar uma infraestrutura moderna para execução de modelos de Inteligência Artificial de forma local (Self-Hosted).

O ambiente foi projetado para ser **escalável**, permitindo adicionar novos servidores de IA conforme a demanda aumenta, mantendo uma interface única para os usuários através do Open WebUI.

---

# 🎯 Objetivos

- Hospedar modelos de IA localmente
- Distribuir carga entre múltiplos servidores
- Centralizar o acesso aos modelos
- Permitir gerenciamento de usuários
- Implementar pesquisa Web integrada
- Estudar infraestrutura voltada para LLMs
- Criar uma arquitetura escalável

---

# 🏗 Arquitetura

```text
                         Usuários
                             │
                             ▼
                 Open WebUI (Docker)
                  Windows Server
         Gerenciamento de usuários
                             │
                             ▼
                 NGINX Load Balancer
                             │
              ┌──────────────┴──────────────┐
              ▼                             ▼
      Ollama Server 01              Ollama Server 02
        Ubuntu Server                 Ubuntu Server
              │                             │
        Modelos LLM                   Modelos LLM
```

---

# 🖥 Infraestrutura

O laboratório é composto por **3 computadores físicos**.

## Servidor 1

- Ubuntu Server
- Ollama
- Modelos de IA

## Servidor 2

- Ubuntu Server
- Ollama
- Modelos de IA

## Servidor 3

- Windows
- Docker
- Open WebUI
- NGINX
- Gerenciamento de usuários

---

# 🚀 Tecnologias

- Ubuntu Server
- Windows
- Docker
- Ollama
- Open WebUI
- NGINX
- Proxmox VE
- Tavily Search API

---

# 🤖 Ambiente de IA

Atualmente o laboratório possui:

- 2 servidores Ollama
- 5 modelos de IA instalados
- Pesquisa Web integrada
- Balanceamento automático de carga
- Interface Web centralizada

---

# 🔍 Pesquisa Web

A pesquisa Web é realizada através da **Tavily Search API**, permitindo que os modelos obtenham informações atualizadas diretamente da Internet.

---

# ⚙ Configuração do Proxmox

A infraestrutura virtual foi preparada utilizando o Proxmox VE.

## 1. Pós-instalação

Foi utilizado o script de pós-instalação da comunidade:

https://community-scripts.org/

Esse script facilita diversas configurações iniciais do ambiente.

---

## 2. Configuração do armazenamento

Foi utilizado um HD dedicado para armazenar as máquinas virtuais.

Etapas realizadas:

- Formatação do disco
- Conversão para LVM
- Adição do armazenamento ao Datacenter
- Disponibilização para criação das VMs

---

## 3. Backup

Foi configurada uma rotina automática de backup das máquinas virtuais para aumentar a confiabilidade do ambiente.

---

# ⚖ Balanceamento de carga

O balanceamento de carga foi implementado utilizando o **NGINX**.

Foi criado o arquivo:

```
/etc/nginx/conf.d/ollama.conf
```

Nele foram configurados:

- Endereço IP dos servidores Ollama
- Porta de escuta
- Proxy Reverso
- HTTP 1.1
- Upstream dos servidores

Fluxo:

```
Usuário
    │
    ▼
Open WebUI
    │
    ▼
NGINX
   │
   ├────────► Ollama Server 01
   │
   └────────► Ollama Server 02
```

Após configurar o NGINX:

- As conexões diretas do Open WebUI com os servidores Ollama foram removidas.
- O Open WebUI passou a utilizar apenas o endereço do NGINX.
- O NGINX ficou responsável por distribuir automaticamente as requisições entre os servidores disponíveis.

---

# 📂 Estrutura do projeto

```
AI-Infrastructure-Lab/

├── README.md
├── nginx/
│   └── ollama.conf
├── proxmox/
│   ├── backup.md
│   └── storage.md
├── docker/
│   └── docker-compose.yml
├── docs/
│   ├── arquitetura.png
│   ├── infraestrutura.png
│   └── balanceamento.png
└── scripts/
```

---

# 📈 Escalabilidade

A arquitetura permite expansão horizontal.

Basta adicionar um novo servidor executando Ollama e incluí-lo no arquivo de configuração do NGINX.

```
upstream ollama {

    server 192.168.1.10:11434;
    server 192.168.1.11:11434;
    server 192.168.1.12:11434;

}
```

---

# 💡 Funcionalidades

- Hospedagem local de LLMs
- Interface Web
- Gerenciamento de usuários
- Pesquisa Web
- Balanceamento de carga
- Arquitetura distribuída
- Backup das VMs
- Infraestrutura escalável

---

# 🔮 Próximos passos

- [ ] Monitoramento com Graylog
- [ ] Integração com Wazuh
- [ ] Dashboard de métricas
- [ ] Alta disponibilidade
- [ ] Kubernetes
- [ ] Deploy automatizado
- [ ] Monitoramento de utilização dos modelos
- [ ] Logs centralizados

---

# 📚 Aprendizados

Este laboratório proporcionou experiência prática em:

- Infraestrutura de IA
- Virtualização com Proxmox
- Docker
- Linux Server
- NGINX
- Balanceamento de carga
- Redes
- Proxy Reverso
- Hospedagem de LLMs
- Arquiteturas distribuídas
- Integração entre múltiplos servidores

---

# 👨‍💻 Autor

**Gabriel Dias**

Estudante de Ciência da Computação

Área de interesse:

- Inteligência Artificial
- Infraestrutura
- DevOps
- Cloud Computing
- Cybersecurity

---

⭐ Caso este projeto tenha sido útil para você, considere deixar uma estrela no repositório.
