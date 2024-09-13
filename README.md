# padrao-saga-orquestrado
Microsserviços Padrão Saga Orquestrado

# Microsserviços Padrão Saga Orquestrado

![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=java&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/postgresql-4169E1?style=for-the-badge&logo=postgresql&logoColor=blue&color=%23f6f7f8)
![MongoDB](https://img.shields.io/badge/-MongoDB-13aa52?style=for-the-badge&logo=mongodb&logoColor=white)
![Docker](https://img.shields.io/badge/docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Apache Kafka](https://img.shields.io/badge/Apache_Kafka-231F20?style=for-the-badge&logo=apache-kafka&logoColor=white)

## Índice
- [Sobre o Projeto](#sobre-o-projeto)
- [Arquitetura Proposta](#arquitetura-proposta)
- [Descrição](#Descrição)
- [Objetivos](#Objetivo)
- [Instalação](#instalação)
- [Acessando a aplicação](#acessando-a-aplicação)
- [Acessando tópicos com Redpanda Console](#acessando-tópicos-com-redpanda-console)
- [Dados da API](#dados-da-api)
- [Produtos registrados e seu estoque](#produtos-registrados-e-seu-estoque)
- [Endpoint para visualizar a saga](#endpoint-para-visualizar-a-saga)



## Sobre o Projeto
   O padrão Saga orquestrado é uma abordagem onde um controlador central coordena as transações dos serviços envolvidos na saga. Esse controlador diz o que cada serviço deve fazer, gerenciando a sequência de processos e 
   mantendo a consistência dos dados em ambientes de microserviços.


## Arquitetura Proposta
![image](https://github.com/user-attachments/assets/ed6ec801-d927-4080-ae85-4d78d83ff560)


## Descrição
- Order-Service: microsserviço responsável apenas por gerar um pedido inicial, e receber uma notificação. Aqui que teremos endpoints REST para inciar o processo e recuperar os dados dos eventos. O banco de dados utilizado 
  será o MongoDB.
- Orchestrator-Service: microsserviço responsável por orquestrar todo o fluxo de execução da Saga, ele que saberá qual microsserviço foi executado e em qual estado, e para qual será o próximo microsserviço a ser enviado, 
  este microsserviço também irá salvar o processo dos eventos. Este serviço não possui banco de dados.
- Product-Validation-Service: microsserviço responsável por validar se o produto informado no pedido existe e está válido. Este microsserviço guardará a validação de um produto para o ID de um pedido. O banco de dados 
  utilizado será o PostgreSQL.
- Payment-Service: microsserviço responsável por realizar um pagamento com base nos valores unitários e quantidades informadas no pedido. Este microsserviço guardará a informação de pagamento de um pedido. O banco de 
  dados utilizado será o PostgreSQL.
- Inventory-Service: microsserviço responsável por realizar a baixa do estoque dos produtos de um pedido. Este microsserviço guardará a informação da baixa de um produto para o ID de um pedido. O banco de dados utilizado 
  será o PostgreSQL.  


## Objetivo
- Implementar microsserviços seguindo o padrão saga orquestrado.
- Facilitar a comunicação e o gerenciamento entre microsserviços.
- Integrar com bancos de dados e sistemas de mensageria de maneira eficiente.


## Pré-requisitos
- Java 17 ou superior
- Docker
- Gradle 7.6 ou superior

## Instalação
1. Clone o repositório:
    ```bash
    git clone https://github.com/lucassouzadebarros/microsservicos-padrao-saga-orquestrado.git
    ```
2. Executar com Docker Compose:

- Certifique-se de que você tem o Docker e o Docker Compose instalados.
- Navegue até o diretório do projeto onde o arquivo docker-compose.yml está localizado.
- Execute o comando Docker Compose para subir os serviços utilizados pela as aplicações.

    ```bash
    docker-compose up -d
    ```
- Para parar os containers, use:
     ```bash
    docker-compose down
    ```
     
4. Execute cada aplicação:
    ```bash
    mvn spring-boot:run
    ```  

## Acessando a aplicação

Para acessar as aplicações e realizar um pedido, basta acessar a URL:

http://localhost:3000/swagger-ui.html

Você chegará nesta página:
![image](https://github.com/user-attachments/assets/9272b983-766d-4f97-abeb-8ac7f8c22766)

As aplicações executarão nas seguintes portas:

- Order-Service: 3000
- Orchestrator-Service: 8080
- Product-Validation-Service: 8090
- Payment-Service: 8091
- Inventory-Service: 8092
- Apache Kafka: 9092
- Redpanda Console: 8081
- PostgreSQL (Product-DB): 5432
- PostgreSQL (Payment-DB): 5438
- PostgreSQL (Inventory-DB): 5439
- MongoDB (Order-DB): 27017

## Acessando tópicos com Redpanda Console

Para acessar o Redpanda Console e visualizar tópicos e publicar eventos, basta acessar:

http://localhost:8081

Você chegará nesta página:
![image](https://github.com/user-attachments/assets/85a89ff7-5723-40f4-a786-a8cabed71f14)

## Dados da API

É necessário conhecer o payload de envio ao fluxo da saga, assim como os produtos cadastrados e suas quantidades.

## Produtos registrados e seu estoque

Existem 3 produtos iniciais cadastrados no serviço product-validation-service e suas quantidades disponíveis em inventory-service:

- COMIC_BOOKS (4 em estoque)
- BOOKS (2 em estoque)
- MOVIES (5 em estoque)
- MUSIC (9 em estoque)

## Endpoint para iniciar a saga:

POST http://localhost:3000/api/order

Payload:

 ```bash
{
  "products": [
    {
      "product": {
        "code": "COMIC_BOOKS",
        "unitValue": 15.50
      },
      "quantity": 3
    },
    {
      "product": {
        "code": "BOOKS",
        "unitValue": 9.90
      },
      "quantity": 1
    }
  ]
}
```  

Resposta:

 ```bash
{
  "id": "64429e987a8b646915b3735f",
  "products": [
    {
      "product": {
        "code": "COMIC_BOOKS",
        "unitValue": 15.5
      },
      "quantity": 3
    },
    {
      "product": {
        "code": "BOOKS",
        "unitValue": 9.9
      },
      "quantity": 1
    }
  ],
  "createdAt": "2023-04-21T14:32:56.335943085",
  "transactionId": "1682087576536_99d2ca6c-f074-41a6-92e0-21700148b519"
}
 ```

## Endpoint para visualizar a saga:

É possível recuperar os dados da saga pelo orderId ou pelo transactionId, o resultado será o mesmo:

GET http://localhost:3000/api/event?orderId=64429e987a8b646915b3735f

GET http://localhost:3000/api/event?transactionId=1682087576536_99d2ca6c-f074-41a6-92e0-21700148b519

Resposta:

 ```bash
{
  "id": "64429e9a7a8b646915b37360",
  "transactionId": "1682087576536_99d2ca6c-f074-41a6-92e0-21700148b519",
  "orderId": "64429e987a8b646915b3735f",
  "payload": {
    "id": "64429e987a8b646915b3735f",
    "products": [
      {
        "product": {
          "code": "COMIC_BOOKS",
          "unitValue": 15.5
        },
        "quantity": 3
      },
      {
        "product": {
          "code": "BOOKS",
          "unitValue": 9.9
        },
        "quantity": 1
      }
    ],
    "totalAmount": 56.40,
    "totalItems": 4,
    "createdAt": "2023-04-21T14:32:56.335943085",
    "transactionId": "1682087576536_99d2ca6c-f074-41a6-92e0-21700148b519"
  },
  "source": "ORCHESTRATOR",
  "status": "SUCCESS",
  "eventHistory": [
    {
      "source": "ORCHESTRATOR",
      "status": "SUCCESS",
      "message": "Saga started!",
      "createdAt": "2023-04-21T14:32:56.78770516"
    },
    {
      "source": "PRODUCT_VALIDATION_SERVICE",
      "status": "SUCCESS",
      "message": "Products are validated successfully!",
      "createdAt": "2023-04-21T14:32:57.169378616"
    },
    {
      "source": "PAYMENT_SERVICE",
      "status": "SUCCESS",
      "message": "Payment realized successfully!",
      "createdAt": "2023-04-21T14:32:57.617624655"
    },
    {
      "source": "INVENTORY_SERVICE",
      "status": "SUCCESS",
      "message": "Inventory updated successfully!",
      "createdAt": "2023-04-21T14:32:58.139176809"
    },
    {
      "source": "ORCHESTRATOR",
      "status": "SUCCESS",
      "message": "Saga finished successfully!",
      "createdAt": "2023-04-21T14:32:58.248630293"
    }
  ],
  "createdAt": "2023-04-21T14:32:58.28"
}
 ```


  
