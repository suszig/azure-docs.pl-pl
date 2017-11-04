
<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### <a name="obtain-the-connection-string-from-the-azure-portal"></a>Uzyskanie parametrów połączenia z portalu Azure
Użyj [portalu Azure](https://portal.azure.com/) do uzyskania wymaganych przez program kliencki wchodzić w interakcje z bazy danych SQL Azure ciągu połączenia: 

1. Kliknij przycisk **PRZEGLĄDAJ** > **baz danych SQL**.
2. Wprowadź nazwę bazy danych w polu tekstowym filtru u lewej górnej **baz danych SQL** bloku.
3. Kliknij wiersz bazy danych.
4. Po wyświetleniu bloku bazy danych, można kliknąć formanty standardowe Minimalizuj zwijanie bloków, używane do przeglądania i filtrowania bazy danych dla wygody visual. 
   
    ![Filtr, aby odizolować bazy danych][10-FilterDatabase]
5. W bloku bazy danych, kliknij **Pokaż parametry połączenia bazy danych**.
6. Jeśli zamierzasz używać biblioteki połączenia ADO.NET, skopiuj ciąg z etykietą **ADO**. 
   
    ![Skopiuj parametry połączenia ADO bazy danych][20-CopyAdoConnectionString]
7. W jeden format lub innym ciągu połączenia należy wkleić kod klienta programu.

Aby uzyskać więcej informacji, zobacz:<br/>[Parametry połączenia i pliki konfiguracyjne](http://msdn.microsoft.com/library/ms254494.aspx).

<!-- Image references. -->

[10-FilterDatabase]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-a.png

[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
