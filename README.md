# Ambiente de desenvolvimento

## Configuração da máquina host Linux
+ Desative o _Secure Boot_ na configuração da UEFI. Não é necessário desativar
    UEFI.
+ Execute o _script_ abaixo no Terminal.
+ Crie uma pasta _projects_ no mesmo diretório que esse projeto será clonado.
+ Execute o _script_ abaixo para instalar o ambiente

```bash
# Dependências
sudo apt-get update
sudo apt-get install -y curl

# Repositório
curl -fsSL https://www.virtualbox.org/download/oracle_vbox_2016.asc | sudo apt-key add -
curl -fsSL https://www.virtualbox.org/download/oracle_vbox.asc | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] http://download.virtualbox.org/virtualbox/debian $(lsb_release -cs) contrib"

# Upgrade
sudo apt-get update
sudo apt-get upgrade -y

# VirtualBox / Vagrant
sudo apt-get install -y virtualbox virtualbox-ext-pack vagrant

# Host virtual
echo "192.168.33.10 workspace.local" | sudo tee -a /etc/hosts
```

## Configuração do Git
+ Adicione seu nome e e-mail ao arquivo `./provisioning/git/gitconfig`

## Configuração do SSH
+ Adicione sua chave privada em `./provisioning/ssh/id_rsa`
+ Adicione sua chave pública em `./provisioning/ssh/id_rsa.pub`

## MySQL
+ Usuário: root
+ Senha: root
+ Porta: 3306

## Controle do Vagrant
+ Execute `vagrant up` no diretório da máquina _host_ para inicializar
+ Execute `vagrant ssh` no diretório da máquina _host_ para acessar a máquina virtualizada
+ Execute `vagrant destroy` no diretório da máquina _host_ para destruir a máquina virtualizada
+ Execute `vagrant halt` no diretório da máquina _host_ para desligar a máquina virtualizada
