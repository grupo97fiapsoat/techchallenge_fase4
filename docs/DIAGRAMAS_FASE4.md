# 📊 Diagramas Fase 4 - Arquitetura de Microserviços

## 1. Fluxo do Sistema (Cliente) - Fase 4

### Descrição Textual para Geração da Imagem:

```
INÍCIO (Oval)
  ↓
[Cliente Acessa Sistema] (Retângulo)
  ↓
┌─ Escolhe identificação? (Diamante)
│  ├─ Sim → POST /api/v1/customers (Order Service) → [Cliente Identificado] → SQL Server
│  └─ Não → [Token Anônimo Gerado] → SQL Server
  ↓
[Criar Pedido] POST /api/v1/orders (Order Service)
  ↓
[Selecionar Itens] (Retângulo)
  ├─ Lanche
  ├─ Acompanhamento  
  ├─ Bebida
  └─ Sobremesa
  ↓
SQL Server (Order Service) ← [Pedido Criado]
  ↓
[Checkout] POST /api/v1/orders/{id}/checkout
  ↓
[Payment Service] → [Gerar QR Code Mercado Pago]
  ↓
[Cliente Paga via QR Code]
  ↓
[Webhook Mercado Pago] → POST /webhook/mercadopago (Payment Service)
  ↓
[Confirmar Pagamento] POST /api/v1/payments/{id}/confirm (Payment Service)
  ↓
[Payment Service] → HTTP → [Order Service] (Atualizar Status = Paid)
  ↓
[Payment Service] → HTTP → [Production Service] (Criar Pedido de Produção)
  ↓
MongoDB (Production Service) ← [Pedido em Produção Criado]
  ↓
[Atualizar Status Produção] PUT /api/v1/production-orders/{id}/status (Production Service)
  ├─ Recebido
  ├─ Em Preparação
  ├─ Pronto
  └─ Finalizado
  ↓
[Production Service] → HTTP → [Order Service] (Atualizar Status do Pedido)
  ↓
SQL Server (Order Service) ← [Status Atualizado]
  ↓
[Cliente Consulta Status] GET /api/v1/orders/{id}/status (Order Service)
  ↓
FIM (Oval)
```

**Endpoints Principais:**
- Order Service: `POST /api/v1/orders`, `GET /api/v1/orders/{id}/status`
- Payment Service: `POST /api/v1/payments/checkout`, `POST /api/v1/payments/{id}/confirm`, `POST /webhook/mercadopago`
- Production Service: `POST /api/v1/production-orders`, `PUT /api/v1/production-orders/{id}/status`

**Comunicação entre Serviços:**
- Payment → Order (HTTP REST): Atualizar status de pagamento
- Payment → Production (HTTP REST): Criar pedido de produção
- Production → Order (HTTP REST): Atualizar status do pedido

---

## 2. Fluxo Administrativo (ADM) - Fase 4

### Descrição Textual para Geração da Imagem:

**2.1 - Consultar Lista de Clientes:**
```
INÍCIO
  ↓
[Identificação do ADM]
  ↓
┌─ Perfil encontrado? (Diamante)
│  ├─ Não → POST /api/v1/auth/register (Order Service)
│  └─ Sim → POST /api/v1/auth/login (Order Service)
  ↓
[Consulta Lista de Clientes] GET /api/v1/customers (Order Service)
  ↓
SQL Server (Order Service)
  ↓
[Visualiza Lista de Clientes]
  ↓
FIM
```

**2.2 - Consultar Cliente por CPF:**
```
INÍCIO
  ↓
[Identificação do ADM]
  ↓
┌─ Perfil encontrado? (Diamante)
│  ├─ Não → POST /api/v1/auth/register (Order Service)
│  └─ Sim → POST /api/v1/auth/login (Order Service)
  ↓
[ADM digita CPF do cliente]
  ↓
[Consulta Cliente por CPF] GET /api/v1/customers/cpf/{cpf} (Order Service)
  ↓
SQL Server (Order Service)
  ↓
[Visualiza Dados do Cliente]
  ↓
FIM
```

**2.3 - Cadastrar Novo Produto:**
```
INÍCIO
  ↓
[Identificação do ADM]
  ↓
┌─ Perfil encontrado? (Diamante)
│  ├─ Não → POST /api/v1/auth/register (Order Service)
│  └─ Sim → POST /api/v1/auth/login (Order Service)
  ↓
[ADM digita dados do produto: nome, descrição, categoria, preço, imagem]
  ↓
[Cadastra Novo Produto] POST /api/v1/products (Order Service)
  ↓
SQL Server (Order Service)
  ↓
[Produto Cadastrado]
  ↓
FIM
```

**2.4 - Consultar Pedidos em Produção:**
```
INÍCIO
  ↓
[Identificação do ADM]
  ↓
┌─ Perfil encontrado? (Diamante)
│  ├─ Não → POST /api/v1/auth/register (Production Service)
│  └─ Sim → POST /api/v1/auth/login (Production Service)
  ↓
[Consulta Pedidos em Produção] GET /api/v1/production-orders/in-production (Production Service)
  ↓
MongoDB (Production Service)
  ↓
[Visualiza Fila de Produção]
  ↓
[Atualiza Status] PUT /api/v1/production-orders/{id}/status (Production Service)
  ↓
FIM
```

---

## 3. Diagrama da Infraestrutura - Fase 4

### Descrição Textual para Geração da Imagem:

```
┌─────────────────────────────────────────────────────────────────┐
│                    ARQUITETURA DE MICROSSERVIÇOS                │
│                           (Fase 4)                              │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                      ORDER SERVICE                              │
├─────────────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │  API Layer   │  │ Application  │  │   Domain     │         │
│  │  (Controllers)│  │  (Commands/  │  │  (Entities/  │         │
│  │              │  │   Queries)   │  │  ValueObjs)  │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
│         │                │                │                     │
│         └────────────────┴────────────────┘                     │
│                          │                                      │
│                   ┌──────────────┐                             │
│                   │Infrastructure│                             │
│                   │ (Repositories│                             │
│                   │   /Services) │                             │
│                   └──────────────┘                             │
│                          │                                      │
│                   ┌──────────────┐                             │
│                   │  SQL Server  │                             │
│                   │   (Pedidos,  │                             │
│                   │  Clientes,   │                             │
│                   │  Produtos)   │                             │
│                   └──────────────┘                             │
└─────────────────────────────────────────────────────────────────┘
         │                    │                    │
         │ HTTP REST          │ HTTP REST          │ HTTP REST
         │                    │                    │
┌────────────────────────┐    │    ┌──────────────────────────┐
│   PAYMENT SERVICE      │    │    │   PRODUCTION SERVICE     │
├────────────────────────┤    │    ├──────────────────────────┤
│  ┌──────────────┐     │    │    │  ┌──────────────┐        │
│  │  API Layer   │     │    │    │  │  API Layer   │        │
│  │  (Controllers)│     │    │    │  │  (Controllers)│        │
│  └──────────────┘     │    │    │  └──────────────┘        │
│         │             │    │    │         │                 │
│  ┌──────────────┐     │    │    │  ┌──────────────┐        │
│  │ Application  │     │    │    │  │ Application  │        │
│  │   / Domain   │     │    │    │  │   / Domain   │        │
│  └──────────────┘     │    │    │  └──────────────┘        │
│         │             │    │    │         │                 │
│  ┌──────────────┐     │    │    │  ┌──────────────┐        │
│  │Infrastructure│     │    │    │  │Infrastructure│        │
│  │   (Payment   │     │    │    │  │  (MongoDB    │        │
│  │   Service,   │     │    │    │  │   Repos,     │        │
│  │   HTTP Client)│    │    │    │  │  Notifications│       │
│  └──────────────┘     │    │    │  └──────────────┘        │
│         │             │    │    │         │                 │
│  ┌──────────────┐     │    │    │  ┌──────────────┐        │
│  │  SQL Server  │     │    │    │  │   MongoDB    │        │
│  │  (Pagamentos)│     │    │    │  │  (Pedidos de │        │
│  └──────────────┘     │    │    │  │   Produção)  │        │
│                       │    │    │  └──────────────┘        │
│         │             │    │    │         │                 │
│         └─────────────┴────┴────┴─────────┘                 │
│                    │                │                        │
│               ┌──────────┐    ┌──────────┐                  │
│               │Mercado   │    │Email/SMS │                  │
│               │Pago API  │    │Services  │                  │
│               └──────────┘    └──────────┘                  │
└──────────────────────────────────────────────────────────────┘

LEGENDA:
- ──── = Comunicação HTTP REST entre microserviços
- │ = Conexão com banco de dados ou serviço externo
- Cada serviço possui seu próprio banco de dados
- Cada serviço pode ser deployado independentemente
```

---

## 4. Diagrama do Banco de Dados - Fase 4

### Descrição Textual para Geração da Imagem:

**4.1 - ORDER SERVICE (SQL Server):**
```
┌─────────────────────────────────────────────────────────┐
│              ORDER SERVICE DATABASE                     │
│                  (SQL Server)                           │
└─────────────────────────────────────────────────────────┘

┌──────────────────┐
│   Customers      │
├──────────────────┤
│ id (PK)          │
│ Name             │
│ Email            │
│ Cpf              │
│ CreatedAt        │
│ UpdatedAt        │
└──────────────────┘
         │
         │ 1:N
         │
┌──────────────────┐
│    Orders        │
├──────────────────┤
│ id (PK)          │
│ CustomerId (FK)  │─────┐
│ Status           │     │
│ TotalPrice       │     │
│ QrCode           │     │
│ PreferenceId     │     │
│ CreatedAt        │     │
│ UpdatedAt        │     │
└──────────────────┘     │
         │               │
         │ 1:N           │
         │               │
┌──────────────────┐     │
│  OrderItems      │     │
├──────────────────┤     │
│ id (PK)          │     │
│ OrderId (FK)     │─────┘
│ ProductId (FK)   │──┐
│ ProductName      │  │
│ UnitPrice        │  │
│ Quantity         │  │
└──────────────────┘  │
                      │
                      │ N:1
                      │
┌──────────────────┐  │
│   Products       │  │
├──────────────────┤  │
│ id (PK)          │◄─┘
│ Name             │
│ Description      │
│ Category         │
│ Price            │
│ Images           │
│ CreatedAt        │
│ UpdatedAt        │
└──────────────────┘

┌──────────────────┐
│    Users         │
├──────────────────┤
│ id (PK)          │
│ Username         │
│ Email            │
│ PasswordHash     │
│ Roles            │
│ IsActive         │
│ LastLoginAt      │
│ CreatedAt        │
│ UpdatedAt        │
└──────────────────┘
```

**4.2 - PAYMENT SERVICE (SQL Server):**
```
┌─────────────────────────────────────────────────────────┐
│            PAYMENT SERVICE DATABASE                     │
│                 (SQL Server)                            │
└─────────────────────────────────────────────────────────┘

┌──────────────────┐
│   Payments       │
├──────────────────┤
│ id (PK)          │
│ OrderId          │  (Referência ao Order Service)
│ Amount           │
│ Status           │  (Pending, Processing, Paid, Failed)
│ PaymentMethod    │
│ QrCode           │
│ PreferenceId     │  (Mercado Pago)
│ TransactionId    │
│ CreatedAt        │
│ UpdatedAt        │
└──────────────────┘
```

**4.3 - PRODUCTION SERVICE (MongoDB):**
```
┌─────────────────────────────────────────────────────────┐
│          PRODUCTION SERVICE DATABASE                    │
│                  (MongoDB)                              │
└─────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────┐
│   ProductionOrders (Collection)          │
├──────────────────────────────────────────┤
│ _id (ObjectId)                           │
│ OrderId (String)                         │  (Referência ao Order Service)
│ CustomerId (String, nullable)            │
│ Items: [                                 │
│   {                                      │
│     ProductId: String,                   │
│     ProductName: String,                 │
│     Quantity: Number,                    │
│     UnitPrice: Decimal                   │
│   }                                      │
│ ]                                        │
│ Status: String                           │  (Received, Preparing, Ready, Completed, Cancelled)
│ TotalPrice: Decimal                      │
│ CreatedAt: DateTime                      │
│ UpdatedAt: DateTime                      │
│ StartedAt: DateTime (nullable)           │
│ CompletedAt: DateTime (nullable)         │
└──────────────────────────────────────────┘
```

**Observações Importantes:**
- Cada serviço possui seu próprio banco de dados
- Serviços NÃO acessam bancos de dados de outros serviços
- Comunicação entre serviços é feita via HTTP REST
- OrderId e CustomerId são referências (não foreign keys físicas)
- MongoDB usa documentos (não tabelas)

---

## Como Gerar as Imagens

Use ferramentas como:
- **Draw.io (diagrams.net)**: https://app.diagrams.net/
- **Lucidchart**: https://www.lucidchart.com/
- **Miro**: https://miro.com/
- **PlantUML**: Para diagramas textuais
- **Figma**: Para diagramas mais elaborados

**Elementos Visuais Sugeridos:**
- Retângulos: Processos/Componentes
- Diamantes: Decisões
- Ovals: Início/Fim
- Cilindros: Bancos de Dados
- Setas: Fluxo/Comunicação
- Nuvens: Serviços Externos
- Caixas com bordas: Microserviços

