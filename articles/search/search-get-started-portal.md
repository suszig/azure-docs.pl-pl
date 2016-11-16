---
title: "Wprowadzenie do usługi Azure Search | Microsoft Docs"
description: "Dowiedz się, jak utworzyć pierwszy indeks usługi Azure Search, korzystając z tego samouczka i przykładowych danych usługi DocumentDB. Przedstawione ćwiczenie jest oparte na portalu, nie korzysta z kodu i jest przeprowadzane przy użyciu kreatora importu danych."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 21adc351-69bb-4a39-bc59-598c60c8f958
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 10/03/2016
ms.author: heidist
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: dddbcbcd82900d7537c2d60631cc1753554d9486


---
# <a name="get-started-with-azure-search-in-the-portal"></a>Wprowadzenie do usługi Azure Search w portalu
To niekorzystające z kodu wprowadzenie pozwala rozpocząć pracę z usługą Microsoft Azure Search za pomocą funkcji wbudowanych bezpośrednio w portal. 

W samouczku założono, że istnieje [przykładowa baza danych DocumentDB platformy Azure](#apdx-sampledata), którą możesz łatwo utworzyć, korzystając z naszych danych i instrukcji, ale możesz też dostosować te kroki do swoich danych istniejących w bazie danych zarówno DocumentDB, jak i SQL.

> [!NOTE]
> Ten samouczek wprowadzający wymaga [subskrypcji platformy Azure](/pricing/free-trial/?WT.mc_id=A261C142F) i [usługi Azure Search](search-create-service-portal.md). 
> 
> 

## <a name="find-your-service"></a>Znajdowanie usługi
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Otwórz pulpit nawigacyjny usługi Azure Search. Oto kilka sposobów na odnalezienie pulpitu nawigacyjnego.
   
   * Na pasku przechodzenia kliknij pozycję **Usługi wyszukiwania**. Pasek przechodzenia wyświetla wszystkie usługi aprowizowane w Twojej subskrypcji. Jeśli zdefiniowano usługę wyszukiwania, na liście zostanie wyświetlona pozycja **Usługi wyszukiwania**.
   * Na pasku przechodzenia kliknij pozycję **Przeglądaj**, a następnie wpisz ciąg „wyszukiwanie” w polu wyszukiwania, aby wyświetlić listę wszystkich usług wyszukiwania utworzonych w Twoich subskrypcjach.

## <a name="check-for-space"></a>Sprawdzanie ilości wolnego miejsca
Wielu klientów zaczyna od bezpłatnej usługi. Ta wersja jest ograniczona do trzech indeksów, trzech źródeł danych i trzech indeksatorów. Przed rozpoczęciem upewnij się, że dysponujesz miejscem na dodatkowe elementy. Ten przewodnik tworzy po jednym obiekcie każdego typu.

## <a name="create-an-index-and-load-data"></a>Tworzenie indeksu i ładowanie danych
Zapytania wyszukiwania przechodzą przez kolejne pozycje *indeksu* zawierającego dane z możliwością wyszukiwania, metadane i konstrukcje używane do optymalizacji niektórych zachowań wyszukiwania. Pierwszym krokiem jest zdefiniowanie i wypełnienie indeksu.

Indeks możesz utworzyć na jeden z kilku sposobów. Jeśli dane znajdują się w magazynie, który usługa Azure Search może przeszukiwać — takim jak usługa Azure SQL Database, program SQL Server na maszynie wirtualnej platformy Azure lub usługa DocumentDB — możesz bardzo łatwo utworzyć i wypełnić indeks, używając *indeksatora*.

Aby podczas tego zadania korzystać jedynie z portalu, zostaną użyte dane z usługi DocumentDB, które mogą być przeszukiwane przy użyciu indeksatora za pośrednictwem kreatora **Import danych**. 

Przed kontynuowaniem pracy z samouczkiem utwórz [przykładową bazę danych usługi DocumentDB](#apdx-sampledata), a następnie wróć do tej sekcji, aby wykonać poniższe kroki.

<a id="defineDS"></a>

#### <a name="step-1-define-the-data-source"></a>Krok 1. Definiowanie źródła danych
1. Na pulpicie nawigacyjnym usługi Azure Search kliknij pozycję **Importuj dane** na pasku poleceń, aby uruchomić kreatora, który zarówno tworzy, jak i wypełnia indeks.
   
    ![][7]
2. W kreatorze kliknij pozycję **Źródło danych** > **DocumentDB** > **Nazwa** i wpisz nazwę źródła danych. Źródło danych jest obiektem połączenia w usłudze Azure Search, który może być używany z innymi indeksatorami. Po jego utworzeniu staje się on dostępny jako „istniejące źródło danych” w Twojej usłudze.
3. Wybierz istniejące konto usługi DocumentDB oraz bazę danych i kolekcję. Jeśli używasz przykładowych danych, które udostępniamy, definicja źródła danych wygląda następująco:
   
    ![][2]

Zwróć uwagę, że zapytanie jest pomijane. Jest to spowodowane tym, że obecnie nie implementujemy funkcji śledzenia zmian w zestawie danych. Jeśli Twój zestaw danych zawiera pole, które przechowuje informacje o aktualizowaniu rekordu, możesz skonfigurować indeksator usługi Azure Search tak, aby mógł używać funkcji śledzenia zmian dla wybranych aktualizacji Twojego indeksu.

Kliknij przycisk **OK**, aby ukończyć ten krok kreatora.

#### <a name="step-2-define-the-index"></a>Krok 2. Definiowanie indeksu
W kreatorze kliknij pozycję **Indeks** i zapoznaj się z powierzchnią projektową tworzenia indeksu usługi Azure Search. Indeks wymaga co najmniej nazwy i kolekcji pól, z jednym polem oznaczonym jako klucz dokumentu. Ponieważ używamy zestawu danych usługi DocumentDB, pola są automatycznie wykrywane przez kreatora, a do indeksu są wstępnie ładowane pola i przypisane typy danych. 

  ![][3]

Mimo że pola i typy danych są skonfigurowane, nadal konieczne jest przypisanie atrybutów. Pola wyboru w górnej części listy pól są *atrybutami indeksu*, które kontrolują sposób używania pola. 

* **Pobieranie** oznacza, że pole jest wyświetlane na liście wyników wyszukiwania. Możesz oznaczyć poszczególne pola tak, aby nie były uwzględniane w wynikach wyszukiwania. W tym celu wystarczy usunąć zaznaczenie danego pola wyboru, na przykład wtedy, gdy pola są używane tylko w wyrażeniach filtrowania. 
* Atrybuty **Filtrowanie**, **Sortowanie** i **Tworzenie aspektów** określają, czy pole może być używane w strukturze nawigacji tworzenia aspektów, filtrowania lub sortowania. 
* **Wyszukiwanie** oznacza, że pole jest uwzględniane podczas wyszukiwania pełnotekstowego. Ciągi zazwyczaj można wyszukiwać. Pól liczbowych i logicznych często nie można wyszukiwać. 

Zanim opuścisz tę stronę oznacz w indeksie pola w następujący sposób (Pobieranie, Wyszukiwanie i tak dalej). Większość pól ma możliwość pobierania. Większość pól ciągów ma możliwość wyszukiwania (dla klucza nie musisz włączać opcji Wyszukiwanie). Niektóre pola, takie jak genre, orderableOnline, rating i tags również mają możliwość filtrowania, sortowania i tworzenia aspektów. 

| Pole | Typ | Opcje |
| --- | --- | --- |
| id |Edm.String | |
| albumTitle |Edm.String |Pobieranie, Wyszukiwanie |
| albumUrl |Edm.String |Pobieranie, Wyszukiwanie |
| genre |Edm.String |Pobieranie, Wyszukiwanie, Filtrowanie, Sortowanie, Tworzenie aspektów |
| genreDescription |Edm.String |Pobieranie, Wyszukiwanie |
| artistName |Edm.String |Pobieranie, Wyszukiwanie |
| orderableOnline |Edm.Boolean |Pobieranie, Filtrowanie, Sortowanie, Tworzenie aspektów |
| tags |Collection(Edm.String) |Pobieranie, Filtrowanie, Tworzenie aspektów |
| price |Edm.Double |Pobieranie, Filtrowanie, Tworzenie aspektów |
| margin |Edm.Int32 | |
| rating |Edm.Int32 |Pobieranie, Filtrowanie, Sortowanie, Tworzenie aspektów |
| inventory |Edm.Int32 |Pobieranie |
| lastUpdated |Edm.DateTimeOffset | |

Dla porównania na poniższym zrzucie ekranu przedstawiono indeks utworzony według specyfikacji z powyższej tabeli.

 ![][4]

Kliknij przycisk **OK**, aby ukończyć ten krok kreatora.

#### <a name="step-3-define-the-indexer"></a>Krok 3. Definiowanie indeksatora
W kreatorze **Import danych** kliknij pozycję **Indeksator** > **Nazwa**, wpisz nazwę indeksatora i użyj ustawień domyślnych dla pozostałych wartości. Ten obiekt definiuje proces wykonywalny. Po utworzeniu indeksatora możesz go uruchamiać w ramach harmonogramu cyklicznego, ale na razie użyj opcji domyślnej, aby uruchomić indeksatora jednorazowo natychmiast po kliknięciu przycisku **OK**. 

Wszystkie pozycje importowanych danych powinny zostać uzupełnione i przygotowane do pracy.

  ![][5]

Aby uruchomić kreatora, kliknij przycisk **OK** w celu rozpoczęcia importowania, a następnie zamknij kreatora.

## <a name="check-progress"></a>Sprawdzanie postępu
Aby sprawdzić postęp wróć do pulpitu nawigacyjnego usługi, przewiń w dół, a następnie kliknij dwukrotnie kafelek **Indeksatory**, aby otworzyć listę indeksatorów. Na liście powinien być widoczny właśnie utworzony indeksator o stanie wskazującym trwające indeksowanie lub pomyślne jego zakończenie wraz z liczbą dokumentów umieszczonych w indeksie usługi Azure Search.

  ![][6]

## <a name="query-the-index"></a>Wykonywanie zapytań względem indeksu
Dysponujesz już indeksem wyszukiwania, którego możesz używać do wykonywania zapytań. 

**Eksplorator wyszukiwania** jest narzędziem do wykonywania zapytań wbudowanym w portal. Zawiera pole wyszukiwania, dzięki czemu możesz sprawdzić, czy dane wejściowe wyszukiwania zwracają oczekiwane dane. 

1. Kliknij pozycję **Eksplorator wyszukiwania** na pasku poleceń.
2. Zwróć uwagę, który indeks jest aktywny. Jeśli nie jest to właśnie utworzony indeks, kliknij pozycję **Zmień indeks** na pasku poleceń, aby wybrać odpowiedni indeks.
3. Pole wyszukiwania pozostaw puste, a następnie kliknij przycisk **Wyszukaj**, aby uruchomić wyszukiwanie z użyciem symboli wieloznacznych, które zwraca wszystkie dokumenty.
4. Wprowadź kilka zapytań wyszukiwania pełnotekstowego. Możesz przejrzeć wyniki wyszukiwania z użyciem symboli wieloznacznych, aby zapoznać się z artystami, albumami i gatunkami, dla których możesz wykonywać zapytania.
5. Wypróbuj inną składnię zapytania, korzystając z [przykładów podanych na końcu tego artykułu](https://msdn.microsoft.com/library/azure/dn798927.aspx) oraz modyfikując zapytanie do używania ciągów wyszukiwania, które mogą znajdować się w indeksie.

## <a name="next-steps"></a>Następne kroki
Po jednokrotnym uruchomieniu kreatora możesz wrócić i wyświetlić lub zmodyfikować poszczególne składniki: indeks, indeksator lub źródło danych. Niektóre zmiany, takie jak zmiana typu danych pola, są niedozwolone w indeksie, ale większość właściwości i ustawień można modyfikować. Aby wyświetlić poszczególne składniki, kliknij kafelek **Indeks**, **Indeksator** lub **Źródła danych** na pulpicie nawigacyjnym w celu wyświetlenia listy istniejących obiektów.

Aby dowiedzieć się więcej o innych funkcjach wymienionych w tym artykule, odwiedź te linki:

* [Indexers](search-indexer-overview.md) (Indeksatory)
* [Create Index](https://msdn.microsoft.com/library/azure/dn798941.aspx) (Tworzenie indeksu) — zawiera szczegółowe wyjaśnienie atrybutów indeksu
* [Eksplorator wyszukiwania](search-explorer.md)
* [Search Documents](https://msdn.microsoft.com/library/azure/dn798927.aspx) (Wyszukiwanie dokumentów) — zawiera przykłady składni zapytania

Możesz spróbować wykonać ten sam przepływ pracy z użyciem kreatora Importu danych dla innych źródeł danych, takich jak usługa Azure SQL Database lub program SQL Server na maszynach wirtualnych platformy Azure.

> [!NOTE]
> Niedawno ogłoszono wprowadzenie w indeksatorze obsługi przeszukiwania usługi Azure Blob Storage, ale ta funkcja jest w wersji zapoznawczej i nie została jeszcze udostępniona w portalu. Aby wypróbować ten indeksator, trzeba napisać kod. Zobacz [Indexing Azure Blob storage in Azure Search](search-howto-indexing-azure-blob-storage.md) (Indeksowanie danych z usługi Azure Blob Storage w usłudze Azure Search), aby uzyskać więcej informacji.
> <a id="apdx-sampledata"></a>
> 
> 

## <a name="appendix-create-sample-data-in-documentdb"></a>Dodatek: tworzenie danych przykładowych w usłudze DocumentDB
W tej sekcji zostanie utworzona mała baza danych w usłudze DocumentDB, której możesz użyć podczas wykonywania zadań w tym samouczku.

Poniższe instrukcje zawierają ogólne wytyczne, ale nie są wyczerpujące. Jeśli potrzebujesz więcej pomocy przy nawigacji w portalu usługi DocumentDB lub przy wykonywaniu zadań, możesz zapoznać się z dokumentacją usługi DocumentDB. Jednak większość potrzebnych poleceń znajduje się na pasku poleceń usługi w górnej części pulpitu nawigacyjnego lub w bloku bazy danych. 

  ![][1]

### <a name="create-musicstoredb-for-this-tutorial"></a>Tworzenie bazy danych musicstoredb dla tego samouczka
1. [Kliknij tutaj](https://github.com/HeidiSteen/azure-search-get-started-sample-data), aby pobrać plik ZIP zawierający pliki danych w formacie JSON dla sklepu z muzyką. Udostępniamy 246 dokumentów w formacie JSON dla tego zestawu danych.
2. Dodaj usługę DocumentDB do subskrypcji, a następnie otwórz pulpit nawigacyjny usługi.
3. Kliknij pozycję **Dodaj bazę danych**, aby utworzyć nową bazę danych o identyfikatorze `musicstoredb`. Po utworzeniu zostanie ona wyświetlona w kafelku bazy danych poniżej na stronie.
4. Kliknij nazwę bazy danych, aby otworzyć blok bazy danych.
5. Kliknij pozycję **Dodaj kolekcję**, aby utworzyć kolekcję o identyfikatorze `musicstorecoll`.
6. Kliknij pozycję **Eksplorator dokumentów**.
7. Kliknij pozycję **Przekaż**.
8. Z obszaru **Przekazywanie dokumentu** przejdź do folderu lokalnego, który zawiera pobrane wcześniej pliki w formacie JSON. Wybierz pliki w formacie JSON w partiach po 100 lub mniej.
   * 386.json
   * 387.json
   * . . .
   * 486.json
9. Powtórz ten krok, aby uzyskać kolejną partię plików i powtarzaj go, dopóki nie zostanie przekazany ostatni plik, 669.json.
10. Kliknij pozycję **Eksplorator zapytań**, aby zweryfikować, czy przekazane dane spełniają wymagania Eksploratora dokumentów dotyczące przekazywania.

Możesz to łatwo zrobić, używając zapytania domyślnego, ale możesz też zmodyfikować zapytanie domyślne tak, aby wybierało pierwszych 300 elementów (w tym zestawie danych znajduje się mniej niż 300 elementów).

Powinny zostać wyświetlone dane wyjściowe JSON, począwszy od dokumentu numer 386, a kończąc na dokumencie 669. Po załadowaniu danych możesz [powrócić do kroków przewodnika](#defineDS), aby zbudować indeks, używając **Kreatora importu danych**.

<!--Image references-->
[1]: ./media/search-get-started-portal/AzureSearch-GetStart-Docdbmenu1.png
[2]: ./media/search-get-started-portal/AzureSearch-GetStart-DataSource.png
[3]: ./media/search-get-started-portal/AzureSearch-GetStart-DefaultIndex.png
[4]: ./media/search-get-started-portal/AzureSearch-GetStart-FinishedIndex.png
[5]: ./media/search-get-started-portal/AzureSearch-GetStart-ImportReady.png
[6]: ./media/search-get-started-portal/AzureSearch-GetStart-IndexerList.png
[7]: ./media/search-get-started-portal/search-data-import-wiz-btn.png



<!--HONumber=Nov16_HO2-->


