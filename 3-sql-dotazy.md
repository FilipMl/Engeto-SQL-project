# Úkol 3 - Dotaz 1

        SELECT goods, AVG(price_growth) AS avg_price_growth
            FROM (SELECT `year`, goods,
                    CASE
                        WHEN year = 2006 then null
                        ELSE ROUND((goods_unit_price/lag_goods_unit_price-1)*100, 2)
                    END as price_growth
                    FROM (SELECT *, lag(goods_unit_price) OVER (ORDER BY goods, year) AS lag_goods_unit_price
                            FROM (SELECT `year`, goods, goods_unit_price, goods_unit_size, goods_unit 
                                    FROM t_filip_mlicka_project_SQL_primary_final
                                    GROUP BY `year`, goods 
                                    ORDER BY goods, `year`) goods_price_by_year) goods_price_by_year_with_lagged_price) goods_avg_price_growth
            GROUP BY goods
            ORDER BY avg_price_growth
