# Guia de Estudo – Amazon Web Services (AWS)

A **Amazon Web Services (AWS)** é a plataforma de computação em nuvem da Amazon, lançada em 2006.  
Ela disponibiliza serviços sob demanda, acessíveis pela internet, permitindo soluções **escaláveis**, **seguras** e de **alta disponibilidade**.

## Princípios essenciais da nuvem na AWS
- **Elasticidade**: ajuste automático de recursos conforme a demanda.  
- **Escalabilidade**: expansão planejada para suportar mais usuários e cargas de trabalho.  
- **Alta disponibilidade**: infraestrutura redundante em várias regiões e zonas.  
- **Responsabilidade compartilhada**:  
  - AWS → segurança da infraestrutura.  
  - Cliente → configuração, dados e permissões.  

---

## Regiões e Zonas de Disponibilidade
- **Região AWS:** um conjunto de data centers localizados em uma área geográfica específica (ex.: `us-east-1` na Virgínia, EUA).  
- **Zona de Disponibilidade (AZ):** cada região é composta por **duas ou mais AZs**, que são data centers fisicamente separados, mas interconectados.  

Isso garante **alta disponibilidade** e **tolerância a falhas**.  
Exemplo: Se uma AZ falhar, outra assume a carga automaticamente.

---

## Formas de Acesso à AWS
- **Console AWS:** interface gráfica via navegador.  
- **AWS CLI:** acesso via linha de comando, útil para automação.  
- **CloudShell:** terminal integrado no navegador, sem precisar instalar CLI localmente.

---

## Modelos de Serviço em Nuvem
- **IaaS (Infrastructure as a Service):** você gerencia VMs, rede e armazenamento (ex.: EC2).  
- **PaaS (Platform as a Service):** você só se preocupa com o código; a AWS cuida da infraestrutura (ex.: Elastic Beanstalk).  
- **SaaS (Software as a Service):** você apenas utiliza o software pronto (ex.: Gmail, Salesforce).

---

## Modelos de Custo
A AWS adota o modelo de **pagamento conforme o uso (pay-as-you-go)**.  

### Principais modalidades
- **Sob demanda (On-Demand)** → paga pelo tempo de uso.  
- **Instâncias reservadas** → contrato de 1 a 3 anos, com descontos.  
- **Savings Plans** → planos flexíveis para workloads previsíveis.  
- **Spot Instances** → até 90% de desconto, mas sujeitas à interrupção.  
- **Free Tier** → camada gratuita para testes (ex.: 750h/mês de EC2 t2.micro por 12 meses).  

---

## Escalar Recursos Verticalmente x Horizontalmente
- **Escalabilidade Vertical (Scale Up):**  
  - Aumentar os recursos de uma única instância (mais CPU, RAM, disco).  
  - Simples de aplicar, mas limitado pela capacidade máxima do hardware.  

- **Escalabilidade Horizontal (Scale Out):**  
  - Adicionar múltiplas instâncias para dividir a carga.  
  - Proporciona **alta disponibilidade e resiliência**.  
  - É a estratégia mais usada em ambientes de nuvem.  

Exemplo prático:  
- Vertical: trocar uma instância **t2.micro** por uma **t3.large**.  
- Horizontal: manter várias instâncias **t2.micro** atrás de um **Load Balancer**.

## Principais Serviços

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

#### Classes de Armazenamento do Amazon S3

O Amazon S3 oferece diferentes **classes de armazenamento** para equilibrar custo e desempenho, de acordo com o perfil de uso dos dados. Cada classe possui características específicas em termos de **disponibilidade, durabilidade, custo e tempo de acesso**.

### S3 Standard
- **Uso**: Dados acessados com frequência.  
- **Durabilidade**: 99,999999999% (11 9’s).  
- **Disponibilidade**: 99,99%.  
- **Custo**: Mais alto em relação às demais classes.  
- **Cenário**: Aplicações web, dados críticos de produção, conteúdos frequentemente acessados.

---

### S3 Standard-IA (Infrequent Access)
- **Uso**: Dados acessados esporadicamente, mas que precisam de rápido acesso quando necessários.  
- **Durabilidade**: 99,999999999%.  
- **Disponibilidade**: 99,9%.  
- **Custo**: Mais barato que o Standard, mas cobra taxa de recuperação por GB.  
- **Cenário**: Backups, logs, dados para recuperação de desastres.

---

### S3 One Zone-IA
- **Uso**: Dados de acesso esporádico, mas armazenados em apenas uma zona de disponibilidade (AZ).  
- **Durabilidade**: 99,999999999%, mas dependente de uma única AZ.  
- **Disponibilidade**: 99,5%.  
- **Custo**: 20% mais barato que o Standard-IA.  
- **Cenário**: Dados que podem ser recriados facilmente ou que não exigem replicação multi-AZ.

---

### S3 Intelligent-Tiering
- **Uso**: Dados com padrões de acesso imprevisíveis.  
- **Funcionamento**: Move automaticamente os objetos entre camadas (frequente e infrequente) com base no uso.  
- **Durabilidade**: 99,999999999%.  
- **Disponibilidade**: 99,9%.  
- **Custo**: Inclui pequena taxa de monitoramento.  
- **Cenário**: Quando não se sabe a frequência de acesso dos dados.

---

### S3 Glacier Instant Retrieval
- **Uso**: Arquivamento com necessidade de acesso imediato, mas pouco frequente.  
- **Durabilidade**: 99,999999999%.  
- **Disponibilidade**: 99,9%.  
- **Custo**: Muito baixo para armazenamento, cobra recuperação.  
- **Cenário**: Arquivos médicos, dados de mídia, backups regulatórios.

---

### S3 Glacier Flexible Retrieval
- **Uso**: Arquivamento de longo prazo, com acesso eventual e recuperação de minutos a horas.  
- **Durabilidade**: 99,999999999%.  
- **Disponibilidade**: 99,99%.  
- **Custo**: Mais barato que Glacier Instant.  
- **Cenário**: Dados históricos, arquivos raramente acessados.  

---

### S3 Glacier Deep Archive
- **Uso**: Arquivamento de longo prazo e mais barato da AWS, acesso raríssimo (horas para recuperar).  
- **Durabilidade**: 99,999999999%.  
- **Disponibilidade**: 99,99%.  
- **Custo**: Mais baixo de todas as classes.  
- **Cenário**: Arquivos regulatórios, retenção de 7 a 10 anos ou mais.

---

### S3 Reduced Redundancy (descontinuada para novos objetos)
- **Uso**: Dados não críticos, replicados em menos AZs.  
- **Durabilidade**: 99,99% (menor que outras classes).  
- **Status**: **Não recomendado** para novos projetos.

---
# Diagrama de Arquitetura AWS

O diagrama representa a arquitetura de um sistema na AWS, mostrando como os serviços interagem para processar e armazenar dados.

## Arquitetura Serverless mínima — S3 + Lambda

![Desafio AWS](https://github.com/user-attachments/assets/42fce601-0637-4bcc-9f0e-0da853d27599)

### Fluxo resumido

1. Usuário faz upload no S3.
2. Evento ObjectCreated dispara a Lambda.
3. Lambda processa e grava resultado em S3.


## Arquitetura Simples com VM — EC2 + EBS + S3
   
![Desafio AWS (1)](https://github.com/user-attachments/assets/cf46337a-4a8a-4a49-bf1b-d14222a7581c)

### Fluxo resumido

1. Usuário envia arquivo ao S3 (ou diretamente para a EC2).
2. EC2 baixa do S3, processa usando EBS para armazenamento de bloco.
3. EC2 salva resultados no S3 (opcional)

---

## Conclusão
A AWS é a principal plataforma de computação em nuvem, oferecendo **flexibilidade, escalabilidade e segurança**.  
O domínio dos serviços **EC2, EBS e S3** é essencial para projetos práticos.  
Compreender o **modelo de custos**, as boas práticas de **arquitetura distribuída** e os mecanismos de **alta disponibilidade** garante o uso eficiente da plataforma em ambientes reais.
