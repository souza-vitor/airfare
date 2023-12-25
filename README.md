# Data Science Independent Project #5 – Analyze Airfare Data

## Descrição
Como analista de dados de uma empresa, você recebe dados de tarifas aéreas que cobrem os 1.000 principais mercados contíguos de pares de cidades em estados. Ajude-os a analisar esses dados e identificar tendências.

Utilizei o SQLite e o DB Browser for SQLite para manupilação e consulta do banco de dados.

E agora irei responder as perguntas:

<br>

**- Que intervalo de anos está representado nos dados?**

```sql
select DISTINCT Year
from airfare_data
order by year;
```

R: De 1996 a 2018

<br>
<br>

**- Quais são os voos mais curtos e mais longos e entre quais duas cidades eles estão?**

```sql
select nsmiles, city1, city2
from airfare_data
group by nsmiles
having min(nsmiles);
```

```sql
select nsmiles, city1, city2
from airfare_data
group by nsmiles
having max(nsmiles)
order by nsmiles desc;
```

<br>
<br>

**- Quantas cidades distintas estão representadas nos dados (independentemente de ser a origem ou o destino)?**


```sql
select count(DISTINCT city) as total
from (
	select city1 as city from airfare_data
	union all
	select city2 as city from airfare_data
) as sub;
```

<br>
<br>

**- Qual companhia aérea aparece com mais frequência como a transportadora com a tarifa mais baixa (ou seja, transportadora_low)? E quanto à companhia aérea com a maior participação de mercado (ou seja, transportadora_lg)?**

Tarifa mais baixa:

```sql
select carrier_low, count(carrier_low) as total
from airfare_data
where carrier_low != 'NULL'
group by carrier_low
order by total asc
limit 1
```

Maior participação:

```sql
select carrier_lg, count(carrier_lg) as total
from airfare_data
where carrier_lg != 'NULL'
group by carrier_lg
order by total desc
limit 1;
```

<br>
<br>

**- Quantos casos existem em que a transportadora com a maior quota de mercado não é a transportadora com a tarifa mais baixa? Qual é a diferença média na tarifa?**

```sql
select count(carrier_low) as total, round(avg(fare_lg - fare_low), 2) as avg_diff
from airfare_data
where carrier_low != (select carrier_low from (select carrier_low, count(carrier_low) as total
from airfare_data
where carrier_low != 'NULL'
group by carrier_low
order by total asc
limit 1) as sub) and carrier_lg != carrier_low;
```

<br>
<br>


## Considerações finais
Aprendi bastante sobre subconsultas enquanto fazia as analises.

Segue o link do projeto (em inglês) e do curso que estou fazendo, caso queiram dar uma olhada:

https://discuss.codecademy.com/t/data-science-independent-project-5-analyze-airfare-data/419949

https://www.codecademy.com/career-journey/data-scientist-aly