Jednostka transakcji bazy danych (DTU) jest jednostką miary w usłudze SQL Database reprezentującą względną moc baz danych na podstawie miary rzeczywistej: transakcji bazy danych. Wybraliśmy zestaw operacji typowych dla żądania przetwarzania transakcji online (OLTP), a następnie zmierzyliśmy, ile transakcji można ukończyć na sekundę w warunkach pełnego obciążenia (to krótka wersja, wszelkie niuanse opisano w temacie [Omówienie testów porównawczych](../articles/sql-database/sql-database-benchmark-overview.md)). 

Na przykład baza danych Premium P11 z 1750 jednostkami DTU zapewnia 350 razy więcej mocy obliczeniowej DTU niż podstawowa baza danych z 5 jednostkami DTU. 

![Wprowadzenie do usługi SQL Database: jednostki DTU z jedną bazą danych według warstwy i poziomu.](./media/sql-database-understanding-dtus/single_db_dtus.png)

> [!NOTE]
> Podczas migrowania istniejącej bazy danych programu SQL Server można użyć narzędzia innej firmy, [Kalkulatora DTU usługi Azure SQL Database](http://dtucalculator.azurewebsites.net/), aby oszacować poziom wydajności i warstwę usługi, której może ona wymagać w usłudze Azure SQL Database.
> 
> 

### <a name="dtu-vs-edtu"></a>Jednostka DTU a eDTU
Jednostka DTU do pojedynczych baz danych odpowiada jednostce eDTU do elastycznych baz danych. Na przykład baza danych w podstawowej elastycznej puli baz danych udostępnia do 5 jednostek eDTU. Jest to ta sama wydajność co w przypadku jednej podstawowej bazy danych. Różnica polega na tym, że elastyczna baza danych nie korzysta z żadnych jednostek eDTU z puli, dopóki nie jest to konieczne. 

![Wprowadzenie do usługi SQL Database: pule elastyczne według warstwy.](./media/sql-database-understanding-dtus/sqldb_elastic_pools.png)

Najlepszy będzie prosty przykład. Użyjmy podstawowej elastycznej puli baz danych z 1000 jednostek DTU i upuśćmy do niej 800 baz danych. Pod warunkiem, że zawsze używanych jest tylko 200 z 800 baz danych (5 DTU X 200 = 1000), pojemność puli nie zostanie osiągnięta i wydajność bazy danych nie zmniejszy się. Dla jasności ten przykład uproszczono. Rzeczywiste obliczenia są nieco bardziej skomplikowane. Portal wykonuje obliczenia za użytkownika i przekazuje zalecenia w oparciu o historię użycia bazy danych. Informacje na temat funkcji rekomendacji można znaleźć w temacie [Price and performance considerations for an elastic database pool](../articles/sql-database/sql-database-elastic-pool-guidance.md) (Zagadnienia dotyczące cen i wydajności dla elastycznej puli baz danych). Można też wykonać obliczenia samodzielnie. 



<!--HONumber=Jan17_HO3-->


