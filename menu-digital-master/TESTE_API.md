# Testando a API do Backend

## 📡 Endpoints Disponíveis

### 1. Criar Pedido
```bash
POST http://localhost:8080/api/orders
Content-Type: application/json

{
  "customerName": "João Silva",
  "tableNumber": "5",
  "items": [
    {
      "name": "X-Burger Classic",
      "price": 28.90,
      "quantity": 2
    },
    {
      "name": "Refrigerante Lata",
      "price": 5.90,
      "quantity": 1
    }
  ]
}
```

**Resposta:**
```json
{
  "id": 1,
  "customerName": "João Silva",
  "tableNumber": "5",
  "total": 63.70,
  "status": "PENDING",
  "token": "abc123-def456-ghi789",
  "items": [
    {
      "id": 1,
      "name": "X-Burger Classic",
      "price": 28.90,
      "quantity": 2
    },
    {
      "id": 2,
      "name": "Refrigerante Lata",
      "price": 5.90,
      "quantity": 1
    }
  ],
  "createdAt": "2024-01-15T10:30:00"
}
```

### 2. Listar Todos os Pedidos
```bash
GET http://localhost:8080/api/orders
```

### 3. Buscar Pedido por ID
```bash
GET http://localhost:8080/api/orders/1
```

### 4. Buscar Pedido por Token
```bash
GET http://localhost:8080/api/orders/token/abc123-def456-ghi789
```

### 5. Atualizar Pedido
```bash
PUT http://localhost:8080/api/orders/1
Content-Type: application/json

{
  "customerName": "João Silva Atualizado",
  "tableNumber": "10",
  "items": [
    {
      "name": "X-Bacon Especial",
      "price": 32.90,
      "quantity": 1
    }
  ]
}
```

### 6. Deletar Pedido
```bash
DELETE http://localhost:8080/api/orders/1
```

## 🧪 Testando com cURL

### Criar Pedido
```bash
curl -X POST http://localhost:8080/api/orders \
  -H "Content-Type: application/json" \
  -d '{
    "customerName": "Maria Santos",
    "tableNumber": "3",
    "items": [
      {
        "name": "X-Burger Classic",
        "price": 28.90,
        "quantity": 1
      }
    ]
  }'
```

### Listar Pedidos
```bash
curl http://localhost:8080/api/orders
```

### Buscar por Token
```bash
curl http://localhost:8080/api/orders/token/SEU_TOKEN_AQUI
```

### Deletar Pedido
```bash
curl -X DELETE http://localhost:8080/api/orders/1
```

## 🔍 Testando com Postman

1. Importe a collection abaixo ou crie manualmente as requisições
2. Configure a base URL: `http://localhost:8080/api`
3. Teste cada endpoint

### Collection Postman (JSON)
```json
{
  "info": {
    "name": "Menu Digital API",
    "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
  },
  "item": [
    {
      "name": "Criar Pedido",
      "request": {
        "method": "POST",
        "header": [
          {
            "key": "Content-Type",
            "value": "application/json"
          }
        ],
        "body": {
          "mode": "raw",
          "raw": "{\n  \"customerName\": \"João Silva\",\n  \"tableNumber\": \"5\",\n  \"items\": [\n    {\n      \"name\": \"X-Burger Classic\",\n      \"price\": 28.90,\n      \"quantity\": 2\n    }\n  ]\n}"
        },
        "url": {
          "raw": "http://localhost:8080/api/orders",
          "protocol": "http",
          "host": ["localhost"],
          "port": "8080",
          "path": ["api", "orders"]
        }
      }
    },
    {
      "name": "Listar Pedidos",
      "request": {
        "method": "GET",
        "url": {
          "raw": "http://localhost:8080/api/orders",
          "protocol": "http",
          "host": ["localhost"],
          "port": "8080",
          "path": ["api", "orders"]
        }
      }
    },
    {
      "name": "Buscar por Token",
      "request": {
        "method": "GET",
        "url": {
          "raw": "http://localhost:8080/api/orders/token/{{token}}",
          "protocol": "http",
          "host": ["localhost"],
          "port": "8080",
          "path": ["api", "orders", "token", "{{token}}"]
        }
      }
    }
  ]
}
```

## 🐛 Verificando Erros

### Erro 404 - Not Found
- Verifique se o backend está rodando
- Confirme a URL e porta corretas
- Verifique se o endpoint existe

### Erro 500 - Internal Server Error
- Verifique os logs do backend
- Confirme se o MySQL está rodando
- Verifique se o banco de dados existe

### Erro de CORS
- Confirme que `@CrossOrigin(origins = "*")` está no controller
- Reinicie o backend após alterações

## 📊 Verificando no Banco de Dados

```sql
-- Ver todos os pedidos
SELECT * FROM orders;

-- Ver todos os itens
SELECT * FROM order_item;

-- Ver pedido com itens
SELECT o.*, oi.* 
FROM orders o 
LEFT JOIN order_item oi ON o.id = oi.order_id 
WHERE o.id = 1;

-- Buscar por token
SELECT * FROM orders WHERE token = 'SEU_TOKEN';
```
