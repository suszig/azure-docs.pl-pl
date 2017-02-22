---
title: "Importowanie danych do usługi Azure Search przy użyciu indeksatorów w witrynie Azure Portal | Microsoft Docs"
description: "Używanie Kreatora importu danych usługi Azure Search w witrynie Azure Portal w celu przeszukiwania danych z magazynu Azure Blob Storage, magazynu Table Storage, usługi SQL Database oraz programu SQL Server na maszynach wirtualnych platformy Azure."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: Azure Portal
ms.assetid: f40fe07a-0536-485d-8dfa-8226eb72e2cd
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: heidist
translationtype: Human Translation
ms.sourcegitcommit: 702ea254c19d1f9782f83d8445b7f440f11963b9
ms.openlocfilehash: 2d4898694c69b5eb0194747982fa8bfbfea1f6f8


---
# <a name="import-data-to-azure-search-using-the-portal"></a>Importowanie danych do usługi Azure Search przy użyciu portalu
W witrynie Azure Portal na pulpicie nawigacyjnym usługi Azure Search znajduje się kreator **Importuj dane**, który umożliwia ładowanie danych do indeksu. 

  ![Importowanie danych przy użyciu paska poleceń][1]

Kreator wewnętrznie konfiguruje i wywołuje *indeksator*, automatyzując kilka kroków procesu indeksowania: 

* Połączenie z zewnętrznym źródłem danych w bieżącej subskrypcji platformy Azure
* Automatyczne generowanie schematu indeksu na podstawie struktury źródła danych
* Tworzenie dokumentów na podstawie zestawu wierszy pobranego ze źródła danych
* Przekazywanie dokumentów do indeksu w usłudze wyszukiwania

Ten przepływ pracy można wypróbować przy użyciu przykładowych danych w bazie DocumentDB. Instrukcje znajdziesz na stronie [Wprowadzenie do usługi Azure Search w witrynie Azure Portal](search-get-started-portal.md).

## <a name="data-sources-supported-by-the-import-data-wizard"></a>Źródła danych obsługiwane przez Kreatora importu danych
Kreator importu danych obsługuje następujące źródła danych: 

* Usługa Azure SQL Database
* Dane relacyjne programu SQL Server na maszynie wirtualnej platformy Azure
* Azure DocumentDB
* Azure Blob Storage (wersja zapoznawcza)
* Azure Table Storage (wersja zapoznawcza)

Wymaganymi danymi wejściowymi jest spłaszczony zestaw danych. Importu można dokonać tylko z pojedynczej tabeli, widoku bazy danych lub równoważnej struktury danych. Strukturę danych należy utworzyć przed uruchomieniem kreatora.

Uwaga: niektóre indeksatory są dostępne jedynie w wersji zapoznawczej, co oznacza, że definicja indeksatora zależy od wersji zapoznawczej interfejsu API. Więcej informacji oraz linki znajdziesz w temacie [Omówienie indeksatorów](search-indexer-overview.md).

## <a name="connect-to-your-data"></a>Nawiązywanie połączenia z danymi
1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com) i otwórz pulpit nawigacyjny usługi. Możesz kliknąć przycisk **Usługi wyszukiwania** na pasku dostępu, aby zobaczyć listę usług dostępnych w bieżącej subskrypcji. 
2. Kliknij przycisk **Importuj dane** na pasku poleceń, aby otworzyć blok Importuj dane.  
3. Kliknij pozycję **Połącz z danymi**, aby określić definicję źródła danych, z której będzie korzystać indeksator. W przypadku źródeł danych wewnątrz subskrypcji kreator może zwykle wykrywać i odczytywać informacje o połączeniu, co pozwala zminimalizować wymagania dotyczące konfiguracji.

|  |  |
| --- | --- |
| **Istniejące źródło danych** |Jeśli masz już zdefiniowane indeksatory w usłudze wyszukiwania, możesz użyć istniejącej definicji źródła danych na potrzeby innego importu. |
| **Azure SQL Database** |Nazwę usługi, poświadczenia użytkownika z uprawnieniem do odczytu bazy danych i nazwę bazy danych można określić na stronie lub przy użyciu parametrów połączenia ADO.NET. Wybierz opcję parametrów połączenia, aby wyświetlić lub dostosować właściwości. <br/><br/>Na stronie należy określić tabelę lub widok zawierające zestaw wierszy. Ta opcja jest dostępna po udanym nawiązaniu połączenia. Pojawia się wtedy lista rozwijana, z której można dokonać wyboru. |
| **Program SQL Server na maszynie wirtualnej platformy Azure** |Jako parametry połączenia określ w pełni kwalifikowaną nazwę usługi, identyfikator użytkownika, hasło oraz bazę danych. Aby użyć tego źródła danych, należy wcześniej zainstalować w magazynie lokalnym certyfikat szyfrujący połączenie. <br/><br/>Na stronie należy określić tabelę lub widok zawierające zestaw wierszy. Ta opcja jest dostępna po udanym nawiązaniu połączenia. Pojawia się wtedy lista rozwijana, z której można dokonać wyboru. |
| **DocumentDB** |Wymagane jest konto, baza danych i kolekcja. Wszystkie dokumenty w kolekcji zostaną uwzględnione w indeksie. Można zdefiniować zapytanie w celu spłaszczenia lub filtrowania zestawu wierszy bądź wykrywania zmienionych dokumentów na potrzeby późniejszych operacji odświeżania danych. |
| **Azure Blob Storage** |Wymagane jest miedzy innymi konto magazynu i kontener. Opcjonalnie, jeśli nazwa obiektu blob jest zgodna z konwencją nazw wirtualnych do celów grupowania, można określić część nazwy oznaczającą katalog wirtualny jako folder w kontenerze. Więcej informacji zawiera artykuł [Indeksowanie w usłudze Blob Storage (wersja zapoznawcza)](search-howto-indexing-azure-blob-storage.md). |
| **Azure Table Storage** |Wymagane jest miedzy innymi konto magazynu i nazwa tabeli. Opcjonalnie można określić zapytanie w celu pobrania podzbioru tabel. Więcej informacji zawiera artykuł [(Indeksowanie w usłudze Table Storage (wersja zapoznawcza))](search-howto-indexing-azure-tables.md). |

## <a name="customize-target-index"></a>Dostosowywanie indeksu docelowego
Wstępny indeks jest zazwyczaj ustalany na podstawie zestawu danych. Można uzupełnić schemat, dodając, edytując lub usuwając pola. Ponadto można ustawić atrybuty na poziomie pola, aby określić zachowanie podczas późniejszych wyszukiwań.

1. W obszarze **Dostosuj indeks docelowy** określ nazwę i **Klucz** będące unikatowymi identyfikatorami poszczególnych dokumentów. Klucz musi być ciągiem znaków. Jeśli wartości pól zawierają spacje lub kreski, pamiętaj o ustawieniu opcji zaawansowanych w obszarze **Zaimportuj dane** w celu pominięcia weryfikacji tych znaków.
2. Przejrzyj i popraw pozostałe pola. Nazwa i typ pola są zazwyczaj wypełniane automatycznie. Możesz zmienić typ danych.
3. Ustaw atrybuty indeksu dla każdego pola:
   
   * Atrybut Pobieranie umożliwia zwracanie pola w wynikach wyszukiwania.
   * Atrybut Filtrowanie umożliwia przywoływanie pola w wyrażeniach filtru.
   * Atrybut Sortowanie umożliwia używanie pola podczas sortowania.
   * Atrybut Tworzenie aspektów umożliwia używanie pola w nawigacji aspektowej.
   * Atrybut Wyszukiwanie umożliwia wyszukiwanie pełnotekstowe pola.
4. Kliknij kartę **Analizator**, jeśli chcesz określić analizatora języka na poziomie pola. Obecnie można określić tylko analizatory języka. Skorzystanie z analizatorów niestandardowych lub innych niż analizatory języka, takich jak analizatory słów kluczowych, wzorców itp., będzie wymagać kodu.
   
   * Kliknij pozycję **Możliwość wyszukiwania**, aby określić wyszukiwanie pełnotekstowe w polu i włączyć listę rozwijaną Analizator.
   * Wybierz odpowiedni analizator. Zobacz [Create an index for documents in multiple language](search-language-support.md) (Tworzenie indeksu dla dokumentów w wielu językach).
5. Kliknij pozycję **Sugestor**, aby włączyć podpowiedzi pojawiające się w trakcie pisania w wybranych polach.

## <a name="import-your-data"></a>Importowanie danych
1. W obszarze **Zaimportuj dane** podaj nazwę indeksatora. Pamiętaj, że wynikiem działania kreatora importu danych jest indeksator. Jeśli chcesz go później wyświetlić lub edytować, wybierz go z portalu zamiast ponownie uruchamiać kreatora. 
2. Określ harmonogram oparty na strefie czasowej regionu, w którym usługa jest aprowizowana.
3. Ustaw opcje zaawansowane, aby określić progi dotyczące możliwości dalszego indeksowania w przypadku odrzucenia dokumentu. Ponadto możesz określić, czy pola **Klucz** pola mogą zawierać spacje i ukośniki.  

## <a name="edit-an-existing-indexer"></a>Edytowanie istniejącego indeksatora
Na pulpicie nawigacyjnym usługi kliknij dwukrotnie kafelek Indeksator, aby wysunąć listę wszystkich indeksatorów utworzonych dla subskrypcji. Kliknij dwukrotnie jeden z indeksatorów, aby go uruchomić, edytować lub usunąć. Możesz zastąpić indeks innym istniejącym indeksem, zmienić źródło danych i ustawić opcje progów błędów podczas indeksowania.

## <a name="edit-an-existing-index"></a>Edytowanie istniejącego indeksu
W usłudze Azure Search aktualizacje strukturalne indeksu wymagają odbudowania tego indeksu, co obejmuje usunięcie indeksu, ponowne utworzenie indeksu i ponowne załadowanie danych. Aktualizacje strukturalne obejmują zmianę typu danych oraz zmianę nazwy lub usunięcie pola.

Do zmian, które nie wymagają odbudowania indeksu, należą: dodanie nowego pola, zmiana profilów oceniania, zmiana funkcji sugestii i zmiana analizatorów języka. Aby uzyskać więcej informacji, zobacz [Aktualizowanie indeksu](https://msdn.microsoft.com/library/azure/dn800964.aspx).

## <a name="query-an-index-using-search-explorer"></a>Tworzenie zapytań względem indeksu za pomocą Eksploratora wyszukiwania

Portal zawiera **Eksplorator wyszukiwania**, który umożliwia tworzenie zapytań dotyczących indeksu bez konieczności pisania kodu. Eksploratora wyszukiwania można używać dla dowolnego indeksu.

Środowisko wyszukiwania jest oparte na ustawieniach domyślnych, takich jak [prosta składnia](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) i domyślnym parametrze zapytania [searchMode (https://docs.microsoft.com/rest/api/searchservice/search-documents). Wyniki są zwracane w formacie JSON, co umożliwia inspekcję całego dokumentu.

## <a name="next-steps"></a>Następne kroki
Przejrzyj następujące linki, aby dowiedzieć się więcej o indeksatorach:

* [Indeksowanie w usłudze Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Indeksowanie w usłudze DocumentDB](../documentdb/documentdb-search-indexer.md)
* [Indeksowanie w usłudze Blob Storage (wersja zapoznawcza)](search-howto-indexing-azure-blob-storage.md)
* [Indeksowanie w usłudze Table Storage (wersja zapoznawcza)](search-howto-indexing-azure-tables.md)

<!--Image references-->
[1]: ./media/search-import-data-portal/search-import-data-command.png




<!--HONumber=Jan17_HO2-->


