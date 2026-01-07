# 🎬 Roteiro Completo de Apresentação - Fase 4 (Nota Máxima)

## 📋 DURAÇÃO SUGERIDA: 12-15 minutos

---

## PARTE 1: Introdução e Visão Geral (1-2 minutos)

### 🎯 O que falar:

> "Olá, somos o Grupo 97 e vamos apresentar a **Fase 4** do Tech Challenge. Nesta fase, refatoramos nossa aplicação monolítica em uma **arquitetura de microserviços**, dividida em 3 serviços independentes: Order, Payment e Production."

### 📺 O que mostrar:

1. **Abrir o README principal** (`techchallenge_fase4`)
   - Mostrar a tabela com os 3 microserviços
   - Explicar que cada um tem seu próprio repositório

2. **Destacar os links importantes:**
   - Repositórios GitHub (privados)
   - SonarQube Dashboards
   - GitHub Actions (CI/CD)

---

## PARTE 2: Arquitetura de Microserviços (2-3 minutos)

### 🎯 O que falar:

> "Dividimos a aplicação em **3 microserviços independentes**, cada um com responsabilidades específicas:"

#### Microserviços:

1. **Order Service**: 
   - Gerencia clientes, produtos e pedidos
   - Banco: **SQL Server** (consistência transacional)

2. **Payment Service**: 
   - Processa pagamentos via MercadoPago
   - Banco: **SQL Server** (garantias ACID para transações financeiras)

3. **Production Service**: 
   - Gerencia a fila de produção da cozinha
   - Banco: **MongoDB** (flexibilidade para documentos)

> "Os serviços se comunicam via **HTTP REST**, respeitando o princípio fundamental de microserviços: **um serviço nunca acessa o banco de dados de outro**."

### 📺 O que mostrar:

1. **Diagrama de Infraestrutura (Fase 4)**
   - Mostrar os 3 serviços no Kubernetes (Minikube)
   - Destacar HPA, ConfigMaps, Secrets, Services
   - Mostrar os bancos de dados separados

2. **Diagrama de Bancos de Dados (Fase 4)**
   - Order Service: SQL Server (5 tabelas)
   - Payment Service: SQL Server (1 tabela)
   - Production Service: MongoDB (coleção de documentos)

3. **Fluxo do Sistema (Cliente)**
   - Mostrar o fluxo completo do pedido

4. **Fluxo Administrativo (ADM)**
   - Mostrar os 4 fluxos administrativos

5. **Código de comunicação HTTP REST**
   - Mostrar `PaymentServiceClient.cs` ou `ProductionServiceClient.cs`
   - Explicar como os serviços se comunicam

---

## PARTE 3: Repositórios Separados e Branch Protection (1-2 minutos)

### 🎯 O que falar:

> "Cada microserviço possui seu **próprio repositório privado no GitHub**:"
> - `fastfood-order-service`
> - `fastfood-payment-service`
> - `fastfood-production-service`

> "Todos os repositórios são **privados** e o usuário **`soat-architecture` foi adicionado como colaborador** para validação."

> "A branch **`main` está protegida** em todos os repositórios. Não é possível fazer commit direto. Todas as alterações devem passar por **Pull Request**."

### 📺 O que mostrar:

1. **Abrir cada repositório no GitHub:**
   - Mostrar que são privados
   - Settings → Collaborators → mostrar `soat-architecture` adicionado

2. **Mostrar Branch Protection:**
   - Settings → Rules → Branch protection rules
   - Mostrar que `main` está protegida
   - Mostrar que PR é obrigatório

3. **Mostrar um Pull Request (se tiver):**
   - Mostrar o fluxo de PR
   - Mostrar que os checks do CI precisam passar

---

## PARTE 4: CI/CD e SonarQube (2-3 minutos)

### 🎯 O que falar:

> "Cada repositório possui **pipelines de CI/CD configuradas com GitHub Actions**:"

#### CI (Continuous Integration):
- **Roda em cada Pull Request**
- Executa **build** da aplicação
- Executa **testes unitários** com cobertura
- Envia análise para **SonarQube Cloud**
- **Bloqueia merge** se houver falhas ou cobertura < 70%

#### Deploy (Continuous Deployment):
- **Roda automaticamente no merge para `main`**
- Faz deploy automático dos microserviços

### 📺 O que mostrar:

1. **GitHub Actions de cada repositório:**
   - Abrir Actions de cada um dos 3 repositórios
   - Mostrar os **checks verdes ✅** (não precisa mostrar execução)
   - Destacar: "Build", "Test", "SonarQube Analysis", "Deploy"

2. **SonarQube Dashboards:**
   - Acessar cada projeto no SonarQube Cloud:
     - Order Service: https://sonarcloud.io/project/overview?id=grupo97fiapsoat_fastfood-order-service
     - Payment Service: https://sonarcloud.io/project/overview?id=grupo97fiapsoat_fastfood-payment-service
     - Production Service: https://sonarcloud.io/project/overview?id=grupo97fiapsoat_fastfood-production-service

3. **Mostrar cobertura de testes:**
   - Order Service: **80.3%** ✅
   - Payment Service: **81.46%** ✅
   - Production Service: **83.75%** ✅
   - **Todos acima do mínimo exigido de 80%!**

4. **Mostrar Quality Gate:**
   - Mostrar que todos estão **PASSED** (verde) ✅
   - Explicar que isso garante cobertura ≥ 70% (requisito do PR)

5. **Evidências de cobertura no README:**
   - Mostrar as seções de cobertura em cada repositório

---

## PARTE 5: Testes e Cobertura (2 minutos)

### 🎯 O que falar:

> "Implementamos **testes unitários** com cobertura acima de **80%** em todos os serviços, superando o mínimo exigido."

> "Também implementamos **testes BDD (Behavior-Driven Development)** usando **SpecFlow** para validar cenários de negócio."

### 📺 O que mostrar:

1. **Estrutura de testes de cada projeto:**
   - Mostrar pasta `FastFood.Order.Tests`, `FastFood.Payment.Tests`, `FastFood.Production.Tests`
   - Mostrar exemplos de testes unitários

2. **Arquivo BDD (SpecFlow):**
   - Mostrar arquivo `.feature` (ex: `CriarPedidoEProcessarPagamento.feature`)
   - Mostrar Step Definitions correspondentes
   - Explicar como funciona BDD

3. **Relatório de cobertura:**
   - Mostrar artifacts no GitHub Actions (HTML reports)
   - Ou mostrar no SonarQube (aba "Measures" → "Coverage")

4. **Executar testes localmente (opcional):**
   ```bash
   # Order Service
   dotnet test src/FastFood.Order.Tests/FastFood.Order.Tests.csproj
   
   # Payment Service
   dotnet test src/src/FastFood.Payment.Tests/FastFood.Payment.Tests.csproj
   
   # Production Service
   dotnet test src/src/FastFood.Production.Tests/FastFood.Production.Tests.csproj
   ```

---

## PARTE 6: Como Rodar o Projeto Localmente (3-4 minutos)

### 🎯 O que falar:

> "Vamos demonstrar como rodar o projeto localmente e testar os microserviços."

### 📺 Passo a passo para rodar:

#### Pré-requisitos:
- .NET 8 SDK instalado
- Docker Desktop (para bancos de dados)
- SQL Server (via Docker)
- MongoDB (via Docker ou local)

#### 1. Clonar os repositórios:

```bash
# Clonar cada repositório
git clone https://github.com/grupo97fiapsoat/fastfood-order-service.git
git clone https://github.com/grupo97fiapsoat/fastfood-payment-service.git
git clone https://github.com/grupo97fiapsoat/fastfood-production-service.git
```

#### 2. Configurar os bancos de dados:

**SQL Server (para Order e Payment):**
```bash
# Rodar SQL Server via Docker
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=FastFood2025!" -p 1433:1433 --name sqlserver -d mcr.microsoft.com/mssql/server:2022-latest

# Aguardar SQL Server inicializar (30 segundos)
```

**MongoDB (para Production):**
```bash
# Rodar MongoDB via Docker
docker run -d -p 27017:27017 --name mongodb mongo:latest
```

#### 3. Configurar connection strings:

**Order Service** (`src/FastFood.Order.Api/appsettings.json`):
```json
{
  "ConnectionStrings": {
    "OrderDbConnection": "Server=localhost,1433;Database=FastFood_Order;User Id=sa;Password=FastFood2025!;TrustServerCertificate=true"
  },
  "PaymentServiceBaseUrl": "http://localhost:5002",
  "ProductionServiceBaseUrl": "http://localhost:5003"
}
```

**Payment Service** (`src/src/FastFood.Payment.Api/appsettings.json`):
```json
{
  "ConnectionStrings": {
    "PaymentDbConnection": "Server=localhost,1433;Database=FastFood_Payment;User Id=sa;Password=FastFood2025!;TrustServerCertificate=true"
  },
  "OrderServiceBaseUrl": "http://localhost:5001"
}
```

**Production Service** (`src/src/FastFood.Production.Api/appsettings.json`):
```json
{
  "MongoDB": {
    "ConnectionString": "mongodb://localhost:27017",
    "DatabaseName": "FastFood_Production"
  },
  "OrderServiceBaseUrl": "http://localhost:5001"
}
```

#### 4. Executar migrations:

**Order Service:**
```bash
cd fastfood-order-service
dotnet ef database update --project src/FastFood.Order.Infrastructure --startup-project src/FastFood.Order.Api
```

**Payment Service:**
```bash
cd fastfood-payment-service
dotnet ef database update --project src/src/FastFood.Payment.Infrastructure --startup-project src/src/FastFood.Payment.Api
```

#### 5. Rodar os serviços:

**Terminal 1 - Order Service (porta 5001):**
```bash
cd fastfood-order-service
dotnet run --project src/FastFood.Order.Api
```

**Terminal 2 - Payment Service (porta 5002):**
```bash
cd fastfood-payment-service
dotnet run --project src/src/FastFood.Payment.Api
```

**Terminal 3 - Production Service (porta 5003):**
```bash
cd fastfood-production-service
dotnet run --project src/src/FastFood.Production.Api
```

#### 6. Verificar se os serviços estão rodando:

- Order Service: http://localhost:5001/swagger
- Payment Service: http://localhost:5002/swagger
- Production Service: http://localhost:5003/swagger

---

## PARTE 7: Testando o Sistema (4-5 minutos)

### 🎯 O que falar:

> "Agora vamos demonstrar o funcionamento completo da aplicação, testando todos os fluxos."

### 📺 Demonstração passo a passo:

#### 1. Cadastrar Cliente (Order Service)

**Endpoint:** `POST http://localhost:5001/api/v1/customers`

**Body:**
```json
{
  "name": "João Silva",
  "email": "joao@email.com",
  "cpf": "12345678909"
}
```

**Mostrar:**
- ✅ Validação de CPF
- ✅ Cliente criado com sucesso
- ✅ ID retornado

#### 2. Cadastrar Produto (Order Service)

**Endpoint:** `POST http://localhost:5001/api/v1/products`

**Body:**
```json
{
  "name": "Hambúrguer Artesanal",
  "description": "Hambúrguer com carne artesanal",
  "category": "Lanche",
  "price": 25.90,
  "images": ["https://example.com/hamburger.jpg"]
}
```

**Mostrar:**
- ✅ Produto criado
- ✅ Categorias disponíveis (Lanche, Acompanhamento, Bebida, Sobremesa)

#### 3. Criar Pedido (Order Service)

**Endpoint:** `POST http://localhost:5001/api/v1/orders`

**Body:**
```json
{
  "customerId": "guid-do-cliente-anterior",
  "items": [
    {
      "productId": "guid-do-produto-anterior",
      "quantity": 2
    }
  ]
}
```

**Mostrar:**
- ✅ Pedido criado com status `Pending`
- ✅ Total calculado automaticamente
- ✅ OrderId retornado

#### 4. Gerar QR Code para Pagamento (Order Service → Payment Service)

**Endpoint:** `POST http://localhost:5001/api/v1/orders/{orderId}/checkout`

**Mostrar:**
- ✅ Comunicação entre Order e Payment Service
- ✅ QR Code gerado e retornado
- ✅ Status do pedido muda para `AwaitingPayment`
- ✅ Payment criado no Payment Service

#### 5. Confirmar Pagamento (Payment Service)

**Endpoint:** `POST http://localhost:5002/api/v1/payments/{paymentId}/confirm`

**Body:**
```json
{
  "paymentId": "payment-id-do-mercadopago"
}
```

**Mostrar:**
- ✅ Pagamento confirmado
- ✅ Status muda para `Approved`
- ✅ Comunicação HTTP REST do Payment → Order Service
- ✅ Status do pedido muda para `Paid`
- ✅ Pedido enviado para Production Service

#### 6. Consultar Pedidos em Produção (Production Service)

**Endpoint:** `GET http://localhost:5003/api/v1/production-orders/in-production`

**Mostrar:**
- ✅ Lista de pedidos em produção
- ✅ Dados do MongoDB
- ✅ Status atual de cada pedido

#### 7. Atualizar Status de Produção (Production Service → Order Service)

**Endpoint:** `PUT http://localhost:5003/api/v1/production-orders/{id}/status`

**Body:**
```json
{
  "status": "InPreparation"
}
```

**Mostrar:**
- ✅ Status atualizado no Production Service (MongoDB)
- ✅ Comunicação HTTP REST do Production → Order Service
- ✅ Status do pedido atualizado no Order Service (SQL Server)
- ✅ Fluxo completo: Received → InPreparation → Ready → Completed

#### 8. Consultar Status do Pedido (Order Service) - Público

**Endpoint:** `GET http://localhost:5001/api/v1/orders/{orderId}/status`

**Mostrar:**
- ✅ Endpoint público (sem autenticação)
- ✅ Status atual do pedido
- ✅ Timeline do pedido

---

## PARTE 8: Processo de Deploy Automatizado (1-2 minutos)

### 🎯 O que falar:

> "O deploy é **totalmente automatizado**. Quando fazemos merge de um Pull Request para a branch `main`, o workflow de deploy é executado automaticamente."

### 📺 O que mostrar:

1. **GitHub Actions - Workflow de Deploy:**
   - Mostrar o workflow `deploy.yml` de cada repositório
   - Explicar os passos:
     - Build da imagem Docker
     - Push para Docker Hub / ECR
     - Deploy no Kubernetes (exemplo)

2. **Mostrar checks verdes ✅:**
   - Não precisa mostrar a execução
   - Apenas mostrar que todos os checks passaram

3. **Aplicação rodando (se disponível):**
   - Mostrar endpoints em produção
   - Mostrar serviços rodando no Kubernetes (opcional)

---

## PARTE 9: Resumo e Conclusão (1 minuto)

### 🎯 O que falar:

> "Em resumo, entregamos:"

**✅ Checklist de Entregáveis:**
- ✅ 3 microserviços independentes (Order, Payment, Production)
- ✅ Bancos de dados separados (SQL Server + MongoDB)
- ✅ Comunicação HTTP REST entre serviços
- ✅ Cobertura de testes acima de **80%** (80.3%, 81.46%, 83.75%)
- ✅ Testes BDD implementados (SpecFlow)
- ✅ 3 repositórios separados e privados
- ✅ Branch protection configurado (PR obrigatório)
- ✅ CI/CD completo com GitHub Actions
- ✅ SonarQube Cloud integrado (cobertura ≥ 70% no PR)
- ✅ Deploy automatizado no merge
- ✅ Usuário `soat-architecture` adicionado como colaborador

> "Obrigado pela atenção!"

---

## 🎯 DICAS PARA NOTA MÁXIMA

### ✅ Seja Objetivo:
- Não enrole, vá direto ao ponto
- Foque no que foi pedido no PDF

### ✅ Mostre Evidências:
- Não apenas fale, **mostre na tela**
- Abra os repositórios, SonarQube, GitHub Actions

### ✅ Destaque o Que Foi Pedido:
- Cobertura >80% ✅
- BDD implementado ✅
- SonarQube ≥70% no PR ✅
- CI/CD completo ✅
- 3 repositórios separados ✅

### ✅ Fluxo Funcionando:
- Demonstre o sistema de ponta a ponta
- Mostre comunicação entre serviços
- Teste todos os cenários principais

### ✅ Checks Verdes:
- Mostre que todas as pipelines passaram ✅
- Não precisa mostrar execução, só os checks finais

### ✅ Organização:
- Siga o roteiro de forma lógica
- Arquitetura → Código → Testes → Deploy

---

## 📝 CHECKLIST ANTES DE GRAVAR O VÍDEO

### Repositórios:
- [ ] Verificar que todos os 3 repositórios estão privados
- [ ] Confirmar que `soat-architecture` está adicionado nos 3
- [ ] Verificar branch protection configurado

### SonarQube:
- [ ] Verificar que todos os 3 projetos estão vinculados ao GitHub
- [ ] Confirmar Quality Gate PASSED em todos
- [ ] Verificar cobertura ≥70% em todos

### GitHub Actions:
- [ ] Verificar que todos os workflows estão com checks verdes ✅
- [ ] Confirmar que CI roda em PRs
- [ ] Confirmar que Deploy roda no merge

### Testes:
- [ ] Verificar que cobertura >80% em todos os serviços
- [ ] Confirmar testes BDD rodando
- [ ] Preparar demonstração dos testes

### Demonstração:
- [ ] Testar o fluxo completo localmente antes de gravar
- [ ] Preparar dados de exemplo (clientes, produtos)
- [ ] Ter Swagger/Postman pronto para demonstração

---

## 🚀 COMANDOS RÁPIDOS PARA DEMONSTRAÇÃO

### Rodar tudo rapidamente (Docker Compose - se disponível):

```bash
# Se houver docker-compose.yml configurado
docker-compose up -d
```

### Verificar logs:

```bash
# Order Service
docker-compose logs -f order-service

# Payment Service
docker-compose logs -f payment-service

# Production Service
docker-compose logs -f production-service
```

### Testar endpoints:

```bash
# Health Check de cada serviço
curl http://localhost:5001/health
curl http://localhost:5002/health
curl http://localhost:5003/health
```

---

**Boa sorte na apresentação! 🚀**

