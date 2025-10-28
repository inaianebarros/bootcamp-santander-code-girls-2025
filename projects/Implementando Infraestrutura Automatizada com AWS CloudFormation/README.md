# IV Desafio de Projeto - Implementando Infraestrutura Automatizada com AWS CloudFormation 

## 📘 Descrição do Desafio  
Este laboratório tem como objetivo **implementar uma infraestrutura automatizada com o AWS CloudFormation**.  
O entregável é um **repositório organizado** contendo anotações e insights adquiridos durante a prática, servindo como **material de apoio** para estudos e futuras implementações em projetos reais.

---

## 🎯 Objetivos  

- Aplicar os conceitos aprendidos em um ambiente prático;  
- Documentar processos técnicos de forma clara e estruturada;  
- Utilizar o **GitHub** como ferramenta para compartilhamento de documentação técnica e versionamento de código IaC (Infrastructure as Code).  

---

## ☁️ O que é o AWS CloudFormation  

O **AWS CloudFormation** é um serviço que permite **modelar, provisionar e gerenciar recursos da AWS de forma automatizada** por meio de arquivos de texto no formato **YAML ou JSON**.  
Com ele, é possível criar toda a infraestrutura de um ambiente (como VPCs, EC2, S3, RDS, IAM etc.) de maneira **reprodutível, versionável e segura**, evitando configurações manuais.

---

## ⚙️ Principais Características  

✅ **Infraestrutura como Código (IaC):** define recursos e suas dependências em templates versionáveis.  
✅ **Automação:** reduz erros manuais e garante consistência em diferentes ambientes (dev, stage, prod).  
✅ **Controle de versão:** templates podem ser armazenados em repositórios Git.  
✅ **Integração nativa com AWS:** compatível com a maioria dos serviços AWS.  
✅ **Reversão automática (Rollback):** em caso de falha, o CloudFormation reverte as alterações.  
✅ **Stacks e Templates:** agrupa recursos em *stacks* (pilhas) criadas a partir de *templates*.  

---

## 🧩 Estrutura Básica de um Template CloudFormation  

Um template CloudFormation é composto por **seções principais**. Nem todas são obrigatórias, mas seguem uma estrutura recomendada:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Template exemplo de uma instância EC2
Parameters:
  InstanceType:
    Description: Tipo da instância EC2
    Type: String
    Default: t2.micro

Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: !Ref InstanceType
      ImageId: ami-0c55b159cbfafe1f0

Outputs:
  InstanceId:
    Description: ID da instância criada
    Value: !Ref MyEC2Instance
```
## 🧠 Exemplo em JSON

```json
{
  "AWSTemplateFormatVersion": "2010-09-09",
  "Description": "Criação de bucket S3",
  "Resources": {
    "MyS3Bucket": {
      "Type": "AWS::S3::Bucket",
      "Properties": {
        "BucketName": "meu-bucket-exemplo-cloudformation"
      }
    }
  },
  "Outputs": {
    "BucketName": {
      "Description": "Nome do bucket criado",
      "Value": { "Ref": "MyS3Bucket" }
    }
  }
}
```
## 🔁 Fluxo de Funcionamento do CloudFormation

1. Criação do Template: você define os recursos em YAML ou JSON.

2. Upload do Template: o arquivo é carregado no CloudFormation (via Console, CLI ou API).

3. Criação da Stack: o CloudFormation interpreta o template e cria os recursos na ordem correta.

4. Monitoramento: é possível acompanhar o progresso e logs no Console ou CloudWatch.

5. Atualização e Exclusão: modificações podem ser aplicadas via atualização da stack, mantendo controle de versões.

## 🆚 CloudFormation vs Terraform

| **Característica**        | **AWS CloudFormation**                             | **Terraform (HashiCorp)**                           |
|----------------------------|----------------------------------------------------|-----------------------------------------------------|
| **Provedor**               | Exclusivo da AWS                                  | Multicloud (AWS, Azure, GCP, etc.)                 |
| **Linguagem**              | YAML / JSON                                       | HCL (HashiCorp Configuration Language)             |
| **Controle de Estado**     | Gerenciado automaticamente pela AWS               | Requer arquivo `terraform.tfstate`                 |
| **Integração com AWS**     | Nativa e profunda                                 | Por meio de provedores externos                    |
| **Custo**                  | Gratuito (paga-se apenas pelos recursos criados)  | Gratuito                                            |
| **Execução Local**         | Não (rodando na AWS)                              | Sim                                                 |
| **Rollback automático**    | Sim                                               | Parcial                                             |

*💡 Ambos seguem o conceito de IaC, mas o Terraform é mais flexível para ambientes híbridos/multicloud, enquanto o CloudFormation é mais integrado e otimizado para o ecossistema AWS.*

## 💬 Conclusão

Este laboratório permite compreender na prática o poder da automação e padronização na criação de infraestruturas em nuvem.
Ao dominar o CloudFormation, você se torna capaz de gerenciar ambientes complexos com eficiência, garantindo segurança, reprodutibilidade e rastreabilidade no ciclo de vida da infraestrutura.

##  Referências

- [Documentação oficial do AWS CloudFormation](https://docs.aws.amazon.com/cloudformation/)  
- Getting Started com CloudFormation  
- Vídeos da AWS Brasil no YouTube  
- Aulas práticas do bootcamp DIO
