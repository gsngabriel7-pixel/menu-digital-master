# Instruções de Execução - Menu Digital

## 🎯 Resumo das Alterações

O frontend foi atualizado para consumir dinamicamente as APIs do backend Spring Boot. Agora todos os pedidos são criados, buscados e gerenciados através do backend real.

## 📋 Pré-requisitos

### Backend
- Java 17+
- Maven
- MySQL rodando na porta 3306
- Banco de dados `menudigital` criado

### Frontend
- Node.js 16+
- npm ou yarn

## 🚀 Como Executar

### 1. Iniciar o Backend (Spring Boot)

```bash
cd backend
mvn spring-boot:run
```

O backend estará disponível em: `http://localhost:8080`

**Endpoints disponíveis:**
- `GET /api/orders` - Lista todos os pedidos
- `POST /api/orders` - Cria um novo pedido
- `GET /api/orders/{id}` - Busca pedido por ID
- `GET /api/orders/token/{token}` - Busca pedido por token
- `PUT /api/orders/{id}` - Atualiza um pedido
- `DELETE /api/orders/{id}` - Deleta um pedido

### 2. Iniciar o Frontend (React + Vite)

```bash
cd frontend/smart-menu-token-main
npm install
npm run dev
```

O frontend estará disponível em: `http://localhost:5173` (ou a porta que o Vite indicar)

## 🔧 Configuração

### Backend
Edite `backend/src/main/resources/application.properties` se necessário:
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/menudigital
spring.datasource.username=root
spring.datasource.password=SUA_SENHA
```

### Frontend
Edite `frontend/smart-menu-token-main/.env.local` se necessário:
```env
VITE_API_BASE_URL=http://localhost:8080/api
```

## 📝 Funcionalidades Implementadas

### ✅ Páginas Dinamizadas

1. **Checkout** (`/checkout`)
   - Agora cria pedidos reais no backend
   - Envia dados do cliente e itens do carrinho
   - Recebe token do pedido criado
   - Redireciona para página de confirmação

2. **Order** (`/order/:token`)
   - Busca dados do pedido pelo token
   - Exibe informações completas do pedido
   - Mostra nome do cliente, mesa e total
   - Loading state enquanto carrega

3. **TrackOrder** (`/track`)
   - Busca pedidos por token
   - Exibe status do pedido
   - Lista todos os itens
   - Mostra informações do cliente

4. **AdminOrders** (`/admin/orders`) - NOVO!
   - Lista todos os pedidos do sistema
   - Permite deletar pedidos
   - Exibe informações completas de cada pedido
   - Útil para administração do restaurante

### 🔌 Serviço de API

Criado `src/services/api.ts` com todas as funções para comunicação com o backend:
- `createOrder()` - Criar pedido
- `getOrderByToken()` - Buscar por token
- `getOrderById()` - Buscar por ID
- `getAllOrders()` - Listar todos
- `updateOrder()` - Atualizar pedido
- `deleteOrder()` - Deletar pedido

## 🧪 Testando

1. Acesse o menu em `http://localhost:5173`
2. Adicione itens ao carrinho
3. Vá para o checkout
4. Preencha os dados (nome, idade, mesa)
5. Finalize o pedido
6. Você será redirecionado para a página de confirmação com o token
7. Use o token na página "Acompanhar Pedido" para ver o status

## 🔍 Verificando no Backend

Você pode verificar os pedidos criados acessando diretamente:
- `http://localhost:8080/api/orders` - Ver todos os pedidos
- `http://localhost:8080/api/orders/token/SEU_TOKEN` - Ver pedido específico

## 🐛 Troubleshooting

### Erro de CORS
Se houver erro de CORS, verifique se o backend tem `@CrossOrigin(origins = "*")` no controller.

### Erro de conexão
- Verifique se o backend está rodando na porta 8080
- Verifique se o MySQL está rodando
- Confirme que o banco `menudigital` existe

### Pedido não encontrado
- Verifique se o token está correto
- Confirme que o pedido foi criado no banco de dados
- Verifique os logs do backend para erros

## 📊 Estrutura do Banco de Dados

O backend cria automaticamente as tabelas:
- `orders` - Pedidos principais
- `order_item` - Itens dos pedidos

O relacionamento é 1:N (um pedido tem vários itens).
