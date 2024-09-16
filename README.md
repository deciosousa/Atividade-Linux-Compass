# ATIVIDADE LINUX-COMPASS
Tarefa desenvolvida no âmbito do estágio da Compass-UOL DevSecOps AWS.

## Objetivo da atividade:
Desenvolver um guia prático para criação e configuração de recursos na cloud computing da AWS, atendendo aos requisitos descritos no arquivo **“requisitos.pdf”** deste repositório:

## Configurações Relacionadas ⚙️
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
## Resolução da Atividade

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

