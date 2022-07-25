# Previsão de vendas - Rossman
![rossmann-stores](https://bi.im-g.pl/im/2f/b6/1a/z28009263IER,Megapromocja-w-sklepach-Rossmann--Obnizki-do--50-p.jpg)

# 1. Problema de negócio
    
 A empresa Rossmann é uma rede de drogarias que atua na Europa e possui mais de 4.000 lojas o que a permite ser uma das rede mais populares.

O CFO possui a tarefa de reformar as lojas, mas encontra uma dificuldade em criar o orçamento, para tal o mesmo solicita uma previsão de faturamento de cada loja para as próximas 6 semanas.

 Portanto desenvolvi análises e modelos de previsão dos faturamento nos dados disponibilizados e publiquei o modelo para que o mesmo ou a equipe de negócio possam decidir quais lojas passarão pela reforma.

# 2. Suposições de negócio

Foram tomadas as seguintes suposições de negócio:

- Apenas consideradas lojas com vendas superior a 0.
- Foram descartados os dias em que as lojas foram fechadas.
- Lojas em que não possuíam dados de competidores próximos tiveram o seu valor fixado em 200.000 metros.

# Lista de atributos

Para o processo de análise foi utilizado um dataset público hospedado no [Kaggle](https://www.kaggle.com/competitions/rossmann-store-sales/overview).

Este dataset possui as seguintes variáveis:

|Atributos | Descrição |
|----------|-----------|
|Id | Um identificador único que representa a loja e data. |
|Store | Id único de cada loja. |
|Date | Data em que ocorreu o evento. |
|DayOfWeek | Variável numérica que representa o dia da semana. |
|Sales | Quantidade de vendas do dia. |
|Customers | O número de clientes na loja no dia. Open - Indicador para loja aberta = 1 ou fechada = 0. |
|StateHoliday | Indica um feriado de estado. Tipicamente as lojas fecham nesses dias. a = Feriado público, b = Feriado de páscoa, c = Natal, 0 = Não há feriado. |
|SchoolHoliday | Feriado escolar, indica se a loja naquele dia fechou. |
|StoreType | Tipos de lojas, possuem variações a, b, c, d. |
|Assortment | Nível de variedade de produtos: a = básico, b = extra, c = estendido. |
|CompetitionDistance | Distância em metros para o competidos mais próximo. |
|CompetitionOpenSince[Month/Year] | Disponibiliza o ano e mês em quem o competidor mais próximo abriu. |
|Promo | Indica se a loja está com uma promoção naquele dia. |
|Promo2 | Indica uma continua e consecutiva promoção da loja: 0 = loja não está participando, 1 = loja participando. |
|Promo2Since[Year/Week] | Descreve o ano e semana de quando a loja começa a promo2. |
|PromoInterval | Descreve os meses em que a loja iniciou a promo2, ex.: "Feb,May,Aug,Nov" significa que a loja iniciou as promoções estendidas em cada um desses meses. |

# 3. A solução

A problemática destaca a necessidade de prever as vendas das lojas para definir o orçamento de uma possível reforma. O modelo em que adoto busca prever as vendas diárias das próximas 6 semanas (42 dias).

O formato de entrega deverá conter informações sobre vendas totais de toda a cadeia de lojas, vendas por lojas e vendas diárias. Para usufruir da ferramenta a equipe juntamente com o CFO poderão acessar via navegação web e também no Telegram.

## Estratégia da solução
        
Para resolver o problema de negócio utilizo da metodologia CRISP-DM adaptada para os processos de ciência de dados, as etapas de processos para a solução serão as seguintes:
        
![crisp_method](https://user-images.githubusercontent.com/107287165/179852850-5a659226-5049-4e2e-8874-08176046ef89.png)
        
Minha metodologia utilizou dos seguintes princípios:

**Step 01. Data Description:** Realizo uma análise breve dos dados e suas estatísticas, também limpo alguns dados com potenciais comprometedoras, o foco está em ganhar conhecimento inicial do problema em que estou lidando, em um ambiente de trabalho esta etapa engloba a extração dos dados do banco de dados ou da internet através da técnica de Web Scrapping.

**Step 02. Feature Engineering:** Desenvolvo hipóteses iniciais sobre o negócio para poder derivar novos atributos com base nas variáveis originais para descrever melhor o fenômeno a ser compreendido, estes atributos podem me auxiliar na validação de hipóteses e no treinamento do modelo de Machine Learning.

**Step 03. Data Filtering:** Nesta etapa busco remover algumas variáveis criadas para auxiliar o processo de Feature Engineering, também altero as seguintes colunas:
- Remoção de vendas com valores 0 e lojas que não abriram no dia, não tenho interesse em prever lojas que não vendem.
- Remoção da coluna “customers” pois para utilizá-la eu precisaria prever quantos clientes teríamos nas próximas 6 semanas, isso demanda um novo projeto de previsão.

**Step 04. Exploratory Data Analysis:**  Nesta etapa busco compreender o comportamento de cada variável e suas interações com as demais, também exploro os dados para encontrar insights e entender melhor o impacto das variáveis.

**Step 05. Data Preparation:** Neste processo utilizo de técnicas de re-escala para adequar os dados a uma boa previsão dentro dos modelos de Machine Learning, outra técnica utilizada são transformações de variáveis categóricas para numéricas (em termos simples transformar letras em números)

**Step 06. Feature Selection:** Nessa etapa utilizo do algoritmo Boruta para selecionar as melhores variáveis para um treinamento de Machine Learning e utilizo do conhecimento adquirido na exploração de dados para validar as escolhas do Boruta.

**Step 07. Machine Learning Modelling:** Nesta etapa testo 5 modelos de Machine Learning com o foco de encontrar o melhor modelo, estes modelos são:

- Modelo simples de média (utilizado como baseline)
- Regressão Linear
- Regressão Linear regularizado (Lasso)
- Random Forest Regressor
- XGBoost Regressor

O melhor modelo são o Random Forest e o XGBoost o que me permite concluir que os dados não possuem fortes comportamentos lineares (portanto complexos).

**Step 08. Hyperparameter Fine Tunning:** Nesse fase utilizo da técnica de Random Search para encontrar melhores parâmetros do Modelo de Machine Learning.

**Step 09. Business Values:** Nesta etapa extraio informações de negócio descrevendo as previsões e sua margem de erro que possam explicar todas as dúvidas acionadas pelo problema de negócio, também busco explicar como o modelo de Machine Learning está se comportando.

**Step 10. Deploy Model to Production:** Nesta última etapa decido que o projeto entrega resultado satisfatório e publico em um site e no Telegram para que a equipe de negócio e o CFO possam acessar os resultados que produzi.

# 4. Top 3 Insights de negócio
    
## H6. Lojas com mais promoções consecutivas deveriam vender mais na media.

![image](https://user-images.githubusercontent.com/107287165/179853601-ca7a100d-b37b-42ed-8f06-c00cb5a36be4.png)

Esta hipótese é falsa, podemos perceber através da linha azul que as lojas que aderem em promoções estendidas tendem a vender menos durante todo o período analisado.

A lógica em torno de minha hipótese está no pensamento dos clientes que ao observarem promoções continuas podem associar aquele novo preço como um preço padrão e não mais descontado.

## H7. Lojas deveriam vender menos nos finais de semana.

![image](https://user-images.githubusercontent.com/107287165/179853708-48977cf2-c747-445e-9126-ef680968f7b2.png)

Este primeiro gráfico demonstra que o eixo X nos números 6 e 7 (Sábado e Domingo) as lojas tendem a vender menos, o calculo foi feito com base no somatório.

![image](https://user-images.githubusercontent.com/107287165/179853747-45b19e10-a438-4648-9c84-1c11a4166167.png)

Para o segundo gráfico temos as vendas em mediana, o que busco expressar entre os dois é o quanto as lojas que abrem aos Domingos conseguem vender bem, entretanto ao observar os primeiros gráficos percebemos que as vendas correspondem a uma parcela inexpressiva de lojas, aproximadamente 2,6%.

Concluo que a hipótese é verdadeira com base no primeiro gráfico pois estarei desconsiderando vendas de um grupo tão pequeno de lojas.

## H11. Lojas em media deveriam vender menos durante os feriados escolares.

![image](https://user-images.githubusercontent.com/107287165/179853819-29e3b915-210b-4712-ad22-c3d0ee844bdc.png)

A hipótese é falsa, os períodos de feriados escolares correspondem a cor laranja, o único mês em que o inverso ocorre é em Dezembro.

Este comportamento presente em diversos meses pode acionar um Insight sobre produtos em que estudantes costumam comprar e ofertar promoções.

# 5. Modelos de Machine Learning aplicados
    
Os testes ocorreram com os seguintes algoritmos:

- Média
- Linear Regression
- Lasso Regression
- Random Forest Regressor
- XGB Regressor
    
Utilizei a média como métrica de comparação (baseline) para os demais modelos e dividi em 2 modelos lineares (Linear e Lasso) e 2 não lineares (Random Forest e XGBoost). A divisão me permite entender o nível de complexidade dos dados e entender quais os tipos de modelos devo utilizar.
    
## Performance do Machine Learning
    
Realizei uma comparação entre os cinco modelos utilizando as últimas 6 semanas dos dados de treino com os seguintes resultados:

| Model Name | MAE | MAPE | RMSE |
|------------|-----|------|------|
|XGBoost Regressor | 675.77 |	0.10 | 984.42 |
|Random Forest Regressor | 679.60 |	0.10 | 1011.12 |
|Average Model | 1354.80 | 0.21	| 1835.14 |
|Linear Regression | 1867.09 | 0.29 | 2671.05 |
|Linear Regression - Lasso | 1891.70 | 0.29	| 2744.45 |

Os dados permitem uma breve conclusão em que os modelos lineares estão com desempenho inferior a uma média, portanto posso considerar que estou lidando com dados complexos.

Para garantir maior veracidade dos dados foi realizado o Cross-Validation 5 vezes e obtemos os seguintes resultados:

| Model name | MAE CV | MAPE CV | RMSE CV |
|------------|--------|---------|---------|
|Random Forest Regressor | 836.61 +/- 217.1	| 0.12 +/- 0.02 |	1254.3 +/- 316.17 |
|XGBoost Regressor | 871.07 +/- 178.3 |	0.12 +/- 0.02	| 1260.15 +/- 259.21 |
|Linear Regression | 2081.73 +/- 295.63 |	0.3 +/- 0.02 |	2952.52 +/- 468.37 |
|Lasso | 2116.38 +/- 341.5 | 0.29 +/- 0.01 | 3057.75 +/- 504.26 |

Os melhores modelos são a Random Forest e o XGBoost, ambos com desempenhos similares, decidi escolher o modelo de XGBoost para a publicação devido a sua otimização de recursos utilizados, uma Random Forest tipicamente ocupa maior espaço em disco.

## Hyper Parameter Fine Tunning
    
Para a otimização do XGBoost utilizei do método de Random Search devido a sua rapidez e facilidade, o melhor parâmetro adotado foi:
    
|Parameter | Value |
|----------|-------|
| n_estimators | 3000 |
| eta | 0.3 |
| max_depth | 5 |
| subsample	| 0.7 |
| colsample_bytree | 0.7 |
| min_child_weight |3 |
    
O resultado apresentado pelo modelo foi:
    
| Model Name | MAE | MAPE	| RMSE |
|------------|-----|------|------|
| XGBoost Regressor |	672.50 | 0.10 | 961.79 |
    
Já o resultado com o Cross Validation é:
    
| Model name | MAE CV | MAPE CV | RMSE CV |
|------------|--------|---------|---------|
| XGBoost Regressor	| 890.57 +/- 139.37 |	0.12 +/- 0.02 |	1268.11 +/- 185.04 |
    
O resultado permite concluir um das ineficiências do método Random Search em que não necessariamente consegue encontrar parâmetros otimizados, portanto será conservado os resultados anteriores.

## Performance do modelo
    
A análise abaixo permite observar as flutuações das previsão em cada período, demonstrando oscilações em mudanças buscas de valores.

![image](https://user-images.githubusercontent.com/107287165/179855027-d3c9e40e-de09-4193-963b-a0d8357cc8a4.png)

Os dois últimos gráficos permitem compreender a distribuição de erros e de previsões demonstrando resultados generalizados e com distribuição normal, isto permite concluir um ótimo resultado para o primeiro ciclo de projeto.

# 6. Resultados de negócio
    
Para expressar os resultados de negócio busco descrever as flutuações de cada previsão, ao analisar o erro relativo de cada previsão por loja é possível a tomada de decisão com base no valor previsto, no melhor valor ou no pior valor.
    
| store	| predictions	| worst_scenario | best_scenario | MAE | MAPE |
|-------|-------------|----------------|---------------|-----|------|
| 667	| 320692.41	| 320276.23 | 321108.59 | 416.18 | 0.05 |
| 259	| 544712.19	| 544073.51	| 545350.87	| 638.68 | 0.05 |
| 1097 | 453320.72 | 452765.12 | 453876.31 | 555.59	| 0.05 |
| 1089 | 392789.03 | 392258.11 | 393319.95 | 530.92	| 0.05 |
| 817 | 757912.50 | 756833.20 | 758991.80 | 1079.30 | 0.05 |
    
É importante destacar que existem algumas previsões com alto grau de incerteza, o que não resulta em um bom apoio para a tomada de decisão, alguns exemplos de stores com grandes taxas de erros são:
    
| store	| predictions	| worst_scenario | best_scenario | MAE | MAPE |
|-------|-------------|----------------|---------------|-----|------|
|292 | 105338.44 | 102110.13 | 108566.75 | 3228.31 | 0.55 |
|909 | 244446.17 | 236786.48 | 252105.86 | 7659.69 | 0.53 |
|183 | 217548.39 | 215626.55 | 219470.23 | 1921.84 | 0.33 |
|876 | 203445.88 | 199463.56 | 207428.19 | 3982.31 | 0.29 |
|722 | 354224.81 | 352199.71 | 356249.92 | 2025.10 | 0.27 |
    
Por último uma demonstração da previsão total de vendas:
    
| Scenario | Values |
|----------|--------|
| predictions | $284,608,704.00 |
| worst_scenario | $283,854,989.77 |
| best_scenario | $285,362,431.34 |

# 7. Modelo em produção
    
Considerando uma performance satisfatória do modelo desenvolvi a publicação das ferramentas em dois ambientes:
    
- Site web utilizando do Streamlit e hospedado na Heroku, essa aplicação permite a equipe de negócios acessar as previsões por loja das próximas 6 semanas tanto em tabela quanto em gráficos.
- Também disponibilizei as informações via Bot do Telegram para que a equipe possa acessar valores totais por loja do modelo.

Aplicações podem ser acessadas pelos ícones abaixo:

[![Streamlit](https://user-images.githubusercontent.com/107287165/179856515-5f0bdccd-43b2-4690-b797-95101bc89e18.png)](https://rossmann-web-predict.herokuapp.com/)
[![Telegram](https://user-images.githubusercontent.com/107287165/179856915-09b107d2-ea4b-49b8-9658-da451d8309c2.png)](https://t.me/RossmannDS_bot)

# 8. Lições aprendidas

- Gestão de projetos: Este projeto me permitiu expandir o horizonte quanto ao desenvolvimento de um projeto ponta a ponta com técnicas focadas em gestão ágil, focadas na solução do problema e possibilitando ciclos de melhorias/otimizações.
- Sempre comentar os códigos: Um detalhes de extrema importância que impacta tanto o meu quanto o entendimento dos demais aos quais buscam entender o meu objetivo e cada etapa dentro do código.
- Divisão do trabalho em códigos: Ao dividir o projeto em diversos segmentos pude poupar tempo considerável, existem processos que demandam horas ou dias de processamento contínuo, é importante salvar essas informações a parte e resgatar nos momentos necessários.

# 9. Próximos passos
    
O método CRIPS constrói um modelo de gerenciamento de projetos cíclicos, isto me permite durante cada tomada de decisão escolher entre a melhor opção e a opção mais viável. Alguns desdobramentos para o segundo ciclo de projeto são:
    
- Derivar mais features para novas análises exploratórias e treinamento do modelo.
- Realizar novos testes de hipóteses.
- Utilização de algoritmos para o tratamento de linhas “NA” na limpeza de dados.
- Testar outras seleções de features nos modelos.
- Testar novas preparações de dados para encoding e transformação da variável resposta.
- Analisar as principais lojas com os maiores erros MAPE buscando maneiras de diminuir o erro.
- Análise de resíduos gerados pelo modelo XGBoost.
- Projeto de Machine Learning para a feature “Customers” permitindo prever os seus resultados e encapsular nas features de previsão de vendas.
