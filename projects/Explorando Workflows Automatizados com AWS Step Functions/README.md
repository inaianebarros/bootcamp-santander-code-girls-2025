# II Desafio de Projeto – Explorando Workflows Automatizados com AWS Step Functions
O AWS Step Functions é um serviço de orquestração visual de fluxos de trabalho (workflows) que permite coordenação de vários serviços da AWS em etapas (steps) definidas por estados.

Em termos simples:
- Ele organiza e automatiza o fluxo entre diferentes tarefas (como funções Lambda, chamadas de API, consultas a bancos de dados, etc.),
- Garante que cada etapa ocorra na ordem certa,
- E lida automaticamente com erros, repetições e decisões condicionais.

## 💡 Analogia simples
Pense no Step Functions como o "diretor de um filme":
- Cada ator (serviço AWS) tem seu papel (tarefa).
- O Step Functions é quem coordena a sequência — quem entra em cena, quem fala, quem espera o outro terminar e o que acontece se alguém errar a fala.

## ⚙️ Como ele funciona
O Step Functions usa um modelo chamado máquina de estados (State Machine) — ou seja, o fluxo de execução é definido como uma série de estados, onde cada estado representa uma etapa da aplicação.

Um estado pode ser:

- Uma tarefa (executa algo, como uma função Lambda ou uma API),
- Uma decisão (escolhe o caminho com base em condições),
- Um paralelo (executa várias coisas ao mesmo tempo),
- Um esperar (aguarda um tempo antes de continuar),
- Um fim (encerra o fluxo).

Tudo isso é configurado usando JSON, com base no formato Amazon States Language (ASL).

## 🧩 Componentes principais
| Componente    | Descrição                                                                                 |
|----------------|-------------------------------------------------------------------------------------------|
| **State Machine** | Define o fluxo completo de execução.                                                   |
| **States**        | Cada etapa do processo (pode ser tarefa, decisão, paralelismo, etc.).                  |
| **Tasks**         | São as ações executadas — por exemplo, uma Lambda function, ou chamada de API.         |
| **Transitions**   | Conectam os estados, determinando a ordem e condições de passagem.                     |
| **Execution**     | Cada vez que o fluxo é iniciado, é criada uma execução independente.                   |

## 🧩 Exemplo de código (simplificado)

```json
{
  "Comment": "Exemplo simples de Step Function",
  "StartAt": "ProcessarPedido",
  "States": {
    "ProcessarPedido": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789:function:ProcessarPedido",
      "Next": "CobrarCliente"
    },
    "CobrarCliente": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789:function:CobrarCliente",
      "End": true
    }
  }
}
```
Esse fluxo executa duas tarefas em sequência — uma Lambda que processa o pedido e outra que cobra o cliente.

## 🧠 Exemplo prático
<img width="2944" height="2036" alt="image" src="https://github.com/user-attachments/assets/fbc83c45-0efa-4a25-b531-93d761bdf2a2" />

Este fluxo representa um *workflow* robusto e resiliente, orquestrado pelo **AWS Step Functions**, para processar pedidos em um sistema de e-commerce. Ele garante a execução correta de cada etapa, além de lidar automaticamente com falhas e desvios de caminho.

### 1. Início e Processamento Inicial

O fluxo é iniciado com o recebimento do pedido.

* **1.1. Receber Pedido (Lambda: Invoke - `Process Order`):** O Step Functions chama uma função **AWS Lambda** (`Process Order`) para realizar validações e tarefas iniciais, formatando a entrada para os próximos passos.

### 2. Verificação de Estoque

A próxima etapa crítica é a verificação da disponibilidade do produto.

* **2.1. Verificar Estoque (DynamoDB: GetItem - `Check Stock Database`):** O Step Functions se integra diretamente ao **Amazon DynamoDB** para consultar o status do estoque do produto.
* **2.2. Decisão de Estoque (Choice State - `Choice`):** O *workflow* usa um estado `Choice` para avaliar o resultado da consulta ao estoque:
    * **Caminho Padrão (Default - Estoque Indisponível):** Se o item não estiver em estoque, o fluxo desvia para **`SQS: SendMessage: SendAlert (1)`**, alertando a equipe de que o pedido não pode ser processado e o fluxo é encerrado.
    * **Rule #1 (Estoque Disponível):** Se o item estiver disponível, o fluxo prossegue para a cobrança.

### 3. Cobrança do Cliente e Verificação de Pagamento

Com o estoque garantido, o foco é na transação financeira.

* **3.1. Cobrar Cliente (API Gateway: Invoke - `Payment Service Integration`):** O Step Functions se integra ao **Amazon API Gateway** para chamar um serviço externo ou interno responsável pelo processamento de pagamentos.
* **3.2. Decisão de Pagamento (Choice State - `Choice (1)`):** Após a tentativa de cobrança, um novo estado `Choice` é usado para verificar o status da transação:
    * **Rule #1 (Falha no Pagamento):** Se o pagamento falhar, o fluxo desvia para **`SQS: SendMessage: SendAlert`**, notificando a falha (possivelmente para a área de Atendimento ao Cliente) e o fluxo é encerrado.
    * **Caminho Padrão (Default - Pagamento Aprovado):** Se o pagamento for bem-sucedido, o processo continua.

### 4. Conclusão do Pedido

As etapas finais de execução da ordem.

* **4.1. Gerar Nota Fiscal (Lambda: Invoke - `Create NF`):** Uma segunda função **AWS Lambda** é invocada para gerar o documento fiscal (Nota Fiscal Eletrônica).
* **4.2. Enviar Confirmação (SQS: SendMessage - `Send Confirmation`):** O Step Functions publica uma mensagem no **Amazon SQS** (`Send Confirmation`). Esta mensagem é, tipicamente, consumida por outro serviço assíncrono que enviará o e-mail de confirmação do pedido ao cliente.

### 5. Fim

* O fluxo é concluído no estado **End**, após a confirmação do pedido, ou após o envio de um alerta em caso de falha de estoque ou pagamento.

---

## ⚙️ Recursos do Step Functions Utilizados:

1.  **Orquestração de Tarefas:** Coordenando **Lambda**, **DynamoDB**, **API Gateway** e **SQS**.
2.  **Lógica Condicional:** Uso do **Choice State** para desvios de caminho (estoque e pagamento).
3.  **Resiliência:** O Step Functions lida automaticamente com **retentativas** e **tratamento de erros** (não visíveis no diagrama, mas nativos do serviço), garantindo que, em caso de falha temporária em uma etapa, ela seja repetida.

## 🔗 Integrações nativas

O Step Functions integra-se diretamente com mais de 200 serviços da AWS, incluindo:

- AWS Lambda
- Amazon ECS / EKS / Batch
- DynamoDB, S3, SNS, SQS, RDS
- Glue, SageMaker, API Gateway
- EventBridge e CloudWatch

Essas integrações permitem criar pipelines completos sem precisar escrever código de orquestração manualmente.

## Fontes   
- [AWS Step Functions – Documentação Oficial](https://docs.aws.amazon.com/step-functions/)  
- [Guia de Início Rápido AWS Step Functions](https://aws.amazon.com/pt/step-functions/getting-started/) 
