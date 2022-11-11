[![author](https://img.shields.io/badge/author-Amanda-red.svg)](https://www.linkedin.com/in/amandamagalhaesantonio/) [![](https://img.shields.io/badge/python-3.7+-blue.svg)](https://www.python.org/downloads/release/python-365/) ![GPLv3 license](https://img.shields.io/badge/License-GPLv3-blue.svg)


# 1.0	– Questão de negócio.

A Key Real Estate é uma plataforma de vendas de imóveis. Seu modelo de negócio é comprar bons imóveis e depois vende-los por um preço mais alto, para maximizar seu lucro.
Atualmente a equipe da empresa está tendo dificuldades na tomada de decisão para encontrar bons negócios de compra e venda. O portfólio é muito grande e consome muito para realizações de análises manuais.
O CEO resolveu contratar um analista de dados para auxilia-los a encontrar bons negócios para a empresa.

Problema de Negócio
Quais imóveis que a empresa deveria comprar e por qual preço?
Uma vez o imóvel comprado, qual o melhor momento para vende-lo e por qual preço?


# 2.0	– Premissas de negócio

### Dicionário de Dados

id - ID exclusivo para cada imóvel.

date - Data em que o imóvel ficou disponível para compra.

price - Preço de cada imóvel.

bedrooms - Número de quartos.

bathrooms - Número de banheiros, onde 0,5 representa um banheiro com vaso sanitário, mas sem chuveiro.

sqft_living - Metragem do espaço interior dos imóveis.

sqft_lot - Metragem do espaço terrestre.

floors - Número de andares

waterfront - Uma variável fictícia para saber se o imóvel estava com vista para a água.

view - Um índice de 0 a 4 de quão boa era a vista da propriedade 0 = Sem vista, 1 = Razoável 2 = Média, 3 = Boa, 4 = Excelente.

condition - Um índice de 1 a 5 sobre a condição do imóvel, 1 = Muito Ruim, 2 = Ruim, 3 = Médio, 4 = Bom, 5 = Muito bom.

grade - Um índice de 1 a 13, onde 1-3 fica aquém da construção e projeto do edifício, 7 tem um nível médio de construção e projeto, e 11-13 tem um alto nível de qualidade de construção e projeto.

sqft_above - A metragem quadrada do espaço interno da habitação que está acima do nível do solo.

sqft_basement - A metragem quadrada do espaço interno da habitação que está abaixo do nível do solo.

yr_built - O ano em que a casa foi construída.

yr_renovated - O ano da última reforma da casa.

zipcode - É área de código postal do imóvel.

lat - Latitude

long - Longitude

sqft_living15 - A metragem do espaço habitacional interior para os 15 vizinhos mais próximos.

sqft_lot15 - A metragem dos lotes dos 15 vizinhos mais próximos.

### Manipulação dos Dados

Na coluna bathrooms - Número de banheiros, o valor 0,5 representa um banheiro com vaso sanitário, mas sem chuveiro. E foi mantido desde que o imóvel tivesse um banheiro completo.

Foi considerado que imóveis com valor igual a 0 sejam espaços como studios ou kitnets.

Foi excluindo imóveis com bathrooms = 0

Foi excluindo imóveis com bedrooms = 0 e com vários andares (floors)

Foi mantido banheiro 0.75. Que um banheiro completo contendo um chuveiro ou uma uma banheira.

Foi mantido banheiro 0.25. Que um é banheiro apenas com vaso sanitário desde que o imóvel tivesse um banheiro completo. Exemplo: 1.25 ou 2.25.

O imóvel com valor de 33 bedrooms, foi considerado erro de digitação e foi convertido para um imóvel de 3 quartos. 

Imóveis que apresentaram o bathroom apenas um lavado (0,5) para todo o imóvel foi excluído.

Foi criada uma coluna chamada: “condition_type”

- Se o valor da coluna “condition” for menor ou igual à 2 = bad
- Se o valor da coluna “condition” for igual à 3 ou 4 = regular
- Se o valor da coluna “condition” for igual à 5 = good

 Foi criada uma coluna chamada “dormitory_type”
 
- Se o valor da coluna “bedrooms” for igual à 0 = studio
- Se o valor da coluna “bedrooms” for igual à 1 e 2 = apartment
- Se o valor da coluna “bedrooms” for maior ou igual à 3 = house

Foi criada uma nova coluna chamada: “house_age”

- Se o valor da coluna “date” for maior que 2014-01-01 = “new_house”
- Se o valor da coluna “date” for menor que 2014-01-01 = “old_house”


Foi criada uma classificação para os imóveis, separando-os em baixo padrão e alto padrão, pelo preço.

- Acima de 540.000 = alto padrão
- Abaixo de 540.000 = baixo padrão

Foi criada uma coluna nivel de acordo com o preço

- Nivel 0: preço entre 0 e 321.950
- Nivel 1: preço entre 321.950 e 450.000
- Nivel 2: preço entre 450.000 e 645.000
- Nivel 3: preço acima de  645.000

Foi criada uma coluna season (estação do ano)

Verão: de junho a agosto.

Outono: de setembro a novembro.

Inverno: de dezembro a fevereiro.

Primavera: de março a maio.

# 3.0 - Planejamento da solução. 

Quais imóveis que imobiliária deveria comprar e por qual preço?

- Agrupar os dados por região (Zipcode)
- Dentro de cada região, eu vou encontrar a mediana do preço dos imóveis
-Vou sugerir que os imóveis que estão abaixo do preço mediano de cada região e que estejam em boas condições, sejam comprados.

Uma vez o imóvel comprado, qual o melhor momento para vende-lo e por qual preço?

- Com os dados organizados e tratados
- Agrupar os imóveis por região (Zipcode) e por sazonalidade (Summer, Inter)
- Dentro de cada região e sazonalidade (season), eu vou calcular a mediana de   preço.

Vou sugerir que:
- Condições de venda:

Para imóveis com o preço da compra maior que a mediana.
- O preço de venda será igual ao preço de compra + 10%.

Para imóveis com o preço de compra menor que a mediana.
- O preço de venda será igual ao preço de compra + 30%.

# 3.0	– Os 5 principais insights de negócio

a.	imóveis com vista para água, são 20% mais caros, na média. 
- A hipótese é verdadeira, a variação é de 212.6%

b.	imóveis com data de construção menor que 1955, são 50% mais baratos, na média.
- A hipótese é falsa, a variação é de 0.8%.

c.	imóveis sem porão possuem área total (sqft_lot), 40% maior que imóveis com porão (basement).
- A hipótese é falsa, a variação é de 22.6%.

d.	o crescimento do preço dos imóveis YOY (year over year) é de 10%.
- A hipótese é falsa, a variação é de 0.6 %

e.	imóveis com 3 banheiros tem um crescimento de MoM (month over month) de 15%
- A hipótese é falsa.

# 4.0	– Resultado de negócio

Investimento necessário 
	-$ 4.093.073.058

Retorno das vendas
	-$ 5.550.157.425

Lucro
	-$ 1.457.084.367

# 5.0	– Conclusão

Foram recomendados para compra 10.571 imóveis que se adquiridos terão um investimento necessário de 4 bilhões de dólares e terão um lucro esperado de 1 bilhão, também foi descoberto que  na primavera a média dos preços dos imóveis sobe se tornando o melhor momento para venda pois o valor de venda dos imóveis adquiridos podem ser reajustados acima dos valores de compra.

