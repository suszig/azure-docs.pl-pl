<properties
    pageTitle="Omówienie bazy danych Stretch | Microsoft Azure"
    description="Informacje na temat sposobu, w jaki baza danych Stretch migruje dane historyczne w sposób niewidoczny i bezpieczny do chmury Microsoft Azure."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager=""
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/17/2016"
    ms.author="douglasl"/>

# Omówienie bazy danych Stretch

Baza danych Stretch migruje dane historyczne w sposób niewidoczny i bezpieczny do chmury Microsoft Azure.

Jeśli chcesz od razu rozpocząć pracę z bazą danych Stretch, zobacz artykuł [Get started by running the Enable Database for Stretch Wizard](sql-server-stretch-database-wizard.md) (Rozpoczęcie pracy przez uruchomienie Kreatora włączania bazy danych do usługi Stretch).

## Jakie są zalety bazy danych Stretch?
Baza danych Stretch zapewnia następujące korzyści:

**Opłacalną\- dostępność zimnych danych** Dynamiczne rozciąganie ciepłych i zimnych danych transakcyjnych z programu SQL Server na platformę Microsoft Azure za pomocą bazy danych SQL Server Stretch. W odróżnieniu od typowych magazynów zimnych danych dane są zawsze online i dostępne dla zapytań. Można tanim kosztem zapewnić przechowywanie przez dłuższy danych z dużych tabel, np. historii zamówień klienta. Skorzystaj z taniej platformy Azure, zamiast skalować drogi magazyn lokalny. Aby zachować kontrolę nad kosztami, należy wybrać warstwę cenową i skonfigurować ustawienia w portalu Azure. Skaluj w górę lub w dół w zależności od potrzeb. Więcej szczegółów znajduje się w witrynie [SQL Server Stretch Database Pricing](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/) (Ceny bazy danych SQL Server Stretch)

**Nie wymaga zmian w zapytaniach ani aplikacjach** Płynnie uzyskuj dostęp do danych programu SQL Server — lokalnych i rozciągniętych do chmury.  Ustaw zasady określające, gdzie są przechowywane dane, a program SQL Server będzie obsługiwać ruch danych w tle. Cała tabela jest zawsze online oraz umożliwia zadawanie zapytań. Baza danych Stretch nie wymaga żadnych zmian w istniejących zapytaniach ani aplikacjach — lokalizacja danych jest całkowicie niewidoczna dla aplikacji.

**Usprawnia lokalną konserwację danych** Ogranicz lokalną konserwację i magazynowanie danych. Kopie zapasowe porcji danych w chmurze są tworzone automatycznie. Tworzenie kopii zapasowych danych lokalnych jest szybsze i nie trwa dłużej niż czynności konserwacyjne. Wymagania magazynu lokalnego zostają znacznie ograniczone. Magazyn Azure może być o 80% tańszy niż dodawanie lokalnych dysków SSD.

**Zapewnia bezpieczeństwo danych nawet podczas migracji** Możesz bez obaw rozciągnąć najważniejsze aplikacje do chmury. Opcja Zawsze zaszyfrowane programu SQL Server zapewnia szyfrowanie danych w ruchu. Zabezpieczenia na poziomie wiersza oraz inne zaawansowane funkcje zabezpieczeń programu SQL Server również współpracują z bazą danych Stretch w celu ochrony danych.

## Do czego służy baza danych Stretch?
Po włączeniu bazy danych Stretch dla wystąpienia programu SQL Server, bazy danych i co najmniej jednej tabeli zaczyna ona w trybie dyskretnym migrować dane historyczne na platformę Azure.

-   W przypadku przechowywania danych historycznych w osobnej tabeli można migrować całą tabelę.

-   Jeśli tabela zawiera zarówno dane historyczne jak i bieżące, można określić predykat filtru, aby wybrać wiersze do migracji.

Baza danych Stretch gwarantuje, że jeśli podczas migracji wystąpi błąd, żadne dane nie zostaną utracone. Obejmuje ona również logikę ponawiania do obsługi problemów z połączeniem, które mogą wystąpić podczas migracji. Widok dynamiczny zarządzania zawiera informacje o stanie migracji.

Można wstrzymać migrację danych w celu rozwiązania problemów na serwerze lokalnym lub w celu maksymalnego zwiększenia dostępnej przepustowości sieci.

Nie trzeba wprowadzać zmian w istniejących zapytaniach ani aplikacjach klienckich. Można nadal mieć stały dostęp zarówno do danych lokalnych, jak i zdalnych — nawet podczas migracji danych. W przypadku zapytań zdalnych występuje niewielkie opóźnienie, ale ma ono miejsce tylko, gdy użytkownik przesyła zapytanie dotyczące danych historycznych.

![Omówienie bazy danych Stretch][StretchOverviewImage1]

## Czy potrzebujesz bazy danych Stretch?
Jeśli następujące stwierdzenia są prawdziwe, baza danych Stretch może ułatwić spełnienie wymagań i rozwiązanie problemów.

|Jesteś osobą podejmującą decyzje|Jesteś administratorem baz danych|
|------------------------------|-------------------|
|Muszę przez długi czas przechowywać dane transakcyjne.|Nie panuję już nad rozmiarem tabel.|
|Czasami muszę wykonywać zapytania dotyczące danych historycznych.|Moi użytkownicy mówią, że zależy im na dostępie do danych historycznych, ale rzadko ich używają.|
|Mam aplikacje, w tym starsze wersje, których nie chcę aktualizować.|Muszę ciągle kupować i dodawać więcej miejsca do magazynowania.|
|Chcę znaleźć sposób, aby zaoszczędzić na magazynie.|Nie mogę utworzyć kopii zapasowej ani przywrócić tak dużych tabel w ramach umowy SLA.|

## Jakiego typu bazy danych i tabele nadają się do bazy danych Stretch?
Baza danych Stretch obsługuje transakcyjne bazy danych z dużą ilością danych historycznych, zazwyczaj przechowywanych w małej liczbie tabel. Tabele te mogą zawierać ponad miliard wierszy.

Jeśli używasz funkcji tabeli danych czasowych programu SQL Server 2016, użyj bazy danych Stretch do migrowania całości lub części skojarzonej tabeli historii do opłacalnego magazynu na platformie Azure. Więcej informacji można znaleźć w artykule [Manage Retention of Historical Data in System-Versioned Temporal Tables](https://msdn.microsoft.com/library/mt637341.aspx) (Zarządzanie przechowywaniem danych historycznych w tabelach tymczasowych wersjonowanych w systemie).

Użyj Doradcy w zakresie bazy danych Stretch, funkcji Doradcy uaktualnienia do programu SQL Server 2016, w celu identyfikacji baz danych i tabel dla bazy danych Stretch. Więcej informacji znajduje się w artykule [Identify databases and tables for Stretch Database](sql-server-stretch-database-identify-databases.md) (Identyfikowanie baz danych i tabel dla bazy danych Stretch). Aby dowiedzieć się więcej na temat potencjalnych problemów z blokowaniem, zobacz artykuł [Surface area limitations and blocking issues for Stretch Database](sql-server-stretch-database-limitations.md) (Ograniczenia obszaru powierzchni i problemy z blokowaniem dotyczące bazy danych Stretch).

## <a name="FAQ"></a>Często zadawane pytania dotyczące bazy danych Stretch
**Czy baza danych Stretch działa z &lt;nazwa funkcji programu SQL Server&gt;?**
-   Lista funkcji programu SQL Server, które sprawiają, że tabela nie ma uprawnień pozwalających na jej użycie w bazie danych Stretch, znajduje się w artykule [Surface area limitations and blocking issues for Stretch Database](sql-server-stretch-database-limitations.md) (Ograniczenia obszaru powierzchni i problemy z blokowaniem dotyczące bazy danych Stretch).

-   Możesz pobrać Doradcę uaktualnienia do programu SQL Server 2016 i uruchomić Doradcę bazy danych Stretch, aby zidentyfikować bazy danych i tabele przeznaczone do bazy danych Stretch. Więcej informacji znajduje się w artykule [Identify databases and tables for Stretch Database](sql-server-stretch-database-identify-databases.md) (Identyfikowanie baz danych i tabel dla bazy danych Stretch).

**Czy mogę wskazać inne lokalne wystąpienie programu SQL Server dla bazy danych Stretch?**
Nie. Baza danych Stretch nie obsługuje innego lokalnego wystąpienia programu SQL Server w roli zdalnego punktu końcowego.

**Czy mogę wyłączyć bazę danych Stretch i przenieść zmigrowane dane z powrotem do lokalnej tabeli?**
Tak. Więcej informacji znajduje się w artykule [Disable Stretch Database and bring back remote data](sql-server-stretch-database-disable.md) (Wyłączanie bazy danych Stretch i przywracanie danych zdalnych).

## Warunki
**Lokalna baza danych** Lokalna baza danych programu SQL Server.

**Zdalny punkt końcowy**. Lokalizacja na platformie Microsoft Azure zawierająca dane zdalne bazy danych.

**Dane lokalne**. Dane w bazie danych z włączoną bazą danych Stretch, które nie zostaną przeniesione na platformę Azure na podstawie konfiguracji bazy danych Stretch.

**Dane z uprawnieniami**. Dane w bazie danych z włączoną bazą danych Stretch, które jeszcze nie zostały, ale zostaną przeniesione do platformy Azure na podstawie konfiguracji bazy danych Stretch.

**Dane zdalne**. Dane w bazie danych z włączoną funkcją bazy danych Stretch, które zostały już przeniesione do platformy Azure.

## Architektura
Baza danych Stretch korzysta z zasobów na platformie Microsoft Azure, aby odciążać magazyn archiwizacji danych i przetwarzanie zapytań.

Po włączeniu bazy danych Stretch w bazie danych tworzy ona bezpiecznie połączoną definicję serwera w lokalnej \- wersji programu SQL Server. Obiektem docelowym tej połączonej definicji serwera jest zdalny punkt końcowy. Po włączeniu bazy danych Stretch w tabeli w bazie danych dostarcza ona zasoby zdalne i rozpoczyna migrację danych z uprawnieniami, jeśli funkcja migracji została włączona.

Zapytania dotyczące tabel z włączoną bazą danych Stretch są automatycznie uruchamiane dla lokalnej bazy danych i zdalnego punktu końcowego. Baza danych Stretch korzysta z funkcji przetwarzania na platformie Azure, aby uruchamiać zapytania dla danych zdalnych, zapisując je ponownie. Ponowne zapisanie jest widoczne w formie operatora „zapytania zdalnego” w nowym planie zapytań.

![Architektura bazy danych Stretch][StretchOverviewImage2]

## Zabezpieczenia i uprawnienia

### Włączanie i wyłączanie bazy danych Stretch dla wystąpienia programu SQL Server
Aby rozpocząć konfigurowanie baz danych dla bazy danych Stretch, należy najpierw zmienić opcję konfiguracji na poziomie wystąpienia „archiwum danych zdalnych” przy użyciu sp\_configure. Ta operacja wymaga uprawnień administratora systemu lub administratora serwera. Po włączeniu tej opcji można skonfigurować bazy danych dla bazy danych Stretch, migrować dane i przesyłać zapytania o dane w zdalnym punkcie końcowym.

### Włączanie i wyłączanie bazy danych Stretch dla bazy danych lub tabeli
Aby skonfigurować bazę danych dla bazy danych Stretch, trzeba mieć uprawnienia do KONTROLI BAZY DANYCH. Ponadto należy podać poświadczenia administratora zdalnego punktu końcowego.

Aby skonfigurować tabelę dla bazy danych Stretch, musisz mieć w tabeli uprawnienie ZMIANY i baza danych musi już być skonfigurowana dla bazy danych Stretch.

### Dostęp do danych
Tylko procesy systemowe mogą uzyskać dostęp do połączonej definicji serwera w ramach bazy danych Stretch. Identyfikatory logowania użytkownika nie mogą wystawiać zapytań za pomocą połączonej definicji serwera dla zdalnego punktu końcowego.

Baza danych Stretch nie zmienia modelu uprawnień istniejącej bazy danych. Identyfikatory logowania użytkownika mogą wysyłać zapytania o dane w tabeli z włączoną bazą danych Stretch przy użyciu lokalnej bazy danych. Lokalna baza danych sprawdza uprawnienia dla każdej akcji zainicjowanej przez użytkownika w taki sam sposób jak w przypadku innych obiektów. Jeśli masz uprawnienia dostępu do tabeli, a baza danych Stretch jest włączona, masz dostęp do całej jej zawartości, do której masz uprawnienia, bez względu na to, gdzie fizycznie znajdują się dane.

![Model dostępu do danych bazy danych Stretch][StretchOverviewImage3]

## Testowanie bazy danych Stretch
**Przetestuj bazę danych Stretch za pomocą przykładowej bazy danych AdventureWorks.** Aby pobrać przykładową bazę danych AdventureWorks, pobierz [stąd](https://www.microsoft.com/download/details.aspx?id=49502) przynajmniej plik bazy danych oraz plik przykładów i skryptów. Po przywróceniu przykładowej bazy danych do wystąpienia programu SQL Server 2016 rozpakuj plik przykładów i otwórz plik przykładów bazy danych Stretch z folderu bazy danych Stretch. Uruchom skrypty w tym pliku, aby sprawdzić, jaką przestrzeń zajmują dane przed włączeniem bazy danych Stretch i po jej włączeniu, śledzić postęp migracji danych i potwierdzić, że można nadal wysyłać zapytania o istniejące dane oraz wstawiać nowe dane podczas i po migracji.

## Następny krok
**Zidentyfikuj bazy danych i tabele przeznaczone do bazy danych Stretch.** Pobierz Doradcę uaktualnienia do programu SQL Server 2016 i uruchom Doradcę bazy danych Stretch, aby zidentyfikować bazy danych i tabele przeznaczone do bazy danych Stretch. Doradca bazy danych Stretch identyfikuje również problemy z blokowaniem. Więcej informacji znajduje się w artykule [Identify databases and tables for Stretch Database](sql-server-stretch-database-identify-databases.md) (Identyfikowanie baz danych i tabel dla bazy danych Stretch).

<!--Image references-->
[StretchOverviewImage1]: ./media/sql-server-stretch-database-overview/StretchDBOverview.png
[StretchOverviewImage2]: ./media/sql-server-stretch-database-overview/StretchDBOverview1.png
[StretchOverviewImage3]: ./media/sql-server-stretch-database-overview/StretchDBOverview2.png



<!--HONumber=Jun16_HO2-->


