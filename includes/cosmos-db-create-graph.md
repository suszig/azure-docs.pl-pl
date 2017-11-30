Teraz możesz użyć narzędzia Eksplorator danych w witrynie Azure Portal, aby utworzyć bazę danych grafów. 

1. W portalu Azure, w menu po lewej stronie, wybierz **Eksploratora danych (wersja zapoznawcza)**.

2. W obszarze **Eksploratora danych (wersja zapoznawcza)**, wybierz pozycję **nowy wykres**. Następnie wypełnij strony, korzystając z następujących informacji:

    ![Eksplorator danych w witrynie Azure Portal](./media/cosmos-db-create-graph/azure-cosmosdb-data-explorer.png)

    Ustawienie|Sugerowana wartość|Opis
    ---|---|---
    Identyfikator bazy danych|sample-database|Wprowadź *bazy danych przykładowych* jako nazwę nowej bazy danych. Nazwy bazy danych musi należeć do zakresu od 1 do 255 znaków i nie może zawierać `/ \ # ?` lub spacji.
    Identyfikator grafu|sample-graph|Wprowadź *wykres próbki* jako nazwę nowej kolekcji. Wykres nazwy mają te same wymagania znak jako identyfikatorów w bazie danych.
    Pojemność magazynu| 10 GB|Pozostaw wartość domyślną. To jest pojemność magazynu bazy danych.
    Przepływność|400 jednostek żądania|Pozostaw wartość domyślną. Później możesz skalować przepływność, aby zmniejszyć opóźnienie.
    Klucz partycji|/FirstName|Klucz partycji służący do równomiernego dystrybuowania danych do każdej partycji. Wybranie klucza partycji poprawne jest ważne podczas tworzenia wykresu wydajności. Aby uzyskać więcej informacji, zobacz [projektowania partycjonowania](../articles/cosmos-db/partition-data.md#designing-for-partitioning).

3. Po wypełnić formularza wybierz **OK**.
