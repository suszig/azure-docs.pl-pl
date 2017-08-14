Teraz możesz użyć narzędzia Eksplorator danych w witrynie Azure Portal, aby utworzyć bazę danych i kolekcję. 

1. W witrynie Azure Portal w menu nawigacyjnym po lewej kliknij pozycję **Eksplorator danych (wersja zapoznawcza)**. 

2. W bloku **Eksplorator danych (wersja zapoznawcza)** kliknij pozycję **Nowa kolekcja**, a następnie podaj następujące informacje:

    ![Blok Eksplorator danych w witrynie Azure Portal](./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png)

    Ustawienie|Sugerowana wartość|Opis
    ---|---|---
    Identyfikator bazy danych|Zadania|Nazwa nowej bazy danych. Nazwy baz danych muszą zawierać od 1 do 255 znaków i nie mogą zawierać znaków /, \\, #, ? ani mieć spacji na końcu.
    Identyfikator kolekcji|Items|Nazwa nowej kolekcji. W przypadku nazw kolekcji obowiązują takie same wymagania dotyczące znaków, jak dla identyfikatorów baz danych.
    Pojemność magazynu| Stała (10 GB)|Użyj wartości domyślnej. Ta wartość to pojemność magazynu bazy danych.
    Przepływność|400 RU|Użyj wartości domyślnej. Jeśli chcesz zmniejszyć opóźnienie, możesz później przeskalować przepływność w górę.
    RU/m|Wyłączone|Pozostaw wartość domyślną. Jeśli później będzie konieczna obsługa skokowych wzrostów obciążenia, będzie można włączyć funkcję [RU/m](../articles/cosmos-db/request-units-per-minute.md).
    Klucz partycji|/category|Klucz partycji służący do równomiernego dystrybuowania danych do każdej partycji. Wybór odpowiedniego klucza partycji jest ważny w celu utworzenia wydajnej kolekcji. Więcej informacji zawiera temat [Designing for partitioning](../articles/cosmos-db/partition-data.md#designing-for-partitioning) (Projektowanie pod kątem partycjonowania).    
3. Po wypełnieniu formularza kliknij pozycję **OK**.

W Eksploratorze danych zostanie wyświetlona nowa baza danych i kolekcja. 