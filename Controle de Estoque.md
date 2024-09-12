# Controle-de-Estoque

-- Criação do Banco de Dados
CRIAR BANCO DE DADOS SistemaControleEstoque;
USE SistemaControleEstoque;

-- Criação das Tabelas

-- Tabela de Fornecedores
CRIAR TABELA Fornecedores (
    id_fornecedor INT AUTO_INCREMENT CHAVE PRIMÁRIA,
    nome VARCHAR(100) NÃO NULO,
    endereco VARCHAR(255),
    telefone VARCHAR(20),
    e-mail VARCHAR(100)
);

-- Tabela de Produtos
CRIAR TABELA Produtos (
    id_produto INT AUTO_INCREMENT CHAVE PRIMÁRIA,
    nome_produto VARCHAR(100) NOT NULL,
    descrição TEXTO,
    quantidade_em_estoque INT NÃO NULO,
    preco DECIMAL(10,2) NÃO NULO,
    id_fornecedor INT,
    CHAVE ESTRANGEIRA (id_fornecedor) REFERÊNCIAS Fornecedores(id_fornecedor)
);

-- Tabela de Pedidos de Reposição
CREATE TABLE PedidosReposição (
    id_pedido INT AUTO_INCREMENT CHAVE PRIMÁRIA,
    id_produto INT,
    quantidade_pedida INT NOT NULL,
    data_pedido DATA NÃO NULA,
    status ENUM('Pendente', 'Concluído', 'Cancelado') DEFAULT 'Pendente',
    CHAVE ESTRANGEIRA (id_produto) REFERÊNCIAS Produtos(id_produto)
);

-- Inserção de Dados

-- Inserir Fornecedores
INSERIR EM Fornecedores (nome, endereco, telefone, email) VALORES
('Fornecedor A', 'Rua X, 123', '1111-2222', 'contato@fornecedora.com'),
('Fornecedor B', 'Avenida Y, 456', '3333-4444', 'contato@fornecedorB.com'),
('Fornecedor C', 'Praça Z, 789', '5555-6666', 'contato@fornecedorC.com');

-- Inserir Produtos
INSERT INTO Produtos (nome_produto, descrição, quantidade_em_estoque, preço, id_fornecedor) VALORES
('Produto 1', 'Descrição do Produto 1', 100, 10.50, 1),
('Produto 2', 'Descrição do Produto 2', 200, 15.75, 2),
('Produto 3', 'Descrição do Produto 3', 150, 7.30, 3);

-- Inserir Pedidos de Reposição
INSERT INTO PedidosReposicao (id_produto, quantidade_pedida, data_pedido, status) VALORES
(1, 50, '2024-08-01', 'Pendente'),
(2, 100, '2024-08-05', 'Concluído'),
(3, 30, '2024-08-10', 'Pendente');

-- Consultas

-- 1. Verifique os produtos em estoque
SELECIONAR
    p.nome_produto,
    p.quantidade_em_estoque,
    p.preco,
    f.nome AS nome_fornecedor
DE Produtos p
JOIN Fornecedores f ON p.id_fornecedor = f.id_fornecedor;

-- 2. Consultar pedidos de ajustes feitos
SELECIONAR
    pr.id_pedido,
    p.nome_produto,
    pr.quantidade_pedida,
    pr.data_pedido,
    status pr
DE PedidosReposicao pr
JOIN Produtos p ON pr.id_produto = p.id_produto;

-- 3. Informações sobre os fornecedores
SELECIONE * DE Fornecedores;

-- Atualizações

-- 1. Atualizar a quantidade de produtos em estoque após um novo pedido de pedido (exemplo: atualizar produto com id_produto = 1)
ATUALIZAÇÃO Produtos
SET quantidade_em_estoque = quantidade_em_estoque + 50
ONDE id_produto = 1;

-- 2. Modificar informações de um fornecedor (exemplo: atualizar telefone do fornecedor com id_fornecedor = 1)
ATUALIZAÇÃO Fornecedores
SET telefone = '9999-8888'
ONDE id_fornecedor = 1;

-- Exclusão de Dados

-- 1. Excluir produtos descontinuados (exemplo: remover produto com id_produto = 3)
EXCLUIR DE Produtos
ONDE id_produto = 3;

-- 2. Excluir fornecedores que não fazem mais parte da empresa (exemplo: remover fornecedor com id_fornecedor = 2)
EXCLUIR DE Fornecedores
ONDE id_fornecedor = 2;

-- 3. Cancelar pedidos de remessas necessárias (exemplo: cancelar pedido com id_pedido = 1)
ATUALIZAÇÃO PedidosReposicao
SET status = 'Cancelado'
ONDE id_pedido = 1;

-- 4. Remover pedidos de agendamento cancelados
EXCLUIR DE PedidosReposicao
ONDE status = 'Cancelado';
