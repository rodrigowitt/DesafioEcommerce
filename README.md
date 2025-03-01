Modelo Conceitual

- Cliente (PJ ou PF)
    - ID (chave primária)
    - Nome
    - Endereço
    - Telefone
    - Tipo (PJ ou PF)
- Vendedor
    - ID (chave primária)
    - Nome
    - Endereço
    - Telefone
- Fornecedor
    - ID (chave primária)
    - Nome
    - Endereço
    - Telefone
- Produto
    - ID (chave primária)
    - Nome
    - Descrição
    - Preço
    - Estoque
- Pedido
    - ID (chave primária)
    - ID Cliente (chave estrangeira)
    - Data
    - Total
- Item Pedido
    - ID (chave primária)
    - ID Pedido (chave estrangeira)
    - ID Produto (chave estrangeira)
    - Quantidade
- Pagamento
    - ID (chave primária)
    - ID Pedido (chave estrangeira)
    - Tipo (cartão, boleto, etc.)
    - Data
- Entrega
    - ID (chave primária)
    - ID Pedido (chave estrangeira)
    - Status
    - Código de rastreio

Modelo Lógico


CREATE TABLE Cliente (
  ID INT PRIMARY KEY,
  Nome VARCHAR(100),
  Endereço VARCHAR(200),
  Telefone VARCHAR(20),
  Tipo VARCHAR(2) CHECK (Tipo IN ('PJ', 'PF'))
);

CREATE TABLE Vendedor (
  ID INT PRIMARY KEY,
  Nome VARCHAR(100),
  Endereço VARCHAR(200),
  Telefone VARCHAR(20)
);

CREATE TABLE Fornecedor (
  ID INT PRIMARY KEY,
  Nome VARCHAR(100),
  Endereço VARCHAR(200),
  Telefone VARCHAR(20)
);

CREATE TABLE Produto (
  ID INT PRIMARY KEY,
  Nome VARCHAR(100),
  Descrição VARCHAR(200),
  Preço DECIMAL(10, 2),
  Estoque INT
);

CREATE TABLE Pedido (
  ID INT PRIMARY KEY,
  ID_Cliente INT,
  Data DATE,
  Total DECIMAL(10, 2),
  FOREIGN KEY (ID_Cliente) REFERENCES Cliente(ID)
);

CREATE TABLE Item_Pedido (
  ID INT PRIMARY KEY,
  ID_Pedido INT,
  ID_Produto INT,
  Quantidade INT,
  FOREIGN KEY (ID_Pedido) REFERENCES Pedido(ID),
  FOREIGN KEY (ID_Produto) REFERENCES Produto(ID)
);

CREATE TABLE Pagamento (
  ID INT PRIMARY KEY,
  ID_Pedido INT,
  Tipo VARCHAR(20),
  Data DATE,
  FOREIGN KEY (ID_Pedido) REFERENCES Pedido(ID)
);

CREATE TABLE Entrega (
  ID INT PRIMARY KEY,
  ID_Pedido INT,
  Status VARCHAR(20),
  Código_de_rastreio VARCHAR(20),
  FOREIGN KEY (ID_Pedido) REFERENCES Pedido(ID)
);


Queries SQL

1. Quantos pedidos foram feitos por cada cliente?


SELECT C.Nome, COUNT(P.ID) AS Quantidade_de_Pedidos
FROM Cliente C
JOIN Pedido P ON C.ID = P.ID_Cliente
GROUP BY C.Nome;


2. Algum vendedor também é fornecedor?


SELECT V.Nome
FROM Vendedor V
JOIN Fornecedor F ON V.ID = F.ID;


3. Relação de produtos, fornecedores e estoques?


SELECT P.Nome, F.Nome, P.Estoque
FROM Produto P
JOIN Fornecedor F ON P.ID_Fornecedor = F.ID;


4. Relação de nomes dos fornecedores e nomes dos produtos?


SELECT F.Nome, P.Nome
FROM Fornecedor F
JOIN Produto P ON F.ID = P.ID_Fornecedor;


