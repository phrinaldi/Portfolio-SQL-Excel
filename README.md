# Portfolio-SQL-Excel

# Projeto SQL - Análise de Vendas

1- Este projeto utiliza SQL para extrair e consolidar dados de vendas a partir do banco de dados **AdventureWorks**. Os dados foram ajustados no Excel e apresentados em um dashboard para insights visuais.

## Arquivos
- **script.sql**: Código SQL para criar a view.
- **dados.xlsx**: Dados exportados e modificados no Excel.
- **dashboard.png**: Imagem do dashboard final.

## Como Reproduzir
1. Execute o script SQL no banco de dados AdventureWorks.
2. Analise e explore os dados no arquivo Excel.
3. Confira o dashboard para obter insights.

2- Download arquivo .bak AdventureWorks2014
/* https://docs.microsoft.com/pt-br/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms */ 

3- KPIs do projeto:
-- 3. Definindo os indicadores do projeto 
-- i) Total de Vendas Internet por Categoria 
-- ii) Receita Total Internet por Mês do Pedido 
-- iii) Receita e Custo Total Internet por País 
-- iv) Total de Vendas Internet por Sexo 
-- OBS: O ANO DE ANÁLISE SERÁ APENAS 2013 (ANO DO PEDIDO)

4- Análise das tabelas do Banco de Dados
SELECT * FROM FactInternetSales 
SELECT * FROM DimProductCategory 
SELECT * FROM DimSalesTerritory 
SELECT * FROM DimCustomer

-- TABELA 1: FactInternetSales
-- TABELA 2: DimCustomer
-- TABELA 3: DimSalesTerritory
-- TABELA 4: DimProductCategory ***
 
-- *** Aqui precisaremos fazer um relacionamento em cadeia

-- 5. Definindo as colunas da view VENDAS_INTERNET
 
-- VIEW FINAL VENDAS_INTERNET
 
-- Colunas:
-- SalesOrderNumber         (TABELA 1: FactInternetSales)
-- OrderDate                (TABELA 1: FactInternetSales)
-- EnglishProductCategoryName (TABELA 4: DimProductCategory)
-- FirstName + LastName     (TABELA 2: DimCustomer)
-- Gender                   (TABELA 2: DimCustomer)
-- SalesTerritoryCountry    (TABELA 3: DimSalesTerritory)
-- OrderQuantity            (TABELA 1: FactInternetSales)
-- TotalProductCost         (TABELA 1: FactInternetSales)
-- SalesAmount              (TABELA 1: FactInternetSales)

-- 6. Criando o código da view VENDAS_INTERNET
 
-- i) Total de Vendas Internet por Categoria do Produto
-- ii) Receita Total Internet por Mês do Pedido
-- iii) Receita e Custo Total Internet por País
-- iv) Total de Vendas Internet por Sexo do Cliente
 
-- OBS: O ANO DE ANÁLISE SERÁ APENAS 2013 (ANO DO PEDIDO)

CREATE OR ALTER VIEW VENDAS_INTERNET AS
SELECT
    fis.SalesOrderNumber AS 'Nº PEDIDO',
    fis.OrderDate AS 'DATA PEDIDO',
    dpc.EnglishProductCategoryName AS 'CATEGORIA PRODUTO',
    dc.FirstName + ' ' + dc.LastName AS 'NOME CLIENTE',
    Gender AS ‘Gênero’,
    dst.SalesTerritoryCountry AS 'PAÍS',
    fis.OrderQuantity AS 'QTD. VENDIDA',
    fis.TotalProductCost AS 'CUSTO VENDA',
    fis.SalesAmount AS 'RECEITA VENDA'
FROM FactInternetSales fis
INNER JOIN DimProduct dp ON fis.ProductKey = dp.ProductKey
INNER JOIN DimProductSubcategory dps ON dp.ProductSubcategoryKey = dps.ProductSubcategoryKey
INNER JOIN DimProductCategory dpc ON dps.ProductCategoryKey = dpc.ProductCategoryKey
INNER JOIN DimCustomer dc ON fis.CustomerKey = dc.CustomerKey
INNER JOIN DimSalesTerritory dst ON fis.SalesTerritoryKey = dst.SalesTerritoryKey
WHERE YEAR(OrderDate) = 2013;

-- 7. Conectando ao Excel

A conexão pode ser feita diretamente por meio do Power Query. Porém, faremos a cópia da tabela e o tratamento em Power Query
