# I Desafio de Projeto - Gerenciamento de Instâncias EC2 na AWS

Este laboratório tem como objetivo consolidar conhecimentos em gerenciamento de instâncias EC2 na AWS. 

---

## Objetivo
- Aplicar os conceitos aprendidos em um ambiente prático; 
- Documentar processos técnicos de forma clara e estruturada;

---

### Amazon EC2 (Elastic Compute Cloud)
Serviço de **computação em nuvem** para criar e gerenciar máquinas virtuais.  
- **Uso**: hospedagem de sites, aplicações e processamento de dados.  
- **Conceitos-chave**:  
  - *Instâncias*: servidores virtuais.  
  - *AMI*: imagens de sistemas operacionais/software.  
  - *Security Groups*: firewalls de controle de tráfego.  

---

### Amazon EBS (Elastic Block Store)
Serviço de **armazenamento em blocos** para EC2.  
- Funciona como um **HD virtual persistente**.  
- Tipos: **SSD** (alto desempenho) e **HDD** (baixo custo).  
- Permite **snapshots** (backups incrementais).  

---

### Amazon S3 (Simple Storage Service)
Serviço de **armazenamento de objetos** escalável e durável.  
- Armazena arquivos em **buckets**.  
- Durabilidade de 99,999999999% (“11 noves”).  
- **Usos**: arquivos, backups, sites estáticos, data lakes.  

---
# Diagrama de Arquitetura AWS

O diagrama representa a arquitetura de um sistema na AWS, mostrando como os serviços interagem para processar e armazenar dados.


## Arquitetura Serverless mínima — S3 + Lambda

![arquitetura_s3_lambda](https://github.com/user-attachments/assets/9f42d26b-5e79-47eb-8558-74ba8bddaed4)

### Fluxo resumido

1. Usuário faz upload no S3.
2. Evento ObjectCreated dispara a Lambda.
3. Lambda processa e grava resultado em S3.


## Arquitetura Simples com VM — EC2 + EBS + S3
   
![arquitetura_ec2_ebs_s3](https://github.com/user-attachments/assets/66fecc40-c813-4a63-a933-5454e5365cd0)

### Fluxo resumido

1. Usuário envia arquivo ao S3 (ou diretamente para a EC2).
2. EC2 baixa do S3, processa usando EBS para armazenamento de bloco.
3. EC2 salva resultados no S3 (opcional)

---

##  Recursos Adicionais

- [Documentação EC2 AWS](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/concepts.html)  
- [Guia Markdown GitHub](https://guides.github.com/features/mastering-markdown/)  
- [Draw.io para diagramas](https://draw.io)

---

## Conclusão
A AWS é a principal plataforma de computação em nuvem, oferecendo **flexibilidade, escalabilidade e segurança**.  
O domínio dos serviços **EC2, EBS e S3** é essencial para projetos práticos.  
Compreender o **modelo de custos**, as boas práticas de **arquitetura distribuída** e os mecanismos de **alta disponibilidade** garante o uso eficiente da plataforma em ambientes reais.
