# Resumo dos Comandos MySQL

Este arquivo serve como um guia rápido para os comandos mais comuns do MySQL.

---

## **1. Criação e Seleção de Banco de Dados**

### Criar um banco de dados:
````sql
CREATE DATABASE nome_do_banco;
````

#### Selecionar o banco de dados a ser utilizado:
````sql
USE nome_do_banco;
````

2. Tabelas

Criar uma tabela:


````sql
CREATE TABLE nome_da_tabela (
    coluna1 tipo_dado,
    coluna2 tipo_dado,
    ...
);
````

Exemplo de criação de uma tabela:

````sql
CREATE TABLE cliente (
    id_cliente INT AUTO_INCREMENT PRIMARY KEY,
    nome_cliente VARCHAR(255) NOT NULL,
    email VARCHAR(255),
    telefone VARCHAR(20)
);
````

Exibir a estrutura de uma tabela:

````sql
DESCRIBE nome_da_tabela;
````

3. Manipulação de Dados

Inserir dados na tabela:

````sql
INSERT INTO nome_da_tabela (coluna1, coluna2, ...)
VALUES (valor1, valor2, ...);
````

Exemplo:


````sql
INSERT INTO cliente (nome_cliente, email, telefone)
VALUES ('João Silva', 'joao@exemplo.com', '123456789');
````

Consultar dados (selecionar registros):

````sql
SELECT coluna1, coluna2 FROM nome_da_tabela; 
````
Exemplo:

````sql
SELECT nome_cliente, email FROM cliente; 
````

Atualizar dados:

````sql
UPDATE nome_da_tabela
SET coluna1 = valor1, coluna2 = valor2
WHERE condição; 
````
Exemplo:


UPDATE cliente
SET telefone = '987654321'
WHERE id_cliente = 1; 
````

Excluir dados:

````sql
DELETE FROM nome_da_tabela
WHERE condição; 
````

Exemplo:

````sql
DELETE FROM cliente
WHERE id_cliente = 1; 
````

4. Relacionamentos e Chaves

Adicionar uma chave primária:

````sql
ALTER TABLE nome_da_tabela
ADD PRIMARY KEY (coluna); 
```

Adicionar uma chave estrangeira:

````sql
ALTER TABLE nome_da_tabela
ADD CONSTRAINT fk_nome_da_fk
FOREIGN KEY (coluna)
REFERENCES tabela_referenciada(coluna_referenciada); 
````

5. Consultas Avançadas

Filtragem de dados:

````sql
SELECT coluna1, coluna2 FROM nome_da_tabela
WHERE condição;
````

Exemplo de filtro:

````sql
SELECT nome_cliente, email
FROM cliente
WHERE telefone = '123456789'; 
````

Ordenação dos resultados:

````sql
SELECT coluna1, coluna2 FROM nome_da_tabela
ORDER BY coluna1 [ASC|DESC]; 
````

Limitar a quantidade de resultados:

````sql
SELECT coluna1, coluna2 FROM nome_da_tabela
LIMIT número_de_resultados; 
````

6. Operações de Agregação

Contar registros:

````sql
SELECT COUNT(*) FROM nome_da_tabela; 
````

Somar valores:

````sql
SELECT SUM(coluna) FROM nome_da_tabela; 
````

Média de valores:

````sql
SELECT AVG(coluna) FROM nome_da_tabela;
 ````

Valor máximo ou mínimo:

````sql
SELECT MAX(coluna) FROM nome_da_tabela; 
SELECT MIN(coluna) FROM nome_da_tabela; 
````

7. Modificando Tabelas

Adicionar uma coluna:

````sql
ALTER TABLE nome_da_tabela
ADD coluna tipo_dado; 
````

Exemplo:

````sql
ALTER TABLE cliente
ADD endereco VARCHAR(255); 
````

Alterar o tipo de dado de uma coluna:

````sql
ALTER TABLE nome_da_tabela
MODIFY COLUMN coluna tipo_dado; 
````

Excluir uma coluna:

````sql
ALTER TABLE nome_da_tabela
DROP COLUMN coluna; 
````

Excluir uma tabela:

````sql
DROP TABLE nome_da_tabela; 
````

8. Backup e Restauração

Exportar banco de dados para um arquivo :

````bash

mydump -u usuario -p nome_do_banco > backup.
Restaurar banco de dados a partir de um arquivo :
bash


my -u usuario -p nome_do_banco < backup.

````

9. Consultas de Junção (Joins)

Junção entre tabelas (INNER JOIN):

````sql
SELECT t1.coluna1, t2.coluna2
FROM tabela1 t1
INNER JOIN tabela2 t2 ON t1.coluna = t2.coluna; 
````

Exemplo:

````sql
SELECT cliente.nome_cliente, produto.nome_produto
FROM cliente
INNER JOIN produto ON cliente.id_cliente = produto.id_cliente; 
````

## cláusula CONSTRAINT

A cláusula CONSTRAINT no My é usada para definir restrições (constraints) em colunas de uma tabela. As restrições podem ser usadas para impor regras de integridade e garantir que os dados inseridos ou modificados atendam a certas condições.

A cláusula CONSTRAINT pode ser usada para definir várias restrições, como chaves primárias (PRIMARY KEY), chaves estrangeiras (FOREIGN KEY), unique e check. A seguir, estão os tipos mais comuns de restrições que podem ser aplicadas com a cláusula CONSTRAINT:

1. Chave Primária (PRIMARY KEY)

A chave primária é usada para identificar de maneira única cada linha de uma tabela. Não pode haver valores nulos na coluna que é definida como chave primária.


````sql
CREATE TABLE cliente (
    id_cliente INT,
    nome_cliente VARCHAR(255),
    PRIMARY KEY (id_cliente)
)); 
````

Ou com a cláusula CONSTRAINT:

````sql
CREATE TABLE cliente (
    id_cliente INT,
    nome_cliente VARCHAR(255),
    CONSTRAINT pk_cliente PRIMARY KEY (id_cliente)
)); 
````

2. Chave Estrangeira (FOREIGN KEY)
A chave estrangeira é usada para criar um relacionamento entre duas tabelas, onde uma coluna (ou conjunto de colunas) em uma tabela faz referência à chave primária de outra tabela.

````sql
CREATE TABLE pedido (
    id_pedido INT,
    id_cliente INT,
    PRIMARY KEY (id_pedido),
    FOREIGN KEY (id_cliente) REFERENCES cliente(id_cliente)
); 
````

Ou com a cláusula CONSTRAINT:

````sql
CREATE TABLE pedido (
    id_pedido INT,
    id_cliente INT,
    CONSTRAINT pk_pedido PRIMARY KEY (id_pedido),
    CONSTRAINT fk_cliente FOREIGN KEY (id_cliente) REFERENCES cliente(id_cliente)
); 
````

3. Unique
A restrição UNIQUE garante que todos os valores em uma coluna sejam exclusivos (sem duplicatas), mas permite valores nulos.

````sql
CREATE TABLE cliente (
    id_cliente INT,
    email VARCHAR(255),
    CONSTRAINT unique_email UNIQUE (email)
); 
```` 


4. Check

A restrição CHECK assegura que os valores de uma coluna atendam a uma condição específica. Essa funcionalidade não é suportada em todas as versões do My, mas está disponível a partir do My 8.0.16.

````sql
CREATE TABLE produto (
    id_produto INT,
    nome_produto VARCHAR(255),
    preco DECIMAL(10, 2),
    CONSTRAINT check_preco CHECK (preco >= 0)
); 
````

5. Not Null

A restrição NOT NULL garante que uma coluna não aceite valores nulos. Isso pode ser feito diretamente ao criar a coluna ou usando CONSTRAINT.

````sql
CREATE TABLE cliente (
    id_cliente INT,
    nome_cliente VARCHAR(255) NOT NULL
); 
````

Ou com a cláusula CONSTRAINT:


````sql
CREATE TABLE cliente (
    id_cliente INT,
    nome_cliente VARCHAR(255),
    CONSTRAINT nn_nome_cliente CHECK (nome_cliente IS NOT NULL)
);
````

Exemplo Completo:

Aqui está um exemplo de como você pode usar a cláusula CONSTRAINT para criar um banco de dados com múltiplas restrições:

````sql
CREATE TABLE empresa (
    id_empresa INT AUTO_INCREMENT,
    nome_empresa VARCHAR(255) NOT NULL,
    cnpj VARCHAR(18) NOT NULL,
    PRIMARY KEY (id_empresa),
    CONSTRAINT unique_cnpj UNIQUE (cnpj)
); 

CREATE TABLE cliente (
    id_cliente INT AUTO_INCREMENT,
    nome_cliente VARCHAR(255) NOT NULL,
    email VARCHAR(255),
    id_empresa INT,
    PRIMARY KEY (id_cliente),
    FOREIGN KEY (id_empresa) REFERENCES empresa(id_empresa),
    CONSTRAINT unique_email UNIQUE (email)
); 
````

Neste exemplo:

A tabela empresa tem uma chave primária id_empresa e a restrição UNIQUE no cnpj.
A tabela cliente tem uma chave primária id_cliente, uma chave estrangeira que faz referência à tabela empresa e uma restrição UNIQUE no email.


# Atividade: Banco de Dados Relacional - Sistema de Gestão de Clientes
## Objetivo:
Praticar o conceito de banco de dados relacional criando tabelas relacionadas e realizando consultas SQL para atender a diferentes necessidades do sistema.

## Descrição do Sistema:
Você vai criar um banco de dados para gerenciar empresas, clientes, vendedores e produtos, além de um programa de indicação.

### Tabelas a serem criadas:
empresa:

id_empresa (PK, auto_increment)
nome_empresa (VARCHAR)
cnpj (VARCHAR)
data_criacao (DATE)
cliente:

id_cliente (PK, auto_increment)
nome_cliente (VARCHAR)
email (VARCHAR)
telefone (VARCHAR)
id_empresa (FK)
vendedor:

id_vendedor (PK, auto_increment)
nome_vendedor (VARCHAR)
comissao (DECIMAL)
id_empresa (FK)
produto:

id_produto (PK, auto_increment)
nome_produto (VARCHAR)
preco (DECIMAL)
id_empresa (FK)
indicacao:

id_indicacao (PK, auto_increment)
id_indicador (FK para cliente)
id_indicado (FK para cliente)
data_indicacao (DATE)

## Atividades:

### 1. Criação do Banco e das Tabelas
Escreva os scripts SQL para criar o banco de dados e as tabelas acima, considerando as chaves primárias e estrangeiras.

### 2. Inserção de Dados
Insira ao menos 3 registros em cada tabela, garantindo a consistência entre os dados relacionados.

### 3. Consultas Relacionais
Escreva as consultas SQL para:

Listar todas as empresas com seus respectivos clientes.
Obter todos os produtos vendidos por uma determinada empresa.
Listar os clientes que foram indicados por um cliente específico.
Calcular a comissão total de cada vendedor.
Atualização de Dados
Escreva um comando SQL para alterar o preço de um produto específico.

### 4. Exclusão de Dados
Escreva um comando SQL para excluir todos os clientes de uma empresa específica.
