# Primary

# Secondary
    CREATE TABLE t_Filip_Mlicka_project_SQL_secondary_final (SELECT year, country, ROUND((gdp/lag_gdp-1)*100,2)
                                                                FROM (SELECT `year`, country, gdp, lag(gdp) OVER (ORDER BY year) AS lag_gdp
                                                                        FROM economies e 
                                                                        WHERE country = "Czech Republic" 
                                                                          and year < 2019 
                                                                          and year > 2004) cze_gdp_by_year_lagged_gdp)
