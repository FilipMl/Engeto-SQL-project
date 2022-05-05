# Engeto-SQL-project
Výstupy k SQL projektu 

Pro projekt byly z dostupných dat vytvořeny tabulky ***t_filip_mlicka_project_SQL_primary_final*** a ***x***.


## 1. Rostou v průběhu let mzdy ve všech odvětvích, nebo v některých klesají?

***Odpověď:*** Mzdy ve všech odvětvých ve sledovaném období mzdy rostly. Nejpomalejší růst zaznamenolo odvětví "Peněžnictví a pojišťovnictví".

Pro potřebu analýzy jsem vytvořil tři sql dotazy.

Prvně jsem vytvořil nový sloupec, kterému je přiřazena výše mzdy z předchozího období pomocí funkce **LAG()**

      select *, lag(payroll_recalculated) OVER (ORDER BY payroll_branch, year) as lag_payroll
          from (select `year`, payroll_branch, payroll_recalculated, payroll_value_type
            from t_filip_mlicka_project_SQL_primary_final t
            group by `year`, payroll_branch 
            order by payroll_branch, `year`) t

*Náhled na nově vytvořený pomocný select*

 ![Tabulka 1](/image/tabulka_1_1.jpg)
 
 Díky novému sloupci bylo možné spočítat meziroční růst mezd pro jednotlivá odvětví.
 
       select t.`year` , t.payroll_branch, t.payroll_recalculated,
        CASE
          WHEN year = 2006 then null
          ELSE ROUND((payroll_recalculated/lag_payroll-1)*100, 2)
        END as rust_mezd
        from (select *, lag(payroll_recalculated) OVER (ORDER BY payroll_branch, year) as lag_payroll
          from (select `year`, payroll_branch, payroll_recalculated, payroll_value_type
            from t_filip_mlicka_project_SQL_primary_final t
            group by `year`, payroll_branch 
            order by payroll_branch, `year`) t) t
            
Po použití funkce **GROUP BY** a **AVG()** je zřejmé, že výrazně nejpomaljší růst zaznamenalo odvětví Peněžnictví a pojišťovnictví, které také zaznamenalo nejvíce meziročních poklesů. ***Také je ale zřejmé, že mzdy ve všech odvětvých ve sledovaném období mzdy rostly.***

      select year, payroll_branch, AVG(rust_mezd)  
        from (
        select t.`year` , t.payroll_branch, t.payroll_recalculated,
        CASE
          WHEN year = 2006 then null
          ELSE ROUND((payroll_recalculated/lag_payroll-1)*100, 2)
        END as rust_mezd
        from (select *, lag(payroll_recalculated) OVER (ORDER BY payroll_branch, year) as lag_payroll
          from (select `year`, payroll_branch, payroll_recalculated, payroll_value_type
            from t_filip_mlicka_project_SQL_primary_final t
            group by `year`, payroll_branch 
            order by payroll_branch, `year`) t) t) t
        group by payroll_branch

*Průměrný meziroční růst mezd v odvětvích za období 2006-2018*

![Tabulka 1](/image/tabulka_1_2_.jpg) 

*Meziroční růst mezd v odvětví Peněžnictví a pojišťovnictví za období 2006-2018*

 ![Tabulka 1](/image/tabulka_1_3.jpg)

## 2. Kolik je možné si koupit litrů mléka a kilogramů chleba za první a poslední srovnatelné období v dostupných datech cen a mezd?
Vytvořil jsem pomocný select filtrující statky chléb a mléko. Dále je zde vypočítávana průměrná mzda napříč všemi odvětvími pro dané roky, která slouží k výpočtu množství statků, které je možné zakoupit v letech 2006 a 2018.

      SELECT *, ROUND((prumerna_mzda/goods_unit_price),2) as mnozstvi_statku 
        FROM
        (SELECT `year`, AVG(payroll_recalculated) as prumerna_mzda, goods, goods_unit_price, goods_unit_size, goods_unit
          FROM t_filip_mlicka_project_SQL_primary_final tfmpspf
          WHERE (goods = 'Mléko polotučné pasterované'  
            OR goods = 'Chléb konzumní kmínový')
            AND 
            (`year` = 2006 OR `year` = 2018)
          GROUP BY `year`, goods) t
          
 Výstup:

 ![Tabulka 21](/image/tabulka_2_1.jpg)

## 3. Která kategorie potravin zdražuje nejpomaleji (je u ní nejnižší percentuální meziroční nárůst)?

**Odpověď:** Nejpomaleji zdražují Banány žluté a to v průměru o 0,8 % ročně. Dokonce se zde objevují dva statky, jejichž cena ve sledovaném období průměrně klesala, a to Cukr krystalový (-1,9 % ročně) a Rajská jablka (-0,74 %).

Postupoval jsem obdobně jako v úkolu číslo jedna. Vytvořil sem si pomocný select zobrazující jednotlivé statky a jejich průměrné meziroční změny v cenách.

      SELECT goods, AVG(rust_cen) AS prumerny_rust_cen
        FROM (
          SELECT t.`year`, goods,
            CASE
                WHEN year = 2006 then null
                ELSE ROUND((goods_unit_price/lag_goods_unit_price-1)*100, 2)
              END as rust_cen
            FROM (
              SELECT *, lag(goods_unit_price) OVER (ORDER BY goods, year) AS lag_goods_unit_price
                    FROM (SELECT `year`, goods, goods_unit_price, goods_unit_size, goods_unit 
                      FROM t_filip_mlicka_project_SQL_primary_final t
                      GROUP BY `year`, goods 
                      ORDER BY goods, `year`) t) t) t
        GROUP BY goods
        ORDER BY prumerny_rust_cen

![Tabulka 1](/image/tabulka_3_1.jpg)

## 4. Existuje rok, ve kterém byl meziroční nárůst cen potravin výrazně vyšší než růst mezd (větší než 10 %)?


**5. Má výška HDP vliv na změny ve mzdách a cenách potravin? Neboli, pokud HDP vzroste výrazněji v jednom roce, projeví se to na cenách potravin či mzdách ve stejném nebo násdujícím roce výraznějším růstem?**


