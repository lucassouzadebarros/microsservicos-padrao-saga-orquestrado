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
- Maven

## Instalação
1. Clone o repositório:
    ```bash
    git clone https://github.com/lucassouzadebarros/hexagonal-architecture.git    
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

  
  
