# Axia Web API - Execução com Docker Compose

Este repositório contém a solução Axia, um projeto .NET que utiliza uma arquitetura limpa (Application, Domain, Infrastructure) e expõe uma API através do projeto `Axia.WebApi`.

O projeto está configurado para ser executado facilmente utilizando o Docker Compose, garantindo um ambiente isolado e consistente.

## 🚀 Pré-requisitos

Para executar este projeto, você precisa ter o seguinte software instalado em sua máquina:

| Software | Versão Mínima Recomendada |
| :--- | :--- |
| **Docker** | 20.10+ |
| **Docker Compose** | 1.29+ (ou a versão integrada `docker compose`) |

## 📁 Estrutura do Projeto

A solução segue uma estrutura de pastas padrão para projetos .NET, com o código-fonte principal dentro da pasta `src/`.

```
.
├── src/
│   ├── Axia.Application/
│   ├── Axia.Domain/
│   ├── Axia.Infrastructure/
│   └── Axia.WebApi/  <-- Projeto principal e Dockerfile
├── Axia.slnx
├── docker-compose.yml  <-- Arquivo de orquestração
└── README.md
```

## ⚙️ Execução do Projeto

Siga os passos abaixo para construir a imagem e iniciar o contêiner da API.

### 1. Construir e Iniciar o Serviço

Na raiz do projeto (onde se encontra o arquivo `docker-compose.yml`), execute o seguinte comando. O argumento `--build` garante que a imagem Docker será construída antes de iniciar o contêiner.

```bash
docker compose up --build
```

> **Nota:** Se você já construiu a imagem e não fez alterações no código ou no `Dockerfile`, pode omitir o `--build` para iniciar mais rapidamente: `docker compose up`.

### 2. Parar o Serviço

Para parar e remover o contêiner, a rede e os volumes (se houver), execute:

```bash
docker compose down
```

## 🔗 Acesso à API (Swagger UI)

A aplicação `Axia.WebApi` está configurada para rodar no ambiente de **Desenvolvimento** (`ASPNETCORE_ENVIRONMENT=Development`) e expõe a porta `8080` do contêiner para a porta `8080` do seu host.

O Swagger UI, a interface de documentação da API, estará acessível no seguinte endereço:

[http://localhost:8080/swagger](http://localhost:8080/swagger)

## 📝 Observações Técnicas

O arquivo `docker-compose.yml` configura o serviço `axia-webapi` com as seguintes especificações:

| Configuração | Valor | Descrição |
| :--- | :--- | :--- |
| **Contexto de Build** | `.` (Raiz do Projeto) | Permite que o Docker acesse o `.slnx` e todos os projetos. |
| **Dockerfile** | `src/Axia.WebApi/Dockerfile` | Caminho para o Dockerfile que contém o *multi-stage build*. |
| **Mapeamento de Portas** | `8080:8080` | Mapeia a porta 8080 do host para a porta 8080 do contêiner. |
| **Ambiente** | `Development` | Garante que o Swagger UI e as mensagens de erro detalhadas estejam ativas. |
