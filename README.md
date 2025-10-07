# Projeto E-commerce — Modelagem Lógica e Script SQL Completo

## 🧠 Descrição do Projeto
Este projeto representa a **modelagem lógica e implementação SQL** de um cenário de **e-commerce** completo. O objetivo é aplicar conceitos de **modelagem de banco de dados relacional**, criando um **esquema funcional e normalizado**, com consultas SQL que demonstrem domínio de junções, filtros, agrupamentos e atributos derivados.

O desafio foi baseado nas diretrizes de um projeto educacional, com refinamentos que incluem:
- Clientes Pessoa Física (PF) e Pessoa Jurídica (PJ), com restrição para não coexistirem no mesmo registro.
- Múltiplas formas de pagamento por cliente.
- Entregas com status e código de rastreio.
- Relacionamento entre **vendedores** e **fornecedores** (permitindo sobreposição de funções).
- Controle de **estoque**, **fornecedores**, **produtos**, **pedidos** e **itens de pedido**.

## 🧩 Estrutura do Banco de Dados

**Schema:** `ecommerce`

**Principais tabelas:**
- `clients` — cadastro de clientes PF/PJ com validações por tipo.
- `addresses` — endereços de entrega e cobrança.
- `payments` — múltiplas formas de pagamento por cliente.
- `sellers` — vendedores que realizam as vendas.
- `suppliers` — fornecedores de produtos.
- `seller_supplier` — relação entre vendedor e fornecedor.
- `products` — catálogo de produtos.
- `product_supplier` — relacionamento N:N entre produtos e fornecedores.
- `stock` — controle de estoque.
- `orders` — pedidos realizados.
- `order_items` — itens vinculados a pedidos.
- `shipments` — informações de entrega e rastreamento.

## ⚙️ Criação do Banco de Dados
Execute o script `ecommerce_project.sql` no MySQL Workbench ou outro cliente SQL compatível.

```sql
DROP DATABASE IF EXISTS ecommerce;
CREATE DATABASE ecommerce;
USE ecommerce;
```

O script cria todas as tabelas com **chaves primárias, estrangeiras** e **constraints** de integridade, incluindo verificações para CPF/CNPJ exclusivas de cada tipo de cliente.

## 💾 Inserção de Dados de Exemplo
O script contém `INSERT INTO` com exemplos de:
- Clientes (PF e PJ)
- Produtos e fornecedores
- Pedidos com múltiplos itens
- Estoque e formas de pagamento
- Entregas com status e códigos de rastreio

Esses dados permitem testar todas as consultas propostas.

## 🧮 Consultas (Queries)
O projeto inclui consultas SQL que exploram:

### 🔹 Recuperações simples
```sql
SELECT idClient, client_type, fname, lname, email FROM clients;
```

### 🔹 Filtros (WHERE)
```sql
SELECT * FROM clients WHERE client_type = 'PF';
```

### 🔹 Atributos derivados
```sql
SELECT idOrder, (SELECT SUM(oi.unit_price * oi.quantity) FROM order_items oi WHERE oi.idOrder = o.idOrder) AS total
FROM orders o;
```

### 🔹 Agrupamentos e HAVING
```sql
SELECT c.fname, COUNT(o.idOrder) AS pedidos, SUM(o.total_amount) AS gasto_total
FROM clients c JOIN orders o ON c.idClient = o.idClient
GROUP BY c.fname
HAVING COUNT(o.idOrder) >= 1;
```

### 🔹 Junções complexas (JOINs)
```sql
SELECT p.product_name, s.supplier_name, st.quantity
FROM products p
JOIN product_supplier ps ON p.idProduct = ps.idProduct
JOIN suppliers s ON s.idSupplier = ps.idSupplier
JOIN stock st ON st.idProduct = p.idProduct;
```

### 🔹 Exemplos de perguntas respondidas
- Quantos pedidos foram feitos por cada cliente?
- Algum vendedor também é fornecedor?
- Relação de produtos, fornecedores e estoques.
- Relação de nomes dos fornecedores e produtos.
- Quais pedidos já possuem código de rastreio?
- Quais produtos estão com baixo estoque?

## 📈 Objetivo de Aprendizado
- Aplicar o **mapeamento de modelo conceitual → lógico → físico**.
- Praticar **DDL e DML**.
- Criar consultas SQL complexas e significativas.
- Documentar e compartilhar o projeto em um **repositório GitHub**.

## 🚀 Publicação no GitHub
1. Crie um repositório chamado `ecommerce-sql-model`.
2. Faça upload do arquivo `ecommerce_project.sql`.
3. Adicione este `README.md` na raiz do repositório.
4. Inclua prints de execução no Workbench (tabelas criadas e queries funcionando).

Exemplo de estrutura:
```
📦 ecommerce-sql-model
 ┣ 📜 ecommerce_project.sql
 ┣ 📜 README.md
 ┗ 📂 screenshots/
     ┣ resultado_queries.png
     ┗ estrutura_banco.png
```

## ✅ Conclusão
Este projeto consolida os principais conceitos de modelagem de bancos de dados relacionais e manipulação SQL em um cenário de **e-commerce completo e realista**, incluindo refinamentos práticos exigidos pelo desafio.

---
👨‍💻 **Autor:** Robespierre
📅 **Data:** 2025-10-06
📘 **Banco:** MySQL 8+
🌐 **Licença:** MIT
