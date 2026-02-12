# 💡 Exemplos de Uso

## 🎯 Cenários Práticos

### Cenário 1: Cliente Fazendo Pedido

#### Passo a Passo
1. Cliente acessa o menu em `http://localhost:5173`
2. Navega pelas categorias (Lanches, Bebidas, Sobremesas)
3. Adiciona itens ao carrinho:
   - 2x X-Burger Classic (R$ 28,90 cada)
   - 1x Refrigerante Lata (R$ 5,90)
4. Clica no ícone do carrinho
5. Revisa os itens e clica em "Finalizar Pedido"
6. Preenche o formulário:
   - Nome: "João Silva"
   - Idade: 25
   - Mesa: 5
7. Clica em "Finalizar Pedido"
8. Sistema cria pedido no backend
9. Cliente recebe token: `abc123-def456-ghi789`
10. É redirecionado para página de confirmação

#### Dados Enviados ao Backend
```json
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

#### Resposta do Backend
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

---

### Cenário 2: Cliente Acompanhando Pedido

#### Passo a Passo
1. Cliente acessa `http://localhost:5173/track`
2. Digita o token recebido: `abc123-def456-ghi789`
3. Clica em buscar
4. Sistema busca pedido no backend
5. Exibe informações:
   - Status: PENDING
   - Cliente: João Silva
   - Mesa: 5
   - Total: R$ 63,70
   - Itens do pedido

#### Requisição ao Backend
```
GET http://localhost:8080/api/orders/token/abc123-def456-ghi789
```

#### Resposta
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

---

### Cenário 3: Administrador Gerenciando Pedidos

#### Passo a Passo
1. Administrador acessa `http://localhost:5173/admin/orders`
2. Sistema lista todos os pedidos
3. Visualiza informações de cada pedido:
   - Token, Cliente, Mesa, Status, Total
   - Lista de itens
4. Pode deletar pedidos finalizados

#### Requisição ao Backend
```
GET http://localhost:8080/api/orders
```

#### Resposta
```json
[
  {
    "id": 1,
    "customerName": "João Silva",
    "tableNumber": "5",
    "total": 63.70,
    "status": "PENDING",
    "token": "abc123-def456-ghi789",
    "items": [...],
    "createdAt": "2024-01-15T10:30:00"
  },
  {
    "id": 2,
    "customerName": "Maria Santos",
    "tableNumber": "3",
    "total": 45.80,
    "status": "PENDING",
    "token": "xyz789-uvw456-rst123",
    "items": [...],
    "createdAt": "2024-01-15T10:35:00"
  }
]
```

---

### Cenário 4: Testando API com cURL

#### Criar Pedido
```bash
curl -X POST http://localhost:8080/api/orders \
  -H "Content-Type: application/json" \
  -d '{
    "customerName": "Pedro Costa",
    "tableNumber": "7",
    "items": [
      {
        "name": "X-Bacon Especial",
        "price": 32.90,
        "quantity": 1
      },
      {
        "name": "Batata Frita",
        "price": 15.90,
        "quantity": 1
      },
      {
        "name": "Suco Natural",
        "price": 8.90,
        "quantity": 2
      }
    ]
  }'
```

#### Resposta
```json
{
  "id": 3,
  "customerName": "Pedro Costa",
  "tableNumber": "7",
  "total": 66.60,
  "status": "PENDING",
  "token": "def456-ghi789-jkl012",
  "items": [
    {
      "id": 3,
      "name": "X-Bacon Especial",
      "price": 32.90,
      "quantity": 1
    },
    {
      "id": 4,
      "name": "Batata Frita",
      "price": 15.90,
      "quantity": 1
    },
    {
      "id": 5,
      "name": "Suco Natural",
      "price": 8.90,
      "quantity": 2
    }
  ],
  "createdAt": "2024-01-15T10:40:00"
}
```

#### Buscar Pedido
```bash
curl http://localhost:8080/api/orders/token/def456-ghi789-jkl012
```

#### Listar Todos
```bash
curl http://localhost:8080/api/orders
```

#### Deletar Pedido
```bash
curl -X DELETE http://localhost:8080/api/orders/3
```

---

### Cenário 5: Atualizar Pedido

#### Situação
Cliente quer adicionar mais itens ao pedido existente

#### Requisição
```bash
curl -X PUT http://localhost:8080/api/orders/1 \
  -H "Content-Type: application/json" \
  -d '{
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
      },
      {
        "name": "Brownie",
        "price": 14.90,
        "quantity": 1
      }
    ]
  }'
```

#### Resposta
```json
{
  "id": 1,
  "customerName": "João Silva",
  "tableNumber": "5",
  "total": 78.60,
  "status": "PENDING",
  "token": "abc123-def456-ghi789",
  "items": [
    {
      "id": 6,
      "name": "X-Burger Classic",
      "price": 28.90,
      "quantity": 2
    },
    {
      "id": 7,
      "name": "Refrigerante Lata",
      "price": 5.90,
      "quantity": 1
    },
    {
      "id": 8,
      "name": "Brownie",
      "price": 14.90,
      "quantity": 1
    }
  ],
  "createdAt": "2024-01-15T10:30:00"
}
```

---

## 🧪 Testes Automatizados

### Teste 1: Criar Múltiplos Pedidos
```bash
#!/bin/bash

# Criar 5 pedidos de teste
for i in {1..5}; do
  curl -X POST http://localhost:8080/api/orders \
    -H "Content-Type: application/json" \
    -d "{
      \"customerName\": \"Cliente $i\",
      \"tableNumber\": \"$i\",
      \"items\": [
        {
          \"name\": \"X-Burger Classic\",
          \"price\": 28.90,
          \"quantity\": 1
        }
      ]
    }"
  echo ""
done
```

### Teste 2: Buscar Todos e Contar
```bash
#!/bin/bash

# Buscar todos os pedidos
response=$(curl -s http://localhost:8080/api/orders)

# Contar pedidos (requer jq)
count=$(echo $response | jq '. | length')
echo "Total de pedidos: $count"
```

### Teste 3: Criar e Buscar por Token
```bash
#!/bin/bash

# Criar pedido e extrair token
response=$(curl -s -X POST http://localhost:8080/api/orders \
  -H "Content-Type: application/json" \
  -d '{
    "customerName": "Teste Token",
    "tableNumber": "99",
    "items": [
      {"name": "Teste", "price": 10.00, "quantity": 1}
    ]
  }')

token=$(echo $response | jq -r '.token')
echo "Token criado: $token"

# Buscar pelo token
echo "Buscando pedido..."
curl http://localhost:8080/api/orders/token/$token
```

---

## 📊 Consultas SQL Úteis

### Ver Todos os Pedidos
```sql
SELECT 
  o.id,
  o.customer_name,
  o.table_number,
  o.total,
  o.status,
  o.token,
  COUNT(oi.id) as total_items
FROM orders o
LEFT JOIN order_item oi ON o.id = oi.order_id
GROUP BY o.id
ORDER BY o.created_at DESC;
```

### Pedidos por Mesa
```sql
SELECT 
  table_number,
  COUNT(*) as total_pedidos,
  SUM(total) as valor_total
FROM orders
GROUP BY table_number
ORDER BY total_pedidos DESC;
```

### Itens Mais Pedidos
```sql
SELECT 
  name,
  SUM(quantity) as total_vendido,
  COUNT(*) as vezes_pedido,
  SUM(price * quantity) as receita_total
FROM order_item
GROUP BY name
ORDER BY total_vendido DESC;
```

### Pedidos do Dia
```sql
SELECT *
FROM orders
WHERE DATE(created_at) = CURDATE()
ORDER BY created_at DESC;
```

### Ticket Médio
```sql
SELECT 
  AVG(total) as ticket_medio,
  MIN(total) as menor_pedido,
  MAX(total) as maior_pedido
FROM orders;
```

---

## 🎨 Personalizações

### Adicionar Novo Status
1. Backend: Atualizar `OrderService.java`
```java
order.setStatus("CONFIRMADO"); // ou "PREPARANDO", "PRONTO", etc.
```

2. Frontend: Atualizar `TrackOrder.tsx`
```tsx
{orderData.status === 'PREPARANDO' && (
  <div className="status-preparando">
    Seu pedido está sendo preparado!
  </div>
)}
```

### Adicionar Campo de Observação
1. Backend: Adicionar campo em `Order.java`
```java
private String observation;
```

2. Frontend: Adicionar campo no formulário
```tsx
<Input
  placeholder="Observações (opcional)"
  value={observation}
  onChange={(e) => setObservation(e.target.value)}
/>
```

### Adicionar Filtro por Data
1. Backend: Criar endpoint
```java
@GetMapping("/date/{date}")
public List<Order> getByDate(@PathVariable String date) {
    return service.getOrdersByDate(date);
}
```

2. Frontend: Adicionar filtro
```tsx
<Input
  type="date"
  onChange={(e) => fetchOrdersByDate(e.target.value)}
/>
```
