<properties
    pageTitle="Importowanie danych do usługi Azure Search przy użyciu indeksatorów w Portalu Azure | Microsoft Azure | Hostowana usługa wyszukiwania w chmurze"
    description="Jak korzystać z indeksatorów w Portalu Azure."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""
    tags="Azure Portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="03/10/2016"
    ms.author="heidist"/>

# Importowanie danych do usługi Azure Search przy użyciu portalu

W Portalu Azure na pulpicie nawigacyjnym usługi Azure Search znajduje się polecenie **Importuj dane**, które przeprowadzi Cię przez pozyskiwanie danych w usłudze Azure Search. Polecenie jest oparte na wbudowanej funkcji indeksatora, która przeszukuje istniejące źródło danych, tworząc i przekazując dokumenty na podstawie zestawu wierszy w źródle danych.

Import danych za pomocą kreatora składa się z 3 części:

- połączenia ze źródłem danych;
- indeksu docelowego, do którego dane są przekazywane (kreator często może go wygenerować);
- harmonogramu, który uruchamia import od razu lub w regularnych odstępach czasu.

Aby można było korzystać z indeksatora lub polecenia **Importuj dane**, podstawowe źródło danych musi być jednym z obsługiwanych źródeł danych: usługą Azure SQL Database, relacyjną bazą danych programu SQL Server na maszynie wirtualnej platformy Azure lub usługą Azure DocumentDB.

Importu można dokonać tylko z pojedynczej tabeli, widoku lub równoważnej struktury danych. Aby uzyskać odpowiednie metadane i dane wejściowe dla indeksu wyszukiwania, może być konieczne uprzednie utworzenie tej struktury danych w źródle danych aplikacji.

Ten przepływ pracy można wypróbować przy użyciu przykładowych danych. Na początek warto zobaczyć [Wprowadzenie do usługi Azure Search w Portalu Azure](search-get-started-portal.md).

##Konfigurowanie importu danych

1. Zaloguj się do [Portalu Azure](https://portal.azure.com).

2. Otwórz pulpit nawigacyjny usługi Azure Search. Oto kilka sposobów na odnalezienie pulpitu nawigacyjnego.
    - Na pasku przechodzenia kliknij pozycję **Strona główna**. Na stronie głównej znajdują się kafelki dla każdej usługi w subskrypcji. Kliknij kafelek, aby otworzyć pulpit nawigacyjny usługi.
    - Na pasku przechodzenia kliknij pozycję **Przeglądaj wszystko** > **Filtruj według** > **Usługi wyszukiwania**, aby znaleźć odpowiednią usługę wyszukiwania na liście.

3. W górnej części pulpitu nawigacyjnego usługi znajduje się pasek poleceń zawierający polecenie **Importuj dane**. Kliknij polecenie **Importuj dane**, aby wysunąć blok Importowanie danych.

4. Kliknij pozycję **Połącz z danymi**, aby określić definicję źródła danych, z której będzie korzystać indeksator. Dostępne są następujące opcje:
    -   Opcja Istniejące źródło danych odnosi się do definicji źródła danych, która została już wcześniej utworzona dla indeksatora. Jeśli masz już zdefiniowane indeksatory w usłudze wyszukiwania, możesz ponownie użyć definicji źródła danych na potrzeby innego importu.
    -   Opcja Azure SQL służy do określania połączenia źródła danych z bazą danych SQL na platformie Azure lub bazą danych SQL Server na maszynie wirtualnej platformy Azure.
    -   Opcja DocumentDB jest używana do określania połączenia ze źródłem danych tego typu.

   Zarówno w przypadku opcji Azure SQL, jak i opcji DocumentDB baza danych musi istnieć w ramach subskrypcji. Możesz wkleić parametry połączenia, jeśli je znasz, lub wybrać źródło danych utworzone wcześniej przez osobę dysponującą uprawnieniami do zapisu dla subskrypcji.

5. Kliknij pozycję **Dostosuj indeks docelowy**, aby ukończyć domyślny indeks.
    - Pole **Klucz** jest wymagane. Pole, które zostanie wybrane jako klucz, musi być polem ciągu zawierającym unikatowe wartości.
    - Nazwa i typ pola są zazwyczaj wypełniane automatycznie. Możesz zmienić typ danych.
    - Wybierz atrybuty dla każdego pola:
        - Atrybut Pobieranie umożliwia zwracanie pola w wynikach wyszukiwania.
        - Atrybut Filtrowanie umożliwia przywoływanie pola w wyrażeniach filtru.
        - Atrybut Sortowanie umożliwia używanie pola podczas sortowania.
        - Atrybut Tworzenie aspektów umożliwia używanie pola w nawigacji aspektowej.
        - Atrybut Wyszukiwanie umożliwia wyszukiwanie pełnotekstowe pola.
    - Kliknij kartę **Analizator**, jeśli chcesz określić analizatora języka na poziomie pola. Aby uzyskać szczegółowe informacje, zobacz [Create an index for documents in multiple language](search-language-support.md) (Tworzenie indeksu dla dokumentów w wielu językach).
    - Kliknij pozycję **Sugestor**, jeśli chcesz włączyć uzupełnianie przy wpisywaniu lub autouzupełnianie zapytań.

6. Kliknij pozycję **Importuj dane**, aby wykonać operację importu za pomocą opcji Uruchom teraz lub skonfigurować harmonogram cykliczny.

Ukończona właśnie operacja importu danych utworzyła w tle indeksator. Teraz możesz edytować indeksator bezpośrednio, aby zmienić dowolne jego składniki.

##Edytowanie istniejącego indeksatora

Na pulpicie nawigacyjnym usługi kliknij dwukrotnie kafelek Indeksator, aby wysunąć listę wszystkich indeksatorów utworzonych dla subskrypcji. Kliknij dwukrotnie jeden z indeksatorów, aby go uruchomić, edytować lub usunąć.



<!--HONumber=Jun16_HO2-->


