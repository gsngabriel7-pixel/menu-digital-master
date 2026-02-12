# 🏗️ Arquitetura do Sistema

## 📐 Visão Geral

```
┌─────────────────────────────────────────────────────────────┐
│                        FRONTEND                              │
│                    React + TypeScript                        │
│                      (Port 5173)                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │  Menu    │  │ Checkout │  │  Order   │  │  Track   │   │
│  │  Page    │  │   Page   │  │   Page   │  │   Page   │   │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘   │
│       │             │              │              │          │
│       └─────────────┴──────────────┴──────────────┘          │
│                          │                                   │
│                    ┌─────▼─────┐                            │
│                    │  API      │                            │
│                    │  Service  │                            │
│                    └─────┬─────┘                            │
└──────────────────────────┼──────────────────────────────────┘
                           │
                    HTTP/REST API
                           │
┌──────────────────────────▼──────────────────────────────────┐
│                       BACKEND                                │
│                   Spring Boot + JPA                          │
│                      (Port 8080)                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌────────────────┐                                         │
│  │   Controller   │  ◄── REST Endpoints                     │
│  │  OrderController│                                         │
│  └───────┬────────┘                                         │
│          │                                                   │
│  ┌───────▼────────┐                                         │
│  │    Service     │  ◄── Business Logic                     │
│  │  OrderService  │                                         │
│  └───────┬────────┘                                         │
│          │                                                   │
│  ┌───────▼────────┐                                         │
│  │   Repository   │  ◄── Data Access                        │
│  │ OrderRepository│                                         │
│  └───────┬────────┘                                         │
│          │                                                   │
│  ┌───────▼────────┐                                         │
│  │     Model      │  ◄── Entities                           │
│  │ Order/OrderItem│                                         │
│  └────────────────┘                                         │
└──────────────────────────┬──────────────────────────────────┘
                           │
                        JPA/JDBC
                           │
┌──────────────────────────▼──────────────────────────────────┐
│                      DATABASE                                │
│                       MySQL 8                                │
│                      (Port 3306)                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────┐         ┌──────────────┐                 │
│  │   orders     │ 1     N │  order_item  │                 │
│  │              ├─────────┤              │                 │
│  │ - id         │         │ - id         │                 │
│  │ - customer   │         │ - name       │                 │
│  │ - table      │         │ - price      │                 │
│  │ - total      │         │ - quantity   │                 │
│  │ - status     │         │ - order_id   │                 │
│  │ - token      │         └──────────────┘                 │
│  │ - created_at │                                           │
│  └──────────────┘                                           │
└─────────────────────────────────────────────────────────────┘
```

## 🔄 Fluxo de Dados

### 1. Criar Pedido

```
┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐
│ Cliente │────▶│Checkout │────▶│   API   │────▶│ Backend │
│  (UI)   │     │  Page   │     │ Service │     │Controller│
└─────────┘     └─────────┘     └─────────┘     └────┬────┘
                                                       │
                                                  ┌────▼────┐
                                                  │ Service │
                                                  └────┬────┘
                                                       │
                                                  ┌────▼────┐
                                                  │  Repo   │
                                                  └────┬────┘
                                                       │
                                                  ┌────▼────┐
                                                  │  MySQL  │
                                                  └────┬────┘
                                                       │
┌─────────┐     ┌─────────┐     ┌─────────┐     ┌────▼────┐
│ Cliente │◀────│  Order  │◀────│   API   │◀────│ Backend │
│  (UI)   │     │  Page   │     │ Service │     │ Response│
└─────────┘     └─────────┘     └─────────┘     └─────────┘
```

### 2. Buscar Pedido

```
┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐
│ Cliente │────▶│  Track  │────▶│   API   │────▶│ Backend │
│  (UI)   │     │  Page   │     │ Service │     │Controller│
└─────────┘     └─────────┘     └─────────┘     └────┬────┘
    ▲                                                  │
    │                                             ┌────▼────┐
    │                                             │ Service │
    │                                             └────┬────┘
    │                                                  │
    │                                             ┌────▼────┐
    │                                             │  Repo   │
    │                                             └────┬────┘
    │                                                  │
    │                                             ┌────▼────┐
    │                                             │  MySQL  │
    │                                             └────┬────┘
    │                                                  │
    └──────────────────────────────────────────────────┘
```

## 📦 Camadas da Aplicação

### Frontend (React)

```
src/
├── pages/              # Páginas da aplicação
│   ├── Menu.tsx       # Cardápio
│   ├── Checkout.tsx   # Finalizar pedido
│   ├── Order.tsx      # Confirmação
│   ├── TrackOrder.tsx # Acompanhamento
│   └── AdminOrders.tsx# Administração
│
├── services/          # Comunicação com API
│   └── api.ts        # Funções HTTP
│
├── components/        # Componentes reutilizáveis
│   ├── ProductCard.tsx
│   ├── CartDrawer.tsx
│   └── ui/           # Componentes Shadcn
│
├── contexts/          # Estado global
│   └── CartContext.tsx
│
└── App.tsx           # Rotas principais
```

### Backend (Spring Boot)

```
src/main/java/com/example/menudigital/
├── controller/        # Endpoints REST
│   └── OrderController.java
│
├── service/          # Lógica de negócio
│   └── OrderService.java
│
├── repository/       # Acesso a dados
│   └── OrderRepository.java
│
├── model/            # Entidades JPA
│   ├── Order.java
│   └── OrderItem.java
│
└── dto/              # Transfer Objects
    ├── OrderRequest.java
    ├── OrderResponse.java
    └── OrderItemDTO.java
```

## 🔌 API REST

### Endpoints

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | `/api/orders` | Lista todos os pedidos |
| POST | `/api/orders` | Cria novo pedido |
| GET | `/api/orders/{id}` | Busca por ID |
| GET | `/api/orders/token/{token}` | Busca por token |
| PUT | `/api/orders/{id}` | Atualiza pedido |
| DELETE | `/api/orders/{id}` | Deleta pedido |

### Estrutura de Dados

#### Request (POST/PUT)
```json
{
  "customerName": "string",
  "tableNumber": "string",
  "items": [
    {
      "name": "string",
      "price": number,
      "quantity": number
    }
  ]
}
```

#### Response
```json
{
  "id": number,
  "customerName": "string",
  "tableNumber": "string",
  "total": number,
  "status": "string",
  "token": "string",
  "items": [
    {
      "id": number,
      "name": "string",
      "price": number,
      "quantity": number
    }
  ],
  "createdAt": "datetime"
}
```

## 🗄️ Modelo de Dados

### Relacionamentos

```
Order (1) ──────── (N) OrderItem
  │
  ├─ id (PK)
  ├─ customerName
  ├─ tableNumber
  ├─ total
  ├─ status
  ├─ token (UNIQUE)
  └─ createdAt
                    │
                    ├─ id (PK)
                    ├─ name
                    ├─ price
                    ├─ quantity
                    └─ order_id (FK)
```

### Cascade

- Ao deletar Order, todos OrderItems são deletados automaticamente
- Relacionamento bidirecional mantido pelo JPA

## 🔒 Segurança

### CORS
```java
@CrossOrigin(origins = "*")
```
- Permite requisições de qualquer origem
- Configurar para domínio específico em produção

### Validações
- Backend valida dados recebidos
- Frontend valida formulários
- Tratamento de erros em todas as camadas

## 🚀 Deploy

### Frontend
```bash
npm run build
# Gera pasta dist/ com arquivos estáticos
# Deploy em: Vercel, Netlify, etc.
```

### Backend
```bash
mvn clean package
# Gera arquivo .jar
# Deploy em: Heroku, AWS, Railway, etc.
```

### Banco de Dados
- MySQL em servidor dedicado
- Configurar variáveis de ambiente
- Backup regular dos dados

## 📊 Performance

### Frontend
- Lazy loading de componentes
- Otimização de imagens
- Cache de requisições

### Backend
- Connection pooling
- Índices no banco de dados
- Paginação para listas grandes

### Banco de Dados
- Índice em `token` (UNIQUE)
- Índice em `created_at`
- Relacionamento otimizado

## 🔍 Monitoramento

### Logs
- Backend: Spring Boot Actuator
- Frontend: Console do navegador
- Banco: MySQL logs

### Métricas
- Tempo de resposta da API
- Taxa de erro
- Uso de memória
- Conexões ativas

## 🧪 Testes

### Frontend
- Testes unitários (Jest)
- Testes de integração
- Testes E2E (Cypress)

### Backend
- Testes unitários (JUnit)
- Testes de integração
- Testes de API (Postman)

## 📈 Escalabilidade

### Horizontal
- Múltiplas instâncias do backend
- Load balancer
- Cache distribuído (Redis)

### Vertical
- Aumentar recursos do servidor
- Otimizar queries
- Índices adicionais
