# Úkol 1 - Dotaz 1

    SELECT *, lag(payroll_recalculated) OVER (ORDER BY payroll_branch, `year`) AS lag_payroll
        FROM (SELECT `year`, payroll_branch, payroll_recalculated, payroll_value_type
                FROM t_filip_mlicka_project_SQL_primary_final
                GROUP BY `year`, payroll_branch 
                ORDER BY payroll_branch, `year`) payroll_with_lagged_value
                
# Úkol 1 - Dotaz 2
