# III Desafio de Projeto - Construindo sua Primeira Stack com CloudFormation

O AWS CloudFormation é um serviço que permite criar, configurar e gerenciar recursos da AWS automaticamente, a partir de modelos (templates) escritos em YAML ou JSON.

Em vez de criar manualmente uma instância EC2, um bucket S3 e uma VPC pelo console, você define tudo em um arquivo — e o CloudFormation faz o resto para você.

## 🧱 Como funciona

**1. Você cria um template**
- Nesse arquivo, você define o que quer construir: EC2, S3, VPC, RDS, IAM, etc.
- Exemplo (YAML simplificado):
```yaml
Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0abcdef1234567890
```

**2. O CloudFormation lê o template**
- Ele interpreta esse código e entende o que precisa ser criado.
  
**3. Cria um Stack (pilha)**
- Um Stack é o conjunto dos recursos criados com base no template.
- Se o template define 1 VPC, 2 subnets e 1 instância EC2, todos esses recursos pertencem ao mesmo Stack.
  
**4. Gerencia automaticamente o ciclo de vida**
- Se você atualizar o template, o CloudFormation atualiza os recursos existentes (sem precisar recriar tudo).
- Se excluir o Stack, ele remove todos os recursos criados por aquele template.

## 🚀 Principais Benefícios do AWS CloudFormation

| **Benefício**                 | **Descrição**                                                                 |
|-------------------------------|-------------------------------------------------------------------------------|
| **Automação**                 | Cria e gerencia recursos sem precisar fazer manualmente.                     |
| **Infraestrutura como Código (IaC)** | Tudo é versionável e reproduzível, como código de software.              |
| **Reprodutibilidade**         | Permite recriar ambientes idênticos (ex.: produção, teste e desenvolvimento). |
| **Controle de alterações**    | É possível revisar e aprovar mudanças antes de aplicá-las.                   |
| **Rollback automático**       | Se algo falhar, o CloudFormation reverte as alterações automaticamente.      |
| **Integração**                | Funciona com serviços como IAM, EC2, RDS, ECS, Lambda e muito mais.          |


## 📘 Template YAML – Exemplo CloudFormation
```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: >
  Exemplo simples de infraestrutura AWS criada com CloudFormation.
  Cria uma VPC, uma subnet pública, um Security Group e uma instância EC2.

Parameters:
  KeyName:
    Description: Nome do par de chaves EC2 existente para acesso SSH
    Type: AWS::EC2::KeyPair::KeyName

Resources:
  # ----------------------------
  # 1️⃣ Criação da VPC
  # ----------------------------
  MyVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      EnableDnsSupport: true
      EnableDnsHostnames: true
      Tags:
        - Key: Name
          Value: MinhaVPC

  # ----------------------------
  # 2️⃣ Criação da Subnet Pública
  # ----------------------------
  PublicSubnet:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref MyVPC
      CidrBlock: 10.0.1.0/24
      MapPublicIpOnLaunch: true
      AvailabilityZone: !Select [0, !GetAZs '']
      Tags:
        - Key: Name
          Value: SubnetPublica

  # ----------------------------
  # 3️⃣ Criação do Security Group
  # ----------------------------
  MySecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Permite SSH e HTTP
      VpcId: !Ref MyVPC
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 0.0.0.0/0   # Permite SSH de qualquer lugar (use com cautela)
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0   # Permite acesso HTTP
      Tags:
        - Key: Name
          Value: SG-Publico

  # ----------------------------
  # 4️⃣ Criação da Instância EC2
  # ----------------------------
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0c55b159cbfafe1f0   # Exemplo (Amazon Linux 2 na região us-east-1)
      KeyName: !Ref KeyName
      NetworkInterfaces:
        - AssociatePublicIpAddress: true
          DeviceIndex: 0
          SubnetId: !Ref PublicSubnet
          GroupSet:
            - !Ref MySecurityGroup
      Tags:
        - Key: Name
          Value: MinhaInstanciaEC2

Outputs:
  InstancePublicIP:
    Description: Endereço IP público da instância
    Value: !GetAtt MyEC2Instance.PublicIp

  VPCId:
    Description: ID da VPC criada
    Value: !Ref MyVPC
```

## ⚙️ Como usar esse template
Acesse o AWS Management Console.
1. Vá em CloudFormation → Create stack → With new resources (standard).
2. Faça upload do arquivo .yaml.
3. Clique em Next, escolha o par de chaves (KeyName) e crie o stack.
4. Após o deploy, vá em Outputs para ver o IP público da EC2 e testar o acesso.

## 💡 Dica prática

Esse tipo de template é ótimo para:

- Aprender a organizar dependências entre recursos.
- Praticar Infraestrutura como Código (IaC).
- Automatizar a criação de ambientes de teste.
  
## ✨Referências  
- [Documentação oficial do AWS CloudFormation](https://docs.aws.amazon.com/cloudformation/index.html)  
- [Guia de introdução rápida](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/GettingStarted.html)
