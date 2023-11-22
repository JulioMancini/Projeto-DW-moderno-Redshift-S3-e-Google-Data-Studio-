# DW Moderno e Data Lake - Redshift - S3 - Google Data Studio
Projeto de implementação de um DW Moderno utilizando o Redshift 


### ROTEIRO DO PROJETO

* Criar um Cluster no Redshif
* Criar banco de dados e tabelas
* Criar bucket no Amazon S3
* Upload de arquivo
* Criar Credenciais
* Carregar dados usando o Copy
* Crirar tabela Desnormalizada
* Configurar Redshif para acesso público
* Conectar ao Google Data studio
* Criar Dashboard " ad hoc"

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

### Criar e configurar um Bucket (S3)

* Então, através de um Bucket, você consegue criar um repositório para importar os arquivos.
* entre no site da AWS para criar: https://aws.amazon.com/pt/s3/
* crie um nome unico para o Bucket e mude a região para São Paulo

![dw8](https://github.com/JulioMancini/Projeto-DW-moderno-Redshift-S3-e-Google-Data-Studio-/assets/145502330/d83b9448-67cf-46d5-8268-a893c08dc965)

* Com o Bucket criado pode Carregar os arquivos

![dw 13](https://github.com/JulioMancini/Projeto-DW-moderno-Redshift-S3-e-Google-Data-Studio-/assets/145502330/50a09ada-776b-41d8-aaa3-7d5198cc44ee)

  
* Então, pode clicar no menu, à direita, e você vai ir em Credenciais de Segurança. Criar uma chave de acesso.

![dw 10](https://github.com/JulioMancini/Projeto-DW-moderno-Redshift-S3-e-Google-Data-Studio-/assets/145502330/9f2bdbd9-e917-409e-a9b8-96d5384c9e23)

### Carregar dados usando o Copy


```bash
copy Clientes
from 'endereço da pasta S3'
credentials 'aws_access_key_id= meu id;aws_secret_access_key=chave'            '
region 'sa-east-1'
delimiter ;
IGNOREHEADER 1
DATEFORMAT 'DD/MM/YYYY';
```

![dw 12](https://github.com/JulioMancini/Projeto-DW-moderno-Redshift-S3-e-Google-Data-Studio-/assets/145502330/71de177a-42eb-41f4-adb4-cab4f6a60a49)

* repetir o processo com os outros arquivos do bucket, com o copy em todos os arquivos podemos fazer consultas SQL.

### Criando tabela Desnormalizada

* Então agora vou criar uma tabela desnormalizada, que traga o nome do cliente, a data da venda, o nome do vendedor, o produto, a quantidade e o total da venda.

```bash
select clientes, data, nome as vendedor, produto, quantidade, total
into fatovendas
from vendas v
inner join clientes c on (c.idcliente = v.idcliente)
inner join itensvenda i on (i.idvenda = v.idvenda)
inner join produtos p on (p.idproduto = i.idproduto)
inner join vendedores vn on (vn.idvendedor = v.idvendedor)
```
### Configurar Redshif para acesso público

* Por padrão o Redshift não vai estar disponivel publicamente

![dw 15](https://github.com/JulioMancini/Projeto-DW-moderno-Redshift-S3-e-Google-Data-Studio-/assets/145502330/3c722821-7c41-44fc-98b0-0966943f6522)

* botão Ações ou eu vou clicar nele e vejo aqui o modificar a configuração publicamente
* Então, tem que ir no botão ações e na opção: Modificar a configuração publicamente acessível

![dw16](https://github.com/JulioMancini/Projeto-DW-moderno-Redshift-S3-e-Google-Data-Studio-/assets/145502330/88d749e6-0f91-4518-ab75-8b2658dc85b0)

* É impotante salvar o andpoint para usar no Google Data studio

### Conectar ao Google Data studio e Criar Dashboard

* Para ultilizar o Google Data Studio basta entrar no site: https://lookerstudio.google.com/

* Clicar no botão de coneção com o Redshift

![dw 17](https://github.com/JulioMancini/Projeto-DW-moderno-Redshift-S3-e-Google-Data-Studio-/assets/145502330/759d81f4-88b4-451f-a11d-078836459afd)

* Colocar o endpoint
* Importante remover a porta e o banco de dados dev, deixando só o endereço do cluster.

1. IP ou nome do hoat

`redshift-cluster-1-cmb0hv82ssgs-us-esst-1-redshift.amazonaws.com` 

2. porta

`5439`

3. Banco de dados

`ed`

4. nome do usuário

`awsuser`

5. senha 

`12345678aA` 

* após autenticar podemos clicar na tabela desnormalizada que criamos "fatovendas"

![dw 18](https://github.com/JulioMancini/Projeto-DW-moderno-Redshift-S3-e-Google-Data-Studio-/assets/145502330/c2f564a0-ac7b-4dc7-be8e-08b15c7210cb)


