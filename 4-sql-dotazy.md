  # Úkol 1 - Dotaz 1
  
     SELECT avg_price_growth_by_year.year, avg_price_growth_by_year.avg_price_growth, avg_payroll_growth_by_year.avg_payroll_growth, ABS(avg_price_growth_by_year.avg_price_growth-avg_payroll_growth_by_year.avg_payroll_growth) as diff
          FROM (SELECT `year`, AVG(price_growth) AS avg_price_growth
              FROM (SELECT `year`, goods,
                      CASE
                          WHEN `year` = 2006 then null
                          ELSE ROUND((goods_unit_price/lag_goods_unit_price-1)*100, 2)
                      END as price_growth
                      FROM (SELECT *, lag(goods_unit_price) OVER (ORDER BY goods, year) AS lag_goods_unit_price
                              FROM (SELECT `year`, goods, goods_unit_price, goods_unit_size, goods_unit 
                                      FROM t_filip_mlicka_project_SQL_primary_final
                                      GROUP BY `year`, goods 
                                      ORDER BY goods, `year`) goods_price_by_year) goods_price_by_year_with_lagged_price) goods_avg_price_growth
                      GROUP BY year) avg_price_growth_by_year
          LEFT JOIN (SELECT * 
                        FROM (SELECT `year`, AVG(payroll_growth) as avg_payroll_growth
                                  FROM (SELECT `year` , payroll_branch, payroll_recalculated,
                                          CASE
                                            WHEN year = 2006 then null
                                            ELSE ROUND((payroll_recalculated/lag_payroll-1)*100, 2)
                                          END as payroll_growth
                                          FROM (SELECT *, lag(payroll_recalculated) OVER (ORDER BY payroll_branch, `year`) AS lag_payroll
                                                  FROM (SELECT `year`, payroll_branch, payroll_recalculated, payroll_value_type
                                                          FROM t_filip_mlicka_project_SQL_primary_final
                                                          GROUP BY `year`, payroll_branch 
                                                          ORDER BY payroll_branch, `year`) payroll_by_year) payroll_by_year_with_lagged_payroll) avg_payroll_growth
                        GROUP BY `year`) avg_payroll_growth_by_year) avg_payroll_growth_by_year
          ON avg_price_growth_by_year.year = avg_payroll_growth_by_year.year
