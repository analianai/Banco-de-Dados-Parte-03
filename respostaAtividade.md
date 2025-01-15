## Resposta

#### Criação de tabelas

````sql
-- Criar o banco de dados
CREATE DATABASE SistemaGestaoClientes;
USE SistemaGestaoClientes;

-- Tabela empresa
CREATE TABLE empresa (
    id_empresa INT AUTO_INCREMENT PRIMARY KEY,
    nome_empresa VARCHAR(255) NOT NULL,
    cnpj VARCHAR(18) NOT NULL,
    data_criacao DATE NOT NULL
);

-- Tabela cliente
CREATE TABLE cliente (
    id_cliente INT AUTO_INCREMENT PRIMARY KEY,
    nome_cliente VARCHAR(255) NOT NULL,
    email VARCHAR(255),
    telefone VARCHAR(20),
    id_empresa INT,
    FOREIGN KEY (id_empresa) REFERENCES empresa(id_empresa) ON DELETE CASCADE
);

-- Tabela vendedor
CREATE TABLE vendedor (
    id_vendedor INT AUTO_INCREMENT PRIMARY KEY,
    nome_vendedor VARCHAR(255) NOT NULL,
    comissao DECIMAL(10, 2),
    id_empresa INT,
    FOREIGN KEY (id_empresa) REFERENCES empresa(id_empresa) ON DELETE CASCADE
);

-- Tabela produto
CREATE TABLE produto (
    id_produto INT AUTO_INCREMENT PRIMARY KEY,
    nome_produto VARCHAR(255) NOT NULL,
    preco DECIMAL(10, 2),
    id_empresa INT,
    FOREIGN KEY (id_empresa) REFERENCES empresa(id_empresa) ON DELETE CASCADE
);

-- Tabela indicacao
CREATE TABLE indicacao (
    id_indicacao INT AUTO_INCREMENT PRIMARY KEY,
    id_indicador INT,
    id_indicado INT,
    data_indicacao DATE NOT NULL,
    FOREIGN KEY (id_indicador) REFERENCES cliente(id_cliente) ON DELETE CASCADE,
    FOREIGN KEY (id_indicado) REFERENCES cliente(id_cliente) ON DELETE CASCADE
);

````

#### Inserção de Dados

````sql
-- Inserir empresas
INSERT INTO empresa (nome_empresa, cnpj, data_criacao)
VALUES 
    ('Empresa A', '11.111.111/0001-11', '2023-01-01'),
    ('Empresa B', '22.222.222/0001-22', '2023-06-01');

-- Inserir clientes
INSERT INTO cliente (nome_cliente, email, telefone, id_empresa)
VALUES 
    ('Cliente 1', 'cliente1@empresa.com', '123456789', 1),
    ('Cliente 2', 'cliente2@empresa.com', '987654321', 1),
    ('Cliente 3', 'cliente3@empresa.com', '543216789', 2);

-- Inserir vendedores
INSERT INTO vendedor (nome_vendedor, comissao, id_empresa)
VALUES 
    ('Vendedor 1', 0.05, 1),
    ('Vendedor 2', 0.10, 2);

-- Inserir produtos
INSERT INTO produto (nome_produto, preco, id_empresa)
VALUES 
    ('Produto A', 100.00, 1),
    ('Produto B', 200.00, 1),
    ('Produto C', 300.00, 2);

-- Inserir indicações
INSERT INTO indicacao (id_indicador, id_indicado, data_indicacao)
VALUES 
    (1, 2, '2024-01-01'),
    (1, 3, '2024-01-05');

````

#### Consultas Relacionais

a) Listar todas as empresas com seus respectivos clientes

````sql
SELECT e.nome_empresa, c.nome_cliente
FROM empresa e
LEFT JOIN cliente c ON e.id_empresa = c.id_empresa;
````

b) Obter todos os produtos vendidos por uma determinada empresa

````sql
SELECT p.nome_produto, p.preco
FROM produto p
WHERE p.id_empresa = 1;
````

c) Listar os clientes que foram indicados por um cliente específico

````sql
SELECT c.nome_cliente AS Indicador, ci.nome_cliente AS Indicado
FROM indicacao i
JOIN cliente c ON i.id_indicador = c.id_cliente
JOIN cliente ci ON i.id_indicado = ci.id_cliente
WHERE c.id_cliente = 1;
````

d) Calcular a comissão total de cada vendedor

````sql
SELECT v.nome_vendedor, SUM(p.preco * v.comissao) AS comissao_total
FROM vendedor v
JOIN produto p ON v.id_empresa = p.id_empresa
GROUP BY v.id_vendedor;
````

#### Atualização de Dados

````sql
-- Atualizar o preço de um produto específico
UPDATE produto
SET preco = 120.00
WHERE id_produto = 1;
````

#### Exclusão de Dados

````sql
-- Excluir todos os clientes de uma empresa específica
DELETE FROM cliente
WHERE id_empresa = 1;
````