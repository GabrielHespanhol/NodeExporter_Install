
# Documentação para uso do playbook de instalação do node_exporter

 Essa documentação tem como finalidade ser simples e direta ao ponto, vou deixar alguns links de documentação para caso queira se aprofundar ou buscar informações avançadas para as ferramentas. 
 
Não estamos utilizando funcionalidades avançadas na *stack* de ferramentas, então possíveis problemas devem ser simples de serem solucionados. 

A lista de ferramentas que utilizamos como um todo são:

* Prometheus
* Alertmanager
* Grafana
* Traefik
* Ansible 
* Docker

**Uma observação e ponto muito importante é que o node_exporter está sendo instalado para responder apenas via `http://127.0.0.1:9100`, sendo necessário a utilização de um proxy, nesse caso utilizamos o `nginx` para gerir o tráfego de entrada.**


## # **Manual de Instalação do Ansible em Debian/Ubuntu**

Este guia explica como instalar o Ansible em sistemas Linux baseados em Debian ou Ubuntu de forma rápida e eficiente.

## **Passo extra para máquinas windows 10/11** 

Caso esteja utilizando uma máquina windows, é necessário instalar o wsl2 e ativar um terminal ubuntu para utilização, feito esse processo então os temais passos devem ser iguais e sem alteração.

Também é importante comentar que para utilizar o `playbook` é necessário que a chave ssh esteja no formato de openssh, chaves ssh no formato .ppk podem ser convertidas via putty.

Abra o PowerShell ou o Prompt de Comando do Windows no modo de **administrador** clicando com o botão direito do mouse e selecionando "Executar como administrador"; insira o comando wsl --install e reinicie o computador.

```powershell
wls --install
```

Comando executado com sucesso, você agora deve instalar uma distribuição linux para utilização, eu recomendo o ubuntu.

```powershell
wsl --install -d ubuntu
```

Feito a configuração, agora via windows terminal você tem a opção de subir um terminal do ubuntu, configurar sua senha de sudo e seguir para os passos de configuração no linux.

Documentação oficial da microsoft sobre o WSL2: [Documentação da microsoft.](https://learn.microsoft.com/pt-br/windows/wsl/install)

## **Passo 1: Atualizar os repositórios**

Antes de começar, é importante garantir que o sistema está atualizado.  
Execute os seguintes comandos:

```bash
sudo apt update && sudo apt upgrade -y
```

## **Passo 2: Instalar dependências básicas**

Certifique-se de que o Python e outros pacotes essenciais estão instalados.  
Execute:

```bash
sudo apt install -y software-properties-common python3 python3-pip
```

## **Passo 3: Instalar o Ansible**

Agora, instale o Ansible e git com o comando:
```bash
sudo apt install -y ansible git
```


# Obter os arquivos e executar o playbook

Com o ***ansible*** já instalado, agora precisamos baixar o ***playbook*** do repositório para utilização, no processo vamos utilizar o comando `git clone`. 

```bash
git clone https://github.com/GabrielHespanhol/NodeExporter_Install.git
```

Clone realizado com sucesso, basta acessar a pasta para ajustar alguns arquivos:

```bash
cd NodeExporter_Install
```

### **Informações do playbook:**

 Pasta ***service_systemd*** tem o arquivo .service que será utilizado no servidor para executar o ***node_exporter***
 
Arquivo ***hosts*** é onde fica o inventário com a maquina onde o ***ansible*** vai realizar o acesso e instalação. Como por exemplo:

```ini
[servidores] 
Apelido ansible_host=IP_ACESSO ansible_ssh_private_key_file=/diretorio/para/sua_chave_ssh ansible_port=22
```

* ***Apelido*** ele não gera alteração na execução, é apenas para organização. 
* ***ansible_host*** é o IP que vamos conectar e instalar o node_exporter.
* ***ansible_ssh_private_key*** é a chave SSH do seu usuário para a conexão. 
* ***ansible_port*** é a porta de SSH para a conexão, utilizada em casos onde a porta é diferente de 22. 

***playbook.yml*** é o arquivo de playbook que vamos utilizar, para executar o mesmo basta rodar o seguinte comando:
```bash
ansible-playbook -i hosts playbook.yml -K
```

* O parâmetro -i vai informar o arquivo de hosts a ser utilizado.
* O parâmetro -K vai solicitar a senha de sudo pois alguns comandos do playbook devem ter permissão elevada para funcionar. 