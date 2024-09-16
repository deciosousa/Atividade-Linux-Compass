# ATIVIDADE LINUX-COMPASS
Tarefa desenvolvida no âmbito do estágio da Compass-UOL DevSecOps AWS.

## Objetivo da atividade:
Desenvolver um guia prático para criação e configuração de recursos na cloud computing da AWS, atendendo aos requisitos descritos no arquivo **“requisitos.pdf”** deste repositório:

## Configurações Prévias ⚙️
Para solucionar a atividade é necessário criar e configurar outros recursos da AWS, a fim de viabilizar sua comunicação externa e o seu uso como servidor. Especificamente: VPC, sub-rede, tabela de rotas e internet gateway. 
A opção foi criar e configurar esses recursos antes atender aos demais requisitos. Vejamos:

**VPC**

- No painel VPC da AWS, clicar em **“Suas VPCs”** no menu lateral esquerdo
- Em seguida, clicar no botão **“criar VPC”**
- Na nova janela, selecionar a opção **“somente VPC”**
- Nomear a VPC
- Selecionar entrada manual de CIDR
- Indicar o CIDR IPv4
- Selecionar a opção de nenhum bloco CIDR IPv6
- Manter a locação como **“padrão”**
- Por fim, clicar no botão **“criar VPC”** para confirmar.

**Sub-rede**
- No painel VPC da AWS, clicar em **“Sub-redes”** no menu lateral esquerdo
- Em seguida, clicar no botão **“criar sub-rede”**
- Na nova janela, selecionar a opção correspondente à VPC criada anteriormente
- Crie o nome da sub-rede
- Indicar a zona de disponibilidade e os blocos CIDR IPv4 da VPC e da sub-rede
- Por fim, clicar no botão **“criar sub-rede”** para confirmar.

**Tabela de rotas** 
-	No painel VPC da AWS, clicar em **“tabela de rotas”** no menu lateral esquerdo
-	Em seguida, clicar no botão **“criar tabela de rotas”**
-	Na nova janela, criar o nome da tabela de rotas 
-	Selecionar a opção correspondente à VPC criada anteriormente
-	Clicar no botão **“criar tabela de rotas”** para confirmar
-	Selecionar a tabela de rotas criada
-	Clicar em **"Ações"** > **"Editar rotas"**
-	Clicar em **"Adicionar rota"**
-	Configurar da seguinte forma:
    - Destino: 0.0.0.0/0
    - Alvo: Selecionar o gateway de internet criado anteriormente
-	Clicar em **"Salvar alterações"**.

**Gateway de internet**
-	No painel VPC da AWS, clicar em **“Gateways da internet”** no menu lateral esquerdo
-	Em seguida, clicar no botão **“criar gateway da internet”**
-	Na nova janela, criar o nome do gateway
-	 Clicar no botão **“criar gateway da internet”** para confirmar
-	Selecionar o internet gateway criado
-	Clicar em **"Ações"** > **"Associar à VPC"**
---
## Resolução da Atividade AWS

### Gerar uma chave pública de acesso na AWS e anexá-la à uma nova instância EC2.

-	No painel EC2 da AWS, clicar em **"Pares de chaves"** no menu lateral esquerdo
-	Em seguida, clicar em **"Criar par de chaves"**
-	indicar um nome para a chave 
-	selecionar o formato do arquivo e clicar em **"Criar par de chaves"**
-	salvar o arquivo gerado.

### Criar 1 instância EC2 com o sistema operacional Amazon Linux 2 (Família t3.small, 16 GB SSD) e configurar as regras de segurança

- No painel EC2 da AWS, clicar em **"Instâncias"** no menu lateral esquerdo
-	Em seguida, clicar no botão **"Executar instâncias"**
-	Configurar as Tags (Name, Project e CostCenter) para instâncias e volumes
-	Selecionar a imagem Amazon Linux 2 AMI (HVM) – Kernel 5.10, SSD Volume Type
-	Selecionar o tipo de instância t3.small
-	Escolher a chave gerada anteriormente
-	No item **“configurações de rede”**, clicar em **"Editar"**
-	Selecionar a rede e sub-rede criadas anteriormente
-	Habilitar a opção **“atribuir IP público automaticamente”**
-	Selecionar a opção **“criar grupo de segurança”** e nomeá-lo
-	Editar as regras de entrada da seguinte forma:

    Tipo | Protocolo | Intervalo de portas | Origem | 
    ---|---|---|---|
    SSH | TCP | 22 | 0.0.0.0/0 | 
    TCP personalizado | TCP | 80 | 0.0.0.0/0 | 
    TCP personalizado | TCP | 443 | 0.0.0.0/0 |
    TCP personalizado | TCP | 111 | 0.0.0.0/0 |
    UDP personalizado | UDP | 111 | 0.0.0.0/0 | 
    TCP personalizado | TCP | 2049 | 0.0.0.0/0 |
    UDP personalizado | UDP | 2049 | 0.0.0.0/0 | 

-	Alocar **16 GB** de armazenamento gp2 (SSD)
-	Clicar em **"Executar instância"**.

### Alocar um endereço IP elástico à instância EC2.
	
- No painel EC2, clicar em **"IPs elásticos"** no menu lateral esquerdo
-	Clicar em **"Alocar endereço IP elástico"**
-	Selecionar o IP alocado e clicar em **"Ações"** > **"Associar endereço IP elástico"**
-	Selecionar a instância EC2 criada anteriormente e clicar em **"Associar"**.

---
## Resolução da Atividade Linux

### Configurar o NFS e criar um diretório dentro do filesystem com o seu nome

- Atualizar o sistema `sudo yum update -y`
- Instalei o NFS com `sudo yum install nfs-utils -y`
- Usar o comando `sudo mkdir -p /home/decio/server-nfs`para criar o diretório com o nome do usuário  
- Configurar as permissões do diretório com `sudo chown -R ec2-user:ec2-user /home/decio/server-nfs/` e `sudo chmod 755 /home/decio/server-nfs/`
- Editar o arquivo de configuração **/etc/exports** com `sudo nano /etc/exports`
- Dentro do arquivo **/etc/exports**, digitar `/home/decio/server-nfs/ *(rw,sync,no_subtree_check)` para permitir acesso a todos os clientes
- Salvar as alterações `ctrl + O`, `enter` e fechar o arquivo `ctrl + X`
- Aplicar as mudanças com `sudo exportfs -a`
- Iniciar o NFS com `sudo systemctl start nfs-server`
- Habilitar o NFS com `sudo systemctl enable nfs-server`
- Verificar o status do NFS com `sudo systemctl status nfs-server`

### Subir um Apache no servidor - o Apache deve estar online e rodando

- Instalar o Apache com `sudo yum install httpd -y`
- Iniciar o Apache com `sudo systemctl start httpd`
- Habilitar o Apache com `sudo systemctl enable httpd`
- Verifiquei o status do Apache com `sudo systemctl status httpd`
- Em um browser, digitar o IP público da instância EC2 para ver a página padrão do Apache.


### Criar um script de validação que:  

**1 - verifique se o serviço esta online e envie o resultado da validação para o seu diretorio no NFS**

**2 - contenha Data HORA + nome do serviço + Status + mensagem personalizada de online ou offline**

- Navegar até o diretório NFS com `cd /home/decio/server-nfs/`
- Usar o editor nano para criar o script (extensão .sh). Atribuir o nome desejado: `nano check_services.sh`
- Digitar o seguinte script no arquivo:

  ```bash
  #!/bin/bash

  # Diretório para armazenar os resultados
  output_dir="/home/decio/server-nfs/"
  timestamp=$(date "+%Y-%m-%d %H:%M:%S")

  # Função para verificar o status do serviço
  check_service_status() {
    service_name=$1
    service_status=$(sudo systemctl is-active $service_name)

    if [ "$service_status" == "active" ]; then
        echo "$timestamp - $service_name - ONLINE - O serviço está ativo." >> "${output_dir}${service_name}_online.log"
    else
        echo "$timestamp - $service_name - OFFLINE - O serviço está inativo." >> "${output_dir}${service_name}_offline.log"
    fi
    }
  # Verificar status do NFS
  check_service_status "nfs-server"

  # Verificar status do Apache
  check_service_status "httpd" 
  ```
- Salvar as alterações `ctrl + O`, `enter` e fechar o arquivo `ctrl + X`
- Tornar o scrip executável com `chmod +x check_services.sh`
  
### O script deve gerar 2 arquivos de saida: 1 para o serviço online e 1 para o serviço offline;

 - Os logs gerados do script são chamados **nfs-server_online.log**, **nfs-server_offline.log**, **httpd_online.log** e **httpd_offline.log** e estão armazenados no diretório `/home/decio/server-nfs/` Neles estão contidas as informações solicitadas.

### Preparar a execução automatizada do script a cada 5 minutos.

- Editar o crontab com `crontab -e`
- No arquivo, adicionar o seguinte texto para agendar a execução a cada 5 minutos: `*/5 * * * * /home/decio/server-nfs/check_services.sh`


















