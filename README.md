# Desafio: Processamento de Notas Fiscais com Localstack e AWS Serverless

Este projeto implementa um pipeline de backend serverless para o processamento de notas fiscais, utilizando Localstack para simular o ambiente da AWS localmente. A solução é projetada para receber arquivos JSON de notas fiscais, processá-los e armazená-los em um banco de dados NoSQL.

## Visão Geral do Projeto

O objetivo principal era construir e testar uma arquitetura orientada a eventos, onde o upload de um arquivo em um bucket S3 dispara automaticamente o processamento e armazenamento dos dados. O uso do Localstack foi fundamental para permitir o desenvolvimento e teste de toda a infraestrutura na nuvem sem custos e com um ciclo de feedback rápido.

## Arquitetura da Solução

O fluxo de dados principal segue o seguinte caminho:

1.  **Upload**: Um arquivo `.json` contendo uma ou mais notas fiscais é enviado para um bucket no S3.
2.  **Trigger**: O S3 detecta a criação do novo objeto e aciona um evento de notificação.
3.  **Processamento**: Uma função AWS Lambda é invocada pelo evento do S3, recebendo os detalhes do arquivo.
4.  **Armazenamento**: A função Lambda lê o conteúdo do arquivo `.json`, processa os dados de cada nota fiscal e os insere em uma tabela no DynamoDB.

**Fluxo:**
<img width="863" height="643" alt="image" src="https://github.c" />

## Tecnologias Utilizadas

* **Localstack**: Para simular os serviços da AWS localmente.
* **Docker**: Para executar o container do Localstack.
* **AWS CLI**: Para interagir e provisionar os recursos na nuvem (simulada).
* **Python 3**: Linguagem de programação da função Lambda.
* **Serviços AWS (simulados)**:
    * **S3 (Simple Storage Service)**: Para armazenamento de objetos (arquivos `.json`).
    * **Lambda**: Para computação serverless e execução do código de processamento.
    * **DynamoDB**: Como banco de dados NoSQL para armazenar os dados das notas.
    * **(Opcional) API Gateway**: Para expor a função Lambda através de um endpoint HTTP.

## Como Executar o Projeto

Siga os passos abaixo para configurar e testar a solução em seu ambiente local.

### Pré-requisitos

* Docker instalado e em execução.
* AWS CLI instalado.
* Python 3 instalado.

### 1. Iniciar o Localstack

Execute o container do Localstack, que irá expor os serviços da AWS na porta `4566`.

```bash
docker run --rm -it -p 4566:4566 -p 4560-4590:4560-4590 -e DEBUG=1 localstack/localstack
