# DW Moderno e Data Lake - Redshift - S3 - Google Data Studio

### ROTEIRO DO PROJETO

* Criar um Cluster no Redshif
* Criar banco de dados e tabelas
* Criar bucket no Amazon S3
* Upload de arquivo
* Criar Credenciais
* Carregar dados usando o Copy
* Crirar tabela Desnormalizada
* Configurar Redshif para acesso público
*  Conectar ao Google Data studio
*  Criar Dashboard " ad hoc"

### MODELO DE DADOS

![modelo dw moderno](https://github.com/JulioMancini/a/assets/145502330/9ef80ddb-e721-41ff-a0b6-f7ce3685e6ef)

#### CRIANDO INSTÂNCIA NO REDSHIFT

* primeiro passo e criar uma conta no site da WS e lá vamos acessar o Redshift por uma versão free tier, ou seja, que não vai ter custo deste cluster. https://aws.amazon.com/pt/redshift/

![dw 1](https://github.com/JulioMancini/a/assets/145502330/9733465d-f52d-4c40-86e8-b483ce15fb82)

![dw 3](https://github.com/JulioMancini/a/assets/145502330/8c848cf0-e665-436e-b372-e83935cd2c18)

* Crie uma senha do usuário Administrador e crie o Cluster
  
![dw 4](https://github.com/JulioMancini/a/assets/145502330/367ae8a0-0447-4505-9ee9-a9d0bca2c65c)

### Criando tabelas

* No Editor de consultas, conecte o Cluster clicando nele

![d 5](https://github.com/JulioMancini/a/assets/145502330/e4b930e8-2f02-4b76-93e1-c50433d0ac1c)

* Criar um banco de dados
```bash
create database ed;
```

* Agora vamos abrir uma nova guia, é importante se certificar que está conectado ao banco de dados na nova guia, em seguida vamos criar as tabalas

![dw 6](https://github.com/JulioMancini/a/assets/145502330/dd62b1ec-d8cb-4336-908d-b46d1430850b)

```bash
set datestyle to 'SQL,DMY';

CREATE TABLE Vendedores(
  IDVendedor int  PRIMARY KEY,
  Nome Varchar(50)
);

CREATE TABLE Produtos(
  IDProduto int  PRIMARY KEY,
  Produto Varchar(100),
  Preco Numeric(10,2)
);

CREATE TABLE Clientes(
  IDCliente int   PRIMARY KEY,
  Cliente Varchar(50),
  Estado Varchar(2),
  Sexo Char(1),
  Status Varchar(50)
);

CREATE TABLE Vendas(
  IDVenda int   PRIMARY KEY,
  IDVendedor int references Vendedores (IDVendedor),
  IDCliente int references Clientes (IDCliente),
  Data Date,
  Total Numeric(10,2)
);

CREATE TABLE ItensVenda (
    IDProduto int ,
    IDVenda int ,
    Quantidade int,
    ValorUnitario decimal(10,2),
    ValorTotal decimal(10,2),
	Desconto decimal(10,2),
    PRIMARY KEY (IDProduto, IDVenda)
);
```










