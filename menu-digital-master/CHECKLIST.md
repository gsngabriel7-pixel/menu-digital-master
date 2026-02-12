# ✅ Checklist de Verificação

## 🔧 Antes de Iniciar

### Backend
- [ ] Java 17+ instalado
- [ ] Maven instalado
- [ ] MySQL instalado e rodando
- [ ] Banco de dados `menudigital` criado
- [ ] Senha do MySQL configurada em `application.properties`

### Frontend
- [ ] Node.js 16+ instalado
- [ ] npm ou yarn instalado
- [ ] Dependências instaladas (`npm install`)

## 🚀 Inicialização

### Backend
- [ ] Backend iniciado com `mvn spring-boot:run`
- [ ] Backend rodando na porta 8080
- [ ] Sem erros no console
- [ ] Tabelas criadas automaticamente no MySQL

### Frontend
- [ ] Frontend iniciado com `npm run dev`
- [ ] Frontend rodando (geralmente porta 5173)
- [ ] Sem erros no console
- [ ] Página carrega corretamente

## 🧪 Testes Básicos

### API Backend
- [ ] GET `/api/orders` retorna lista (pode estar vazia)
- [ ] POST `/api/orders` cria pedido com sucesso
- [ ] GET `/api/orders/token/{token}` retorna pedido criado
- [ ] Sem erros de CORS

### Frontend
- [ ] Página inicial (`/`) carrega o menu
- [ ] Consegue adicionar itens ao carrinho
- [ ] Carrinho exibe itens corretamente
- [ ] Página de checkout (`/checkout`) carrega
- [ ] Formulário de checkout funciona
- [ ] Após criar pedido, redireciona para `/order/:token`
- [ ] Página de confirmação exibe dados do pedido
- [ ] Página de acompanhamento (`/track`) funciona
- [ ] Busca por token retorna pedido correto
- [ ] Página admin (`/admin/orders`) lista pedidos

## 🔍 Verificações de Integração

### Criar Pedido Completo
- [ ] Adicionar itens ao carrinho
- [ ] Ir para checkout
- [ ] Preencher formulário (nome, idade, mesa)
- [ ] Clicar em "Finalizar Pedido"
- [ ] Pedido criado no backend
- [ ] Token gerado e exibido
- [ ] Pedido salvo no MySQL
- [ ] Redirecionamento funciona

### Buscar Pedido
- [ ] Copiar token do pedido criado
- [ ] Ir para `/track`
- [ ] Colar token e buscar
- [ ] Dados do pedido exibidos corretamente
- [ ] Itens listados corretamente
- [ ] Total calculado corretamente

### Administração
- [ ] Acessar `/admin/orders`
- [ ] Lista de pedidos carregada
- [ ] Informações exibidas corretamente
- [ ] Botão de deletar funciona
- [ ] Após deletar, lista atualiza

## 🗄️ Verificações no Banco de Dados

```sql
-- Verificar se tabelas foram criadas
SHOW TABLES;

-- Deve mostrar: orders, order_item

-- Verificar estrutura da tabela orders
DESCRIBE orders;

-- Verificar estrutura da tabela order_item
DESCRIBE order_item;

-- Verificar se há pedidos
SELECT COUNT(*) FROM orders;

-- Verificar se há itens
SELECT COUNT(*) FROM order_item;

-- Ver último pedido criado
SELECT * FROM orders ORDER BY created_at DESC LIMIT 1;
```

- [ ] Tabelas `orders` e `order_item` existem
- [ ] Pedidos criados aparecem no banco
- [ ] Itens dos pedidos aparecem no banco
- [ ] Relacionamento entre tabelas funciona
- [ ] Token é único para cada pedido

## 🐛 Troubleshooting

### Backend não inicia
- [ ] Verificar se MySQL está rodando
- [ ] Verificar credenciais em `application.properties`
- [ ] Verificar se porta 8080 está livre
- [ ] Verificar logs de erro no console

### Frontend não conecta ao Backend
- [ ] Verificar se backend está rodando
- [ ] Verificar URL em `.env.local`
- [ ] Verificar console do navegador para erros
- [ ] Verificar se há erro de CORS

### Pedido não é criado
- [ ] Verificar console do navegador
- [ ] Verificar logs do backend
- [ ] Verificar se dados estão sendo enviados corretamente
- [ ] Verificar se MySQL está aceitando conexões

### Pedido não é encontrado por token
- [ ] Verificar se token está correto
- [ ] Verificar se pedido existe no banco
- [ ] Verificar logs do backend
- [ ] Verificar se endpoint está correto

## 📊 Testes de Performance

### Backend
- [ ] Criar 10 pedidos rapidamente
- [ ] Listar todos os pedidos
- [ ] Buscar pedidos por token
- [ ] Tempo de resposta aceitável (< 1s)

### Frontend
- [ ] Navegação entre páginas é rápida
- [ ] Carrinho responde instantaneamente
- [ ] Loading states aparecem quando necessário
- [ ] Sem travamentos ou lentidão

## 🔒 Segurança

- [ ] Validações no frontend funcionam
- [ ] Validações no backend funcionam
- [ ] Erros não expõem informações sensíveis
- [ ] CORS configurado corretamente
- [ ] Senhas não estão em código versionado

## 📱 Responsividade

- [ ] Menu funciona em mobile
- [ ] Checkout funciona em mobile
- [ ] Carrinho funciona em mobile
- [ ] Botões são clicáveis em telas pequenas
- [ ] Texto é legível em todas as telas

## 🎨 Interface

- [ ] Cores e estilos consistentes
- [ ] Ícones carregam corretamente
- [ ] Imagens dos produtos aparecem
- [ ] Feedback visual para ações do usuário
- [ ] Mensagens de erro são claras

## 📝 Documentação

- [ ] README.md está completo
- [ ] INSTRUCOES.md está claro
- [ ] Exemplos de API funcionam
- [ ] Comandos rápidos estão corretos
- [ ] Arquitetura está documentada

## 🚢 Pronto para Produção

- [ ] Todos os testes passam
- [ ] Sem erros no console
- [ ] Sem warnings críticos
- [ ] Documentação completa
- [ ] Código comentado onde necessário
- [ ] Variáveis de ambiente configuradas
- [ ] Backup do banco de dados configurado
- [ ] Logs configurados
- [ ] Monitoramento configurado

## 📈 Melhorias Futuras

- [ ] Adicionar autenticação
- [ ] Implementar WebSocket
- [ ] Adicionar mais status de pedido
- [ ] Criar dashboard de estatísticas
- [ ] Adicionar notificações
- [ ] Implementar sistema de pagamento
- [ ] Adicionar impressão de comprovantes
- [ ] Criar app mobile

---

## ✨ Status Final

Data: ___/___/______

- [ ] Projeto totalmente funcional
- [ ] Todos os testes passaram
- [ ] Documentação completa
- [ ] Pronto para demonstração
- [ ] Pronto para produção

**Observações:**
_____________________________________
_____________________________________
_____________________________________
