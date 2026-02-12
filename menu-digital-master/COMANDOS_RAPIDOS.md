# ⚡ Comandos Rápidos

## 🚀 Iniciar Projeto

### Backend
```bash
cd backend
mvn spring-boot:run
```

### Frontend
```bash
cd frontend/smart-menu-token-main
npm install
npm run dev
```

## 🔗 URLs Importantes

- **Frontend**: http://localhost:5173
- **Backend API**: http://localhost:8080/api
- **Admin**: http://localhost:5173/admin/orders
- **Acompanhar**: http://localhost:5173/track

## 🧪 Testar API Rapidamente

### Criar pedido de teste
```bash
curl -X POST http://localhost:8080/api/orders \
  -H "Content-Type: application/json" \
  -d '{
    "customerName": "Teste",
    "tableNumber": "1",
    "items": [
      {"name": "X-Burger", "price": 28.90, "quantity": 1}
    ]
  }'
```

### Listar todos os pedidos
```bash
curl http://localhost:8080/api/orders
```

## 🗄️ MySQL

### Conectar ao banco
```bash
mysql -u root -p
```

### Ver pedidos
```sql
USE menudigital;
SELECT * FROM orders;
SELECT * FROM order_item;
```

### Limpar dados
```sql
DELETE FROM order_item;
DELETE FROM orders;
```

## 📦 Build para Produção

### Frontend
```bash
cd frontend/smart-menu-token-main
npm run build
```

### Backend
```bash
cd backend
mvn clean package
java -jar target/menu-digital-1.0.0.jar
```

## 🔧 Troubleshooting Rápido

### Backend não inicia
```bash
# Verificar se MySQL está rodando
# Windows
net start MySQL80

# Verificar porta 8080
netstat -ano | findstr :8080
```

### Frontend não conecta
```bash
# Verificar .env.local
cat frontend/smart-menu-token-main/.env.local

# Verificar se backend está rodando
curl http://localhost:8080/api/orders
```

### Erro de CORS
- Reinicie o backend
- Verifique `@CrossOrigin` no controller

## 🎯 Fluxo de Teste Completo

1. Inicie backend e frontend
2. Acesse http://localhost:5173
3. Adicione itens ao carrinho
4. Vá para checkout
5. Preencha: Nome, Idade, Mesa
6. Finalize pedido
7. Copie o token
8. Vá para "Acompanhar Pedido"
9. Cole o token e busque
10. Acesse /admin/orders para ver todos

## 📱 Páginas Disponíveis

- `/` - Menu principal
- `/checkout` - Finalizar pedido
- `/order/:token` - Confirmação do pedido
- `/track` - Acompanhar pedido por token
- `/admin/orders` - Administração (listar/deletar)

## 🔑 Variáveis de Ambiente

### Backend (application.properties)
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/menudigital
spring.datasource.username=root
spring.datasource.password=SUA_SENHA
```

### Frontend (.env.local)
```env
VITE_API_BASE_URL=http://localhost:8080/api
```
