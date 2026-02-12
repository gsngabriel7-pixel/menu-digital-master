# 💻 Dicas de Desenvolvimento

## 🎯 Boas Práticas

### Backend (Spring Boot)

#### 1. Sempre use @Transactional
```java
@Transactional
public Order createOrder(OrderRequest request) {
    // Garante que tudo seja salvo ou nada seja salvo
}
```

#### 2. Valide dados de entrada
```java
@NotNull(message = "Nome é obrigatório")
private String customerName;

@Min(value = 1, message = "Mesa deve ser maior que 0")
private Integer tableNumber;
```

#### 3. Trate exceções adequadamente
```java
@ExceptionHandler(NoSuchElementException.class)
public ResponseEntity<String> handleNotFound(NoSuchElementException e) {
    return ResponseEntity.status(404).body(e.getMessage());
}
```

#### 4. Use DTOs para transferência de dados
```java
// Não exponha entidades diretamente
public OrderResponse toResponse(Order order) {
    // Converte Order para OrderResponse
}
```

#### 5. Configure logs apropriadamente
```properties
logging.level.com.example.menudigital=DEBUG
logging.level.org.springframework.web=INFO
```

### Frontend (React)

#### 1. Use TypeScript para type safety
```typescript
interface Order {
  id: number;
  customerName: string;
  // Sempre defina tipos
}
```

#### 2. Extraia lógica para hooks customizados
```typescript
function useOrders() {
  const [orders, setOrders] = useState<Order[]>([]);
  const [loading, setLoading] = useState(false);
  
  const fetchOrders = async () => {
    // Lógica reutilizável
  };
  
  return { orders, loading, fetchOrders };
}
```

#### 3. Sempre trate erros
```typescript
try {
  await orderApi.createOrder(data);
} catch (error) {
  console.error('Error:', error);
  toast({
    title: "Erro",
    description: error.message,
    variant: "destructive",
  });
}
```

#### 4. Use loading states
```typescript
const [loading, setLoading] = useState(false);

if (loading) {
  return <Loader />;
}
```

#### 5. Valide formulários
```typescript
if (!name.trim()) {
  toast({
    title: "Nome obrigatório",
    variant: "destructive",
  });
  return;
}
```

## 🔧 Ferramentas Úteis

### Backend

#### 1. Spring Boot DevTools
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <scope>runtime</scope>
</dependency>
```
- Reinicialização automática
- LiveReload

#### 2. Postman/Insomnia
- Testar APIs rapidamente
- Salvar coleções de requisições
- Compartilhar com equipe

#### 3. MySQL Workbench
- Visualizar banco de dados
- Executar queries
- Gerenciar estrutura

### Frontend

#### 1. React Developer Tools
- Inspecionar componentes
- Ver props e state
- Analisar performance

#### 2. Redux DevTools (se usar Redux)
- Ver estado global
- Time travel debugging
- Rastrear ações

#### 3. Lighthouse
- Analisar performance
- Verificar acessibilidade
- Otimizar SEO

## 🐛 Debug

### Backend

#### 1. Logs detalhados
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

private static final Logger log = LoggerFactory.getLogger(OrderService.class);

log.debug("Creating order for customer: {}", customerName);
log.error("Error creating order", exception);
```

#### 2. Breakpoints no IntelliJ/Eclipse
- Pause execução
- Inspecione variáveis
- Execute passo a passo

#### 3. Actuator endpoints
```properties
management.endpoints.web.exposure.include=health,info,metrics
```
Acesse: `http://localhost:8080/actuator/health`

### Frontend

#### 1. Console.log estratégico
```typescript
console.log('Order data:', orderData);
console.log('API response:', response);
console.error('Error:', error);
```

#### 2. React DevTools
- Inspecione componentes
- Veja props e state
- Identifique re-renders

#### 3. Network tab
- Veja requisições HTTP
- Verifique payloads
- Analise tempos de resposta

## 🚀 Performance

### Backend

#### 1. Use índices no banco
```sql
CREATE INDEX idx_token ON orders(token);
CREATE INDEX idx_created_at ON orders(created_at);
```

#### 2. Lazy loading de relacionamentos
```java
@OneToMany(fetch = FetchType.LAZY)
private List<OrderItem> items;
```

#### 3. Cache de queries frequentes
```java
@Cacheable("orders")
public List<Order> getAllOrders() {
    return repository.findAll();
}
```

#### 4. Paginação para listas grandes
```java
@GetMapping
public Page<Order> getAll(Pageable pageable) {
    return repository.findAll(pageable);
}
```

### Frontend

#### 1. Lazy loading de rotas
```typescript
const AdminOrders = lazy(() => import('./pages/AdminOrders'));
```

#### 2. Memoização
```typescript
const memoizedValue = useMemo(() => {
  return expensiveCalculation(data);
}, [data]);
```

#### 3. Debounce em buscas
```typescript
const debouncedSearch = useDebounce(searchTerm, 500);
```

#### 4. Otimize imagens
- Use formatos modernos (WebP)
- Comprima imagens
- Lazy load imagens

## 🔒 Segurança

### Backend

#### 1. Nunca exponha senhas
```properties
# Use variáveis de ambiente
spring.datasource.password=${DB_PASSWORD}
```

#### 2. Valide entrada do usuário
```java
@Valid @RequestBody OrderRequest request
```

#### 3. Use HTTPS em produção
```properties
server.ssl.enabled=true
server.ssl.key-store=classpath:keystore.p12
```

#### 4. Rate limiting
```java
@RateLimiter(name = "orders")
public Order createOrder(OrderRequest request) {
    // Limita requisições por tempo
}
```

### Frontend

#### 1. Sanitize input
```typescript
const sanitizedInput = DOMPurify.sanitize(userInput);
```

#### 2. Não armazene dados sensíveis
```typescript
// Evite localStorage para dados sensíveis
// Use sessionStorage ou memória
```

#### 3. Valide no cliente E servidor
```typescript
// Cliente: UX melhor
if (!isValid(data)) return;

// Servidor: Segurança real
```

## 📝 Código Limpo

### Princípios SOLID

#### 1. Single Responsibility
```java
// Cada classe tem uma responsabilidade
class OrderService {
    // Apenas lógica de pedidos
}

class EmailService {
    // Apenas envio de emails
}
```

#### 2. Open/Closed
```java
// Aberto para extensão, fechado para modificação
interface PaymentProcessor {
    void process(Order order);
}

class StripePayment implements PaymentProcessor {
    // Implementação específica
}
```

#### 3. Dependency Injection
```java
// Injete dependências, não crie
@Service
public class OrderService {
    private final OrderRepository repository;
    
    public OrderService(OrderRepository repository) {
        this.repository = repository;
    }
}
```

### Nomenclatura

#### Backend
```java
// Classes: PascalCase
public class OrderService {}

// Métodos: camelCase
public Order createOrder() {}

// Constantes: UPPER_SNAKE_CASE
private static final String DEFAULT_STATUS = "PENDING";
```

#### Frontend
```typescript
// Componentes: PascalCase
function OrderCard() {}

// Variáveis: camelCase
const orderData = {};

// Constantes: UPPER_SNAKE_CASE
const API_BASE_URL = "";
```

## 🧪 Testes

### Backend (JUnit)

```java
@Test
public void shouldCreateOrder() {
    // Arrange
    OrderRequest request = new OrderRequest();
    request.setCustomerName("Test");
    
    // Act
    Order order = service.createOrder(request);
    
    // Assert
    assertNotNull(order.getId());
    assertEquals("Test", order.getCustomerName());
}
```

### Frontend (Jest)

```typescript
test('should render order card', () => {
  const order = { id: 1, customerName: 'Test' };
  render(<OrderCard order={order} />);
  
  expect(screen.getByText('Test')).toBeInTheDocument();
});
```

## 📊 Monitoramento

### Logs Estruturados

```java
log.info("Order created: orderId={}, customer={}, total={}", 
    order.getId(), 
    order.getCustomerName(), 
    order.getTotal()
);
```

### Métricas

```java
@Timed(value = "orders.create", description = "Time to create order")
public Order createOrder(OrderRequest request) {
    // Mede tempo de execução
}
```

### Health Checks

```java
@Component
public class DatabaseHealthIndicator implements HealthIndicator {
    @Override
    public Health health() {
        // Verifica saúde do banco
        return Health.up().build();
    }
}
```

## 🎨 UI/UX

### Feedback Visual

```typescript
// Loading
{loading && <Spinner />}

// Success
toast({ title: "Sucesso!" });

// Error
toast({ title: "Erro", variant: "destructive" });
```

### Acessibilidade

```tsx
// Use labels
<label htmlFor="name">Nome</label>
<input id="name" />

// Use aria-labels
<button aria-label="Fechar">×</button>

// Use semantic HTML
<nav>, <main>, <footer>
```

### Responsividade

```tsx
// Use classes responsivas
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3">
  {/* Adapta ao tamanho da tela */}
</div>
```

## 🔄 Git

### Commits Semânticos

```bash
feat: adiciona busca por token
fix: corrige cálculo do total
docs: atualiza README
refactor: reorganiza estrutura de pastas
test: adiciona testes para OrderService
```

### Branches

```bash
main          # Produção
develop       # Desenvolvimento
feature/xxx   # Nova funcionalidade
fix/xxx       # Correção de bug
hotfix/xxx    # Correção urgente
```

### Pull Requests

- Descreva as mudanças
- Referencie issues
- Adicione screenshots se UI
- Peça review de código

## 📚 Recursos de Aprendizado

### Backend
- [Spring Boot Docs](https://spring.io/projects/spring-boot)
- [Baeldung](https://www.baeldung.com/)
- [JPA Buddy](https://www.jpa-buddy.com/)

### Frontend
- [React Docs](https://react.dev/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [TailwindCSS Docs](https://tailwindcss.com/docs)

### Geral
- [Clean Code](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)
- [Design Patterns](https://refactoring.guru/design-patterns)
- [REST API Best Practices](https://restfulapi.net/)
