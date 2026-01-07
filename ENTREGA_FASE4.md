# 📦 Entrega - Tech Challenge Fase 4

**Disciplina**: Software Architecture - Arquitetura de Microserviços  
**Pós-graduação**: SOAT - FIAP

---

## 👥 Integrantes do Grupo

| Nome | Discord |
|------|---------|
| **[SEU NOME]** | **[SEU DISCORD]** |
| **[NOME 2]** | **[DISCORD 2]** |
| **[NOME 3]** | **[DISCORD 3]** |

---

## 🎯 Requisitos Atendidos

### ✅ 1. Refatoração em 3 Microserviços

Conforme item **1** do PDF da Fase 4, o projeto foi refatorado em **3 microserviços independentes**:

#### 📦 Order Service (Pedidos)
- **Responsabilidade**: Operacionalizar o processo de pedidos, registrar pedidos, retornar informações, listar pedidos em produção
- **Banco de Dados**: SQL Server
- **Repositório**: https://github.com/grupo97fiapsoat/fastfood-order-service

#### 💳 Payment Service (Pagamentos)
- **Responsabilidade**: Operacionalizar a cobrança, registrar solicitação de pagamento, receber retorno do processador, atualizar status do pedido
- **Banco de Dados**: SQL Server
- **Repositório**: https://github.com/grupo97fiapsoat/fastfood-payment-service

#### 🏭 Production Service (Produção)
- **Responsabilidade**: Operacionalizar o processo de produção, acompanhar fila de pedidos, atualizar status de cada passo
- **Banco de Dados**: MongoDB (NoSQL)
- **Repositório**: https://github.com/grupo97fiapsoat/fastfood-production-service

**✅ Comunicação entre Serviços**: Implementada via HTTP REST (sem acesso direto aos bancos de dados de outros serviços)

---

### ✅ 2. Testes Unitários

Conforme item **2** do PDF da Fase 4:

#### 2.a BDD (Behavior-Driven Development) ✅
- **Framework**: SpecFlow 3.9.74
- **Feature**: `CriarPedidoEProcessarPagamento.feature`
- **Cenários implementados**:
  - ✅ Cliente cria um pedido com sucesso
  - ✅ Processar checkout do pedido
  - ✅ Confirmar pagamento do pedido
- **Localização**: Projeto principal `techchallenge_fase4` (BDD compartilhado entre serviços)

#### 2.b Cobertura de Testes >= 80% ✅

| Microserviço | Cobertura | Evidência |
|--------------|-----------|-----------|
| **Order Service** | **80.3%** | [SonarQube](https://sonarcloud.io/project/overview?id=grupo97fiapsoat_fastfood-order-service) |
| **Payment Service** | **81.46%** | [SonarQube](https://sonarcloud.io/project/overview?id=grupo97fiapsoat_fastfood-payment-service) |
| **Production Service** | **83.75%** | [SonarQube](https://sonarcloud.io/project/overview?id=grupo97fiapsoat_fastfood-production-service) |
| **Média Geral** | **81.84%** | ✅ |

**📊 Evidências adicionais:**
- Relatórios HTML de cobertura disponíveis nos **Artifacts** de cada workflow CI/CD
- Quality Gates do SonarQube: **Passed** em todos os serviços

---

### ✅ 3. Repositórios Separados

Conforme item **3** do PDF da Fase 4:

#### 3.a Branch Protection ✅
- ✅ Todas as branchs `main` estão protegidas
- ✅ Não permitem commits diretos (apenas via Pull Request)
- ✅ Configurado via **GitHub Rulesets**

#### 3.b Pull Request com CI/CD ✅
- ✅ Todos os PRs para `main` validam:
  - ✅ **Build da aplicação**
  - ✅ **Testes unitários**
  - ✅ **Qualidade de código via SonarQube** (coverage mínimo: 70%)
- ✅ Quality Gate do SonarQube deve passar para merge

#### 3.c Deploy Automático ✅
- ✅ Workflow de deploy configurado em cada repositório
- ✅ Executa automaticamente no merge para `main`
- ✅ Deploy workflows: `.github/workflows/deploy.yml`

---

## 🔗 Links dos Repositórios

### 📦 Microserviços

1. **Order Service**
   - Repositório: https://github.com/grupo97fiapsoat/fastfood-order-service
   - SonarQube: https://sonarcloud.io/project/overview?id=grupo97fiapsoat_fastfood-order-service
   - CI/CD: https://github.com/grupo97fiapsoat/fastfood-order-service/actions

2. **Payment Service**
   - Repositório: https://github.com/grupo97fiapsoat/fastfood-payment-service
   - SonarQube: https://sonarcloud.io/project/overview?id=grupo97fiapsoat_fastfood-payment-service
   - CI/CD: https://github.com/grupo97fiapsoat/fastfood-payment-service/actions

3. **Production Service**
   - Repositório: https://github.com/grupo97fiapsoat/fastfood-production-service
   - SonarQube: https://sonarcloud.io/project/overview?id=grupo97fiapsoat_fastfood-production-service
   - CI/CD: https://github.com/grupo97fiapsoat/fastfood-production-service/actions

### 📚 Projeto Principal (Fase 4)

- **Repositório Principal**: https://github.com/grupo97fiapsoat/techchallenge_fase4
- **README**: Contém documentação completa, diagramas e evidências

---

## 🎥 Vídeo de Demonstração

**🔗 Link do vídeo**: [INSERIR LINK DO YOUTUBE/VIMEO]

O vídeo demonstra:
- ✅ Funcionamento da aplicação
- ✅ Atualizações efetuadas na arquitetura (microserviços)
- ✅ Processo de deploy de todos os microserviços
- ✅ Processo de teste funcionando (checks verdes no GitHub Actions)

---

## ✅ Checklist de Entrega

- ✅ 3 microserviços refatorados e funcionando
- ✅ Testes unitários com cobertura >= 80%
- ✅ Testes BDD implementados (SpecFlow)
- ✅ 3 repositórios separados (privados)
- ✅ Branch protection configurado
- ✅ CI/CD com SonarQube funcionando
- ✅ Deploy automático configurado
- ✅ Colaborador `soat-architecture` adicionado aos 3 repositórios
- ✅ READMEs com evidências de cobertura
- ✅ Vídeo demonstrativo gravado e publicado

---

## 📝 Observações

- Todos os repositórios estão configurados como **privados**
- O usuário **`soat-architecture`** foi adicionado como colaborador em todos os repositórios
- Os relatórios de cobertura estão disponíveis em:
  - SonarQube Cloud (dashboards públicos)
  - GitHub Actions Artifacts (relatórios HTML)
  - README de cada microserviço

---

**Data de Entrega**: [INSERIR DATA]  
**Grupo**: 97

