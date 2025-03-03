# Banco de Dados para E-commerce

Este projeto consiste na modelagem e implementação de um banco de dados para um sistema de e-commerce. O banco de dados foi projetado para suportar operações de clientes, pedidos, produtos, pagamentos e entregas.

## 📌 Estrutura do Banco de Dados

### Entidades Principais:
- **Cliente** (PJ e PF)
- **Produto**
- **Fornecedor**
- **Pedido**
- **Item do Pedido**
- **Pagamento**
- **Entrega**
- **Estoque**
- **Vendedor**

### Relacionamentos:
- Um **cliente** pode fazer vários **pedidos**.
- Um **pedido** pode conter vários **produtos** (relação N:N com a entidade **item_pedido**).
- Um **produto** pode ter vários **fornecedores**.
- Um **fornecedor** pode fornecer vários **produtos**.
- Um **pedido** pode ter mais de uma forma de **pagamento**.
- Um **pedido** está associado a uma **entrega**, que tem um código de rastreamento e status.
- Um **vendedor** pode ser um **fornecedor**.

## 🛠️ Criação do Banco de Dados

### **Script de Criação das Tabelas**
O script a seguir cria as tabelas com as constraints necessárias:

```sql
CREATE TABLE cliente (
    id_cliente INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    tipo ENUM('PJ', 'PF') NOT NULL,
    cpf_cnpj VARCHAR(14) NOT NULL UNIQUE,
    endereco VARCHAR(255)
);

CREATE TABLE fornecedor (
    id_fornecedor INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    contato VARCHAR(100)
);

CREATE TABLE produto (
    id_produto INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    preco DECIMAL(10,2) NOT NULL
);

CREATE TABLE estoque (
    id_estoque INT PRIMARY KEY AUTO_INCREMENT,
    id_produto INT NOT NULL,
    quantidade INT NOT NULL,
    FOREIGN KEY (id_produto) REFERENCES produto(id_produto)
);

CREATE TABLE vendedor (
    id_vendedor INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL
);

CREATE TABLE pedido (
    id_pedido INT PRIMARY KEY AUTO_INCREMENT,
    id_cliente INT NOT NULL,
    data_pedido DATETIME DEFAULT CURRENT_TIMESTAMP,
    status ENUM('Pendente', 'Enviado', 'Entregue') NOT NULL,
    FOREIGN KEY (id_cliente) REFERENCES cliente(id_cliente)
);

CREATE TABLE item_pedido (
    id_item INT PRIMARY KEY AUTO_INCREMENT,
    id_pedido INT NOT NULL,
    id_produto INT NOT NULL,
    quantidade INT NOT NULL,
    preco_unitario DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (id_pedido) REFERENCES pedido(id_pedido),
    FOREIGN KEY (id_produto) REFERENCES produto(id_produto)
);

CREATE TABLE pagamento (
    id_pagamento INT PRIMARY KEY AUTO_INCREMENT,
    id_pedido INT NOT NULL,
    metodo ENUM('Cartão de Crédito', 'Boleto', 'Pix') NOT NULL,
    valor DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (id_pedido) REFERENCES pedido(id_pedido)
);

CREATE TABLE entrega (
    id_entrega INT PRIMARY KEY AUTO_INCREMENT,
    id_pedido INT NOT NULL,
    codigo_rastreio VARCHAR(20) NOT NULL,
    status ENUM('Em Transporte', 'Entregue', 'Cancelado') NOT NULL,
    FOREIGN KEY (id_pedido) REFERENCES pedido(id_pedido)
);
```

## 🔎 Inserção de Dados para Testes

```sql
-- Inserir clientes
INSERT INTO cliente (nome, tipo, cpf_cnpj, endereco) VALUES
('Empresa ABC', 'PJ', '12345678000199', 'Rua das Empresas, 100'),
('João Silva', 'PF', '12345678900', 'Av. Brasil, 200');

-- Inserir fornecedores
INSERT INTO fornecedor (nome, contato) VALUES
('Fornecedor X', 'fornecedorx@email.com'),
('Fornecedor Y', 'fornecedory@email.com');

-- Inserir produtos
INSERT INTO produto (nome, preco) VALUES
('Notebook', 3500.00),
('Smartphone', 2000.00);

-- Inserir estoque
INSERT INTO estoque (id_produto, quantidade) VALUES
(1, 50),
(2, 30);

-- Inserir vendedores
INSERT INTO vendedor (nome) VALUES
('Carlos Mendes');

-- Inserir pedidos
INSERT INTO pedido (id_cliente, status) VALUES
(1, 'Pendente'),
(2, 'Entregue');

-- Inserir itens do pedido
INSERT INTO item_pedido (id_pedido, id_produto, quantidade, preco_unitario) VALUES
(1, 1, 1, 3500.00),
(2, 2, 1, 2000.00);

-- Inserir pagamentos
INSERT INTO pagamento (id_pedido, metodo, valor) VALUES
(1, 'Cartão de Crédito', 3500.00),
(2, 'Pix', 2000.00);

-- Inserir entrega
INSERT INTO entrega (id_pedido, codigo_rastreio, status) VALUES
(2, 'BR123456789', 'Entregue');
```

## 📊 Queries SQL para Análise de Dados

### **1. Quantos pedidos foram feitos por cada cliente?**
```sql
SELECT c.nome, COUNT(p.id_pedido) AS total_pedidos
FROM cliente c
LEFT JOIN pedido p ON c.id_cliente = p.id_cliente
GROUP BY c.nome;
```

### **2. Algum vendedor também é fornecedor?**
```sql
SELECT v.nome
FROM vendedor v
JOIN fornecedor f ON v.nome = f.nome;
```

### **3. Relação de produtos, fornecedores e estoques**
```sql
SELECT p.nome AS produto, f.nome AS fornecedor, e.quantidade
FROM produto p
JOIN estoque e ON p.id_produto = e.id_produto
JOIN fornecedor f ON f.id_fornecedor = p.id_produto;
```

### **4. Relação de nomes dos fornecedores e nomes dos produtos**
```sql
SELECT f.nome AS fornecedor, p.nome AS produto
FROM fornecedor f
JOIN produto p ON p.id_produto = f.id_fornecedor;
```

## 🚀 Como Usar

1. Execute o script `schema.sql` para criar o banco.
2. Use `queries.sql` para realizar consultas.
3. Utilize um banco de dados MySQL ou PostgreSQL para rodar as queries.

🧩 **Dica:** Utilize ferramentas como MySQL Workbench, DBeaver ou pgAdmin para melhor visualização dos dados.

## 🚀 Tecnologias Utilizadas 
- ![MySQL](https://img.shields.io/badge/mysql-4479A1.svg?style=for-the-badge&logo=mysql&logoColor=white)
- ![Postgres](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white)
---
Projeto criado para fins educacionais, auxiliando no aprendizado de modelagem de bancos de dados relacionais. 📚
📌 **Criado por:** [Lucas Haigert Oliveira]
