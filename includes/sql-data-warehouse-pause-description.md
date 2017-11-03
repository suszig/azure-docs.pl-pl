
<!--
includes/sql-data-warehouse-include-pause-description.md

Latest Freshness check:  2016-04-22 , barbkess.

As of circa 2016-04-22, the following topics might include this include:
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-powershell.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-rest-api.md

-->
W celu ograniczenia kosztów, możesz wstrzymywać i wznawiać obliczeń zasobów na żądanie. Na przykład jeśli nie będzie używać bazy danych w nocy i w weekendy, można wstrzymywać go w tych godzinach i wznawiać go w ciągu dnia. Użytkownik nie zostanie obciążona dla jednostek dwu, podczas bazy danych zostało wstrzymane.

Po zatrzymaniu bazy danych:

* Zasoby obliczeniowe i pamięci są zwracane do puli zasobów dostępnych w centrum danych
* Jednostka DWU koszty są zera na czas trwania pauzy.
* Nie wpływa na magazyn danych i danych pozostaje niezmieniona. 
* Usługa SQL Data Warehouse anuluje wszystkie uruchomione lub umieszczonych w kolejce operacji.

