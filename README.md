# 💻 Projeto de Banco de Dados - Modelos Conceituais e Lógicos

Projeto desenvolvido para a disciplina Projeto Banco de Dados do 3 período de ADS, onde trabalhamos com modelos conceituais e lógicos para representar e analisar um sistema de gerenciamento de clientes e pedidos.

## 🙋 Equipe

- **Angelo Santos** - Matrícula: 01707596
- **Eduardo Henrique** - Matrícula: 01706530
- **Euclides Neto** - Matrícula: 01696172
- **Gabriel Teixeira** - Matrícula: 01413025
- **Ingrid Larissa** - Matrícula: 01552364
- **Klebson Apolinário** - Matrícula: 01704173
- **Samara Jovino** - Matrícula: 01711332

## 🔍 Modelo Conceitual

O modelo conceitual foi desenvolvido para representar as entidades e seus relacionamentos no sistema. Abaixo está uma breve descrição das entidades principais:

- **Cliente**: Armazena informações como nome, CPF, telefone, email e endereço.
- **Endereco**: Contém detalhes do endereço do cliente.
- **Pedido**: Registra os pedidos feitos pelos clientes, incluindo detalhes de pagamento.
- **Produto**: Contém informações sobre os produtos disponíveis, como nome, preço e estoque.

### ➰ Relacionamentos

- **Cliente-Endereco**: Cada cliente possui um endereço associado.
- **Cliente-Pedido**: Um cliente pode fazer vários pedidos, mas cada pedido está associado a um único cliente.
- **Pedido-Endereco**: Cada pedido está associado a um endereço de entrega.
- **Pedido-Produto**: Um pedido pode conter vários produtos, e um produto pode estar em vários pedidos.

## 📝 Modelo Lógico

O modelo lógico foi criado com base no modelo conceitual, definindo as tabelas e colunas que serão implementadas no banco de dados. As tabelas principais incluem:

- **Cliente**: `ClientelD (PrimaryKey)`, `Nome`, `CPF`, `Telefone`, `Email`, `EnderecoID(ForeignKey)`
- **Endereco**: `EnderecoID (PrimaryKey)`, `CEP`, `ClientelD (ForeignKey)`
- **Pedido**: `RedidoID (PrimaryKey)`, `ClientelD (ForeignKey)`, `EnderecoID (ForeignKey)`, `ProdutoID (ForeignKey)`,  `Pagamento`
- **Produto**: `ProdutoID (PrimaryKey)`, `Nome`, `Preco`, `Estoque`


## 🔗 Links

- **Modelo Conceitual** : https://app.brmodeloweb.com/#!/publicview/67d4b21550a65ecc6d275028
-  **Modelo Lógico** : https://app.brmodeloweb.com/#!/publicview/67de179ea403760776b15254
-  **Modelo Fisico** : [Uploacreate database modelofisico;

CREATE TABLE cliente(
  id_cliente int primary key auto_increment,
  nome varchar(50),
  email varchar(40),
  telefone varchar(14),
  senha varchar(15),
  cpf varchar(24),
  id_endereco int
);

create table pedido(
 pedido_id int primary key auto_increment,
 quantidade int,
 pagamento varchar(50),
 cliente_id int,
 endereco_id int,
 produto_id int,
 foreign key (produto_id) references produto(id_produto)
);

create table endereco(
endereco_id int primary key auto_increment,
cep varchar(9),
cliente_id int,
FOREIGN KEY (cliente_id) REFERENCES cliente(id_cliente)
);

create table produto(
id_produto int primary key auto_increment,
nome varchar(40),
estoque int,
preco float
);

update pedido
set produto_id = '1'
where pedido_id = '1';

update pedido
set produto_id = '2'
where pedido_id = '2';

#Inserindo dados CLIENTES
 #Ex:
 
 insert into cliente (nome, email, telefone, senha, cpf, id_endereco)
 values ('Euclides Neto', 'euclidesdacunha@hotmail.com', '8194836922', 'mortandela3', '1001040002-06', '4');
 
 INSERT INTO cliente (nome, email, telefone, senha, cpf, id_endereco) 
VALUES ('Gabriel', 'gabrielteixeira@hotmail.com', '8198989989', 'mortandela1', '1001030002-00', '2');

 INSERT INTO cliente (nome, email, telefone, senha, cpf, id_endereco) 
VALUES ('Klebson', 'binho123@hotmail.com', '8198980089', 'mortandela2', '1001030002-01', '3');



#Inserindo dados PEDIDOS
#Ex:

insert into pedido (quantidade, pagamento, cliente_id, endereco_id)
values ('3', 'Debito', '3', '2');
 
 
 #Inserindo dados PRODUTOS
 #Ex:
 
insert into produto (nome, estoque, preco)
values ('VinhoCarreteiro', '392', '12.90');

insert into produto (nome, estoque, preco)
values ('Reservado', '160', '250');

DELETE FROM produto
WHERE id_produto = '3';


 #Inserindo dados ENDERECOS

insert into endereco (cep)
values ('12345-300');

#Consultas Simples
select * from cliente;
describe cliente;

select * from endereco;
describe endereco;

select * from pedido;
describe pedido;

select * from produto;
describe produto;

show tables;

#Consultas elaboradas

#Seleciona o Nome dos clientes que não fizeram pedido
select nome from cliente
where id_cliente not in (select cliente_id from pedido);

#Seleciona a quantidade de pedidos até agora
SELECT COUNT(*) AS numero_de_pedidos
FROM pedido
WHERE cliente_id < 3;

#Quantidade de produtos vendidos
SELECT pr.nome AS nome_produto, 
p.quantidade FROM pedido p
JOIN produto pr ON p.produto_id = pr.id_produto
WHERE p.pedido_id = 2;

#Listar todos os produtos com nome e preço, ordenados pelo nome:
SELECT nome, preco
FROM produto
ORDER BY nome;

#Listar os clientes que possuem um endereço em um CEP específico
SELECT c.nome, e.CEP
FROM cliente c
JOIN endereco e ON c.id_endereco = e.endereco_id
WHERE e.CEP = '12345-300';

#Listar os produtos com estoque abaixo de um determinado limite (ex: 200 unidades)
SELECT nome, estoque
FROM produto
WHERE estoque < 200;

#Mostrar o total faturado por cada produto (quantidade * preço)
SELECT pr.nome AS nome_produto, 
       SUM(p.quantidade * pr.preco) AS total_faturado
FROM pedido p
JOIN produto pr ON p.produto_id = pr.id_produto
GROUP BY pr.nome;

#Listar os pedidos com produtos acima de um determinado valor (ex: R$ 100,00)
SELECT p.pedido_id, pr.nome AS produto, pr.preco
FROM pedido p
JOIN produto pr ON p.produto_id = pr.id_produto
WHERE pr.preco > 100;

#Calcular a média de preço dos produtos
SELECT AVG(preco) AS preco_medio
FROM produto;

#Contar quantos pedidos existem para cada cliente
SELECT c.nome, COUNT(p.pedido_id) AS total_pedidos
FROM cliente c
LEFT JOIN pedido p ON c.id_cliente = p.cliente_id
GROUP BY c.id_cliente;


ding Modoelo Fisico WineWine.sql…]()


OBS.: Os modelos também estão em PDF no repositório! 😊
