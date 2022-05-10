# Engeto-SQL-project
Výstupy k SQL projektu 

Pro projekt byly z dostupných dat vytvořeny tabulky ***t_filip_mlicka_project_SQL_primary_final*** a ***t_Filip_Mlicka_project_SQL_secondary_final***. Tyto tabulky jsem vytvořil pomocí těchto sql dotazů:
[SQL dotazy pro tvorbu tabulek](https://github.com/FilipMl/Engeto-SQL-project/blob/main/sql-dotazy-primary-secondary.md)

## 1. Rostou v průběhu let mzdy ve všech odvětvích, nebo v některých klesají?

***Odpověď:*** Mzdy ve všech odvětvých ve sledovaném období mzdy rostly. Nejpomalejší růst zaznamenolo odvětví "Peněžnictví a pojišťovnictví".

Pro potřebu analýzy jsem vytvořil tři sql dotazy.

Prvně jsem vytvořil SELECT s novým dopočítaným sloupecem "lag_payroll", kterému je přiřazena výše mzdy z předchozího období pomocí funkce **LAG()**

[1. sql dotaz](https://github.com/FilipMl/Engeto-SQL-project/blob/main/1-sql-dotazy.md)

*Náhled na nově vytvořený pomocný SELECT*

 ![Tabulka 1](/image/tabulka_1_1nova.jpg)
 
 Díky novému sloupci bylo možné spočítat meziroční růst mezd pro jednotlivá odvětví.
 
[2. sql dotaz](https://github.com/FilipMl/Engeto-SQL-project/blob/main/1-sql-dotazy.md)
            
Po použití funkce **GROUP BY payroll_branch** a **AVG(payroll_growth)** je zřejmé, že výrazně nejpomaljší růst zaznamenalo odvětví Peněžnictví a pojišťovnictví, které také zaznamenalo nejvíce meziročních poklesů. ***Také je ale zřejmé, že mzdy ve všech odvětvých ve sledovaném období mzdy rostly.***

[3. sql dotaz](https://github.com/FilipMl/Engeto-SQL-project/blob/main/1-sql-dotazy.md)

*Průměrný meziroční růst mezd v odvětvích za období 2006-2018*

![Tabulka 1](/image/tabulka_1_2nova.jpg) 

*Meziroční růst mezd v odvětví Peněžnictví a pojišťovnictví za období 2006-2018*

 ![Tabulka 1](/image/tabulka_1_3.jpg)

## 2. Kolik je možné si koupit litrů mléka a kilogramů chleba za první a poslední srovnatelné období v dostupných datech cen a mezd?
Vytvořil jsem pomocný select filtrující statky chléb a mléko. Dále je zde vypočítávana průměrná mzda napříč všemi odvětvími pro dané roky, která slouží k výpočtu množství statků, které je možné zakoupit v letech 2006 a 2018.

[1. sql dotaz](https://github.com/FilipMl/Engeto-SQL-project/blob/main/2-sql-dotazy.md)
          
 Výstup:

 ![Tabulka 21](/image/tabulka_2_1nova.jpg)

## 3. Která kategorie potravin zdražuje nejpomaleji (je u ní nejnižší percentuální meziroční nárůst)?

**Odpověď:** Nejpomaleji zdražují Banány žluté a to v průměru o 0,8 % ročně. Dokonce se zde objevují dva statky, jejichž cena ve sledovaném období průměrně klesala, a to Cukr krystalový (-1,9 % ročně) a Rajská jablka (-0,74 %).

Postupoval jsem obdobně jako v úkolu číslo jedna. Vytvořil sem si pomocný select zobrazující jednotlivé statky a jejich průměrné meziroční změny v cenách.

[1. sql dotaz](https://github.com/FilipMl/Engeto-SQL-project/blob/main/3-sql-dotazy.md)

![Tabulka 1](/image/tabulka_3_1nova.jpg)

## 4. Existuje rok, ve kterém byl meziroční nárůst cen potravin výrazně vyšší než růst mezd (větší než 10 %)?

**ODPOVĚĎ:** Dle výsledku je zřejmé, že rozdíl v nárustech žádný rok nepřekročil 10 %. Nejblíže této hodnotě jse ceny a mzdy nacházely v roce 2009, kdy mzdy rostly o 8,2 % rychleji než ceny statků.


Pro vypočet tohoto úkolu jsem využil vytvořený select z předešlé úlohy a vytvořil obdobný pro mzdy. Pomocí funkce LEFT JOIN jsem je následně spojil a vypočítal rozdíl mezi hodnotami. Výsledná tabulka obashuje průmerný rust statků a mezd mezi jednotlivými roky.

[1. sql dotaz](https://github.com/FilipMl/Engeto-SQL-project/blob/main/4-sql-dotazy.md)

![Tabulka 1](/image/tabulka_4_1nova.jpg)


## 5. Má výška HDP vliv na změny ve mzdách a cenách potravin? Neboli, pokud HDP vzroste výrazněji v jednom roce, projeví se to na cenách potravin či mzdách ve stejném nebo násdujícím roce výraznějším růstem?

**ODPOVĚĎ:** Data ukazují, že existuje určitá korelace těchto hodnot, ale vyskytují se zde velké výchylky a nelze tedy usoudit, že výrázný růst HDP se nutně promítne na růstu cen nebo mezd v dalším roce.

U tohoto kroku jsem využil data z předešlého úkolu a podobným způsobem připravil data GDP pro českou republiku.

[1. sql dotaz](https://github.com/FilipMl/Engeto-SQL-project/blob/main/5-sql-dotazy.md)
                  
![Tabulka 1](/image/tabulka_5_1nova.jpg)
