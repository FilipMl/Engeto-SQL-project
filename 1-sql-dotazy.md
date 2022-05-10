# Úkol 1 - Dotaz 1

    SELECT *, lag(payroll_recalculated) OVER (ORDER BY payroll_branch, `year`) AS lag_payroll
        FROM (SELECT `year`, payroll_branch, payroll_recalculated, payroll_value_type
                FROM t_filip_mlicka_project_SQL_primary_final
                GROUP BY `year`, payroll_branch 
                ORDER BY payroll_branch, `year`) payroll_with_lagged_value
                
# Úkol 1 - Dotaz 2

    SELECT `year` , payroll_branch, payroll_recalculated,
        CASE
          WHEN year = 2006 then null
          ELSE ROUND((payroll_recalculated/lag_payroll-1)*100, 2)
        END as payroll_growth
        from (SELECT *, lag(payroll_recalculated) OVER (ORDER BY payroll_branch, `year`) AS lag_payroll
                  FROM (SELECT `year`, payroll_branch, payroll_recalculated, payroll_value_type
                          FROM t_filip_mlicka_project_SQL_primary_final
                          GROUP BY `year`, payroll_branch 
                          ORDER BY payroll_branch, `year`) payroll_with_lagged_value) payroll_growth

# Úkol 1 - Dotaz 3

    SELECT payroll_branch, AVG(payroll_growth) AS `payroll_growth_avg_2006_2018`
        FROM (SELECT payroll_growth.`year` , payroll_growth.payroll_branch, payroll_growth.payroll_recalculated,
                CASE
                  WHEN `year` = 2006 THEN null
                  ELSE ROUND((payroll_recalculated/lag_payroll-1)*100, 2)
                END AS payroll_growth
                FROM (SELECT *, lag(payroll_recalculated) OVER (ORDER BY payroll_branch, `year`) AS lag_payroll
                          FROM (SELECT `year`, payroll_branch, payroll_recalculated, payroll_value_type
                                  FROM t_filip_mlicka_project_SQL_primary_final
                                  GROUP BY `year`, payroll_branch 
                                  ORDER BY payroll_branch, `year`) payroll_with_lagged_value) payroll_growth) payroll_growth_avg_2006_2018
        GROUP BY payroll_branch
