# Úkol 2 - Dotaz 1

    SELECT *, ROUND((payroll_avg/goods_unit_price),2) AS goods_quantity
        FROM (SELECT `year`, AVG(payroll_recalculated) AS payroll_avg, goods, goods_unit_price, goods_unit_size, goods_unit
                FROM t_filip_mlicka_project_SQL_primary_final
                WHERE (goods = 'Mléko polotučné pasterované' OR goods = 'Chléb konzumní kmínový') AND (`year` = 2006 OR `year` = 2018)
                GROUP BY `year`, goods) milk_and_bread_with_avg_payroll
