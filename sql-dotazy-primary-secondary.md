# Primary
        CREATE TABLEt_filip_mlicka_project_SQL_primary_final SELECT payroll_year AS `year`, payroll_branch, payroll AS payroll_recalculated, payroll_value_type, goods,round(goods_unit_price, 2) AS goods_unit_price, goods_unit_size, goods_unit
            FROM (SELECT cp.payroll_year, cpc.name AS payroll_calculation, cpib.name AS payroll_branch,cp.value as payroll, cpu.name AS payroll_unit, cpvt.name AS payroll_value_type, cpvt.code
                    FROM czechia_payroll cp
                    LEFT JOIN czechia_payroll_calculation cpc on  cp.calculation_code = cpc.code
                    LEFT JOIN czechia_payroll_industry_branch cpib on cp.industry_branch_code  = cpib.code
                    LEFT JOIN czechia_payroll_unit cpu on cp.unit_code = cpu.code 
                    LEFT JOIN czechia_payroll_value_type cpvt on cp.value_type_code = cpvt.code
                    WHERE payroll_year > 2005
                        AND cpc.name = "přepočtený"
                        AND cpvt.code = "5958"
                        AND cpib.name is not null
                    GROUP BY cp.payroll_year, cpib.name
                    ORDER BY payroll_year, payroll_quarter, payroll_branch) AS czechia_payroll_new
            JOIN (SELECT year(cp.date_from) AS price_year, cpc.name AS goods, cpc.price_value AS goods_unit_size, cpc.price_unit AS goods_unit, AVG(cp.value) AS goods_unit_price 
                            FROM czechia_price cp
                            LEFT JOIN czechia_price_category cpc on cp.category_code = cpc.code 
                            LEFT JOIN czechia_region cr ON cp.region_code = cr.code 
                            WHERE 0=0
                                AND cr.name is null
                                AND year(date_from) > 2005
                            GROUP BY year(cp.date_from), cpc.name
                            ORDER BY date_from, cpc.name) AS czechia_price_new ON czechia_payroll_new.payroll_year = czechia_price_new.price_year;


# Secondary
    CREATE TABLE t_Filip_Mlicka_project_SQL_secondary_final (
        SELECT year, country, ROUND((gdp/lag_gdp-1)*100,2)
            FROM (SELECT `year`, country, gdp, lag(gdp) OVER (ORDER BY year) AS lag_gdp
                    FROM economies e 
                    WHERE country = "Czech Republic" 
                      and year < 2019 
                      and year > 2004) cze_gdp_by_year_lagged_gdp)
