# Modelagem de Dados para E-commerce com Power BI

Este documento apresenta uma descrição abrangente do processo de modelagem de dados, mantendo o foco nas etapas e decisões tomadas ao longo do desenvolvimento do projeto.

---

## Contexto

Este projeto foi desenvolvido como parte de um desafio no bootcamp da DIO em colaboração com a NTT Data, dentro do módulo "Modelando um Dashboard de E-commerce com Power BI Utilizando Fórmulas DAX".

A base de dados utilizada é a "finnancials", que foi disponibilizada pelo Power BI. As diretrizes do desafio estavam detalhadas em um [arquivo de texto](https://academiapme-my.sharepoint.com/:w:/g/personal/renato_dio_me/EW76WjPAA8RGgC3i44ofFq4BBiWzM-CN5S312YwOQCIwBA?e=7A6KfG). 

## Etapas de Transformação de Dados

1. **Ajuste de Tipos de Dados**: O primeiro passo consistiu na adequação dos tipos de dados da tabela original "finnancials", convertendo-os para formatos apropriados, como decimais fixos quando necessário.

2. **Criação de Tabelas Duplicadas**: A tabela original foi duplicada várias vezes para otimizar o processo de modelagem. A tabela original foi renomeada para "finnancials_origem" e oculta, de modo que as cópias processadas fossem utilizadas na análise.

3. **Tabela `D_Produtos`**: A primeira duplicata foi nomeada "D_Produtos". Nesta tabela, foram realizadas análises avançadas com base na coluna "Product":
   - Contagem de itens por produto.
   - Cálculo dos valores máximo e mínimo de vendas.
   - Cálculo da média e mediana dos valores de vendas.
   - Cálculo da média do preço de fabricação.
   Ao final, foi adicionada uma coluna de índice chamada "id_produto" para identificação única de cada produto.

4. **Tabela `D_Descontos`**: Na segunda duplicata, foram mantidas apenas as colunas "Product", "Discount Band" e "Discounts". Os valores de "Product" foram substituídos pelos respectivos IDs provenientes da tabela "D_Produtos". Após ordenar a tabela e adicionar uma coluna de índice, a coluna "id_descontos" foi criada.

5. **Tabela `D_Produtos_Detalhes`**: A terceira duplicata, renomeada para "D_Produtos_Detalhes", preservou as colunas essenciais, incluindo "Product", "Units Sold", "Manufacturing Price", "Sale Price" e "Discount Band". Uma nova coluna foi inserida para calcular o valor total da venda usando a fórmula: `Total Sale Price = Sale Price * Units Sold`. A coluna de índice "id_produto_detalhes" foi adicionada e os IDs de produtos foram ajustados conforme a "D_Produtos".

6. **Tabela `D_Detalhes`**: A quarta tabela duplicada recebeu o nome "D_Detalhes" e manteve colunas relevantes para análises detalhadas, como "Segment", "Country", "Product", "Gross Sales", "Sales", "COGS" e "Profit". Uma coluna de índice "id_detalhes" foi incluída para permitir a identificação única de cada registro.

7. **Tabela Fato `F_Vendas`**: A tabela de fato "F_Vendas" foi simplificada ao remover colunas desnecessárias para a análise, como "COGS", "Month Number", "Manufacturing Price" e "Gross Sales". Uma chave substituta ("SK_id") foi adicionada para identificação. Para definir as chaves estrangeiras, as colunas foram organizadas de acordo com as tabelas de dimensão, permitindo a criação de relacionamentos baseados em índices únicos para `id_detalhes`, `id_descontos` e `id_produto_detalhes`.

8. **Tabela de Datas `D_Data`**: Uma tabela de datas foi criada em DAX, abrangendo o período de 2013 a 2014, que é o intervalo contido nos dados. Essa tabela é crucial para análises temporais e foi estruturada com as seguintes colunas:
   - "Data" no formato "dd/mm/yyyy".
   - "Ano", "Mês" e "Nome do Mês", facilitando análises ao longo do tempo.

## Modelo de Relacionamentos

Após a criação das tabelas, os relacionamentos automáticos foram desativados para que um esquema em estrela pudesse ser configurado manualmente. As tabelas foram conectadas da seguinte maneira:

- A tabela `F_Vendas` foi vinculada às tabelas de dimensão (`D_Produtos`, `D_Descontos`, `D_Detalhes`, `D_Produtos_Detalhes` e `D_Data`) utilizando chaves estrangeiras, o que melhora a eficiência das consultas e a precisão das métricas no dashboard.
- A tabela original "finnancials_origem" foi oculta para evitar sobrecarga no modelo visual, mantendo a estrutura organizada e focada nas tabelas relevantes.

Este modelo facilita a visualização e a análise de dados de e-commerce, possibilitando a criação de relatórios dinâmicos e analíticos no Power BI, com um layout consistente para a exploração de métricas significativas.

---
