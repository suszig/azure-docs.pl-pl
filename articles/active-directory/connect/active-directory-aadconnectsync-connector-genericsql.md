---
title: "Łącznik usług SQL ogólnego | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób konfigurowania ogólny łącznik SQL firmy Microsoft."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: fd8ccef3-6605-47ba-9219-e0c74ffc0ec9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: billmath
ms.openlocfilehash: 81bacc39d974dfbd1b2aa8dce9e629c508203811
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="generic-sql-connector-technical-reference"></a>Ogólne informacje techniczne Łącznik usług SQL
W tym artykule opisano ogólny łącznik SQL. Artykuł dotyczy następujących produktów:

* Program Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Należy użyć poprawki 4.1.3671.0 lub nowszym [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 i FIM2010R2 łącznika jest dostępna do pobrania z [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

Aby wyświetlić ten łącznik, w akcji, zobacz [ogólny łącznik SQL krok](active-directory-aadconnectsync-connector-genericsql-step-by-step.md) artykułu.

## <a name="overview-of-the-generic-sql-connector"></a>Omówienie łącznika ogólnego SQL
Ogólny łącznik SQL umożliwia integrację usługi synchronizacji z systemu bazy danych, który zapewnia łączność ODBC.  

Z punktu widzenia wysokiego poziomu następujące funkcje są obsługiwane w bieżącej wersji łącznika:

| Funkcja | Pomoc techniczna |
| --- | --- |
| Połączonego źródła danych |Łącznik jest obsługiwana przez wszystkie 64-bitowe sterowniki ODBC. Był testowany z następujących czynności: <li>Program Microsoft SQL Server i SQL Azure</li><li>IBM DB2 10.x</li><li>IBM DB2 9.x</li><li>Oracle 10 i 11 g</li><li>MySQL 5.x</li> |
| Scenariusze |<li>Zarządzanie cyklem życia obiektów</li><li>Zarządzanie hasłami</li> |
| Operacje |<li>Pełny Import i różnicowe importu, eksportu</li><li>Eksportu: Dodawanie, usuwanie, aktualizacji i Zastąp</li><li>Ustawianie hasła, Zmień hasło</li> |
| Schemat |<li>Dynamiczne odnajdowanie obiektów i atrybutów</li> |

### <a name="prerequisites"></a>Wymagania wstępne
Przed użyciem łącznika, upewnij się, że masz następujące czynności na serwerze synchronizacji:

* Framework Microsoft .NET 4.5.2 lub nowszej
* 64-bitowych klienta ODBC — sterowniki

### <a name="permissions-in-connected-data-source"></a>Uprawnienia w połączonego źródła danych
Aby utworzyć lub wykonać dowolną z obsługiwanych czynności w łączniku ogólnego SQL, musi mieć:

* db_datareader
* db_datawriter

### <a name="ports-and-protocols"></a>Porty i protokoły
Porty wymagane przez sterownik ODBC do pracy zapoznaj się dokumentacją dostawcy bazy danych.

## <a name="create-a-new-connector"></a>Utwórz nowy łącznik
Aby utworzyć łącznik ogólnego SQL, w **usługi synchronizacji** wybierz **agenta zarządzania** i **Utwórz**. Wybierz **ogólnego SQL (Microsoft)** łącznika.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>Łączność
Łącznik używa pliku ODBC DSN dla połączenia. Tworzenie przy użyciu pliku DSN **źródeł danych ODBC** dostępnej w menu start w obszarze **narzędzia administracyjne**. W narzędziu administracyjnym, należy utworzyć **pliku DSN** , może zostać dostarczona do łącznika.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/connectivity.png)

Na ekranie łączności jest pierwszy podczas tworzenia nowego łącznika SQL ogólnego. Najpierw należy podać następujące informacje:

* Ścieżka pliku DSN
* Authentication
  * Nazwa użytkownika
  * Hasło

Bazy danych powinna obsługiwać jedną z następujących metod uwierzytelniania:

* **Uwierzytelnianie systemu Windows**: uwierzytelniania bazy danych używa poświadczeń systemu Windows do zweryfikowania użytkownika. Nazwa użytkownika i hasło określone jest używany do uwierzytelniania z bazą danych. To konto wymaga uprawnień do bazy danych.
* **Uwierzytelnianie SQL**: uwierzytelniania używa bazy danych, nazwę użytkownika i hasło zdefiniowana ekranu łączności do połączenia z bazą danych. Jeśli nazwa użytkownika/hasło jest przechowywane w pliku DSN, poświadczenia podane na ekranie połączenia mają pierwszeństwo.
* **Uwierzytelnianie bazy danych SQL Azure**: Aby uzyskać więcej informacji, zobacz [nawiązywanie SQL bazy danych przy użyciu usługi Azure uwierzytelnianie usługi Active Directory](../../sql-database/sql-database-aad-authentication.md).

**Nazwa Wyróżniająca jest zakotwiczenia**: Jeśli wybierzesz tę opcję, nazwa Wyróżniająca jest również używany jako atrybut zakotwiczenia. Może służyć do prostych implementacji, ale również ma następujące ograniczenia:

* Łącznik obsługuje tylko jeden typ obiektu. W związku z tym żadnych atrybutów odwołania może odwoływać się tylko jeden typ obiektów.

**Typ eksportu: Obiekt Zamień**: podczas eksportu, gdy tylko niektóre atrybuty, które zostały zmienione, cały obiekt z wszystkie atrybuty są eksportowane i zastępuje istniejący obiekt.

### <a name="schema-1-detect-object-types"></a>Schemat 1 (Wykryj typy obiektów)
Na tej stronie zamierzasz skonfigurować sposób łącznik ma znaleźć różne typy obiektów w bazie danych.

Każdy typ obiektu jest przedstawiony jako partycja i skonfigurowane dodatkowe na **Konfiguruj partycje i hierarchie**.

![schema1a](./media/active-directory-aadconnectsync-connector-genericsql/schema1a.png)

**Obiekt metody wykrywania typu**: łącznik obsługuje te metody wykrywania typu obiektu.

* **Stała wartość**: Podaj listę typów obiektów z listy rozdzielanej przecinkami. Na przykład: `User,Group,Department`.  
  ![schema1b](./media/active-directory-aadconnectsync-connector-genericsql/schema1b.png)
* **Procedury przechowywanej-tabeli/widoku**: Podaj nazwę tabeli/widoku/przechowywane procedury, a następnie nazwę kolumny, która zawiera listę typów obiektów. Użycie procedury składowanej, określ również parametry dla niego w formacie **[Name]: [kierunek]: [wartość]**. Podaj każdego parametru w osobnym wierszu (Użyj klawiszy Ctrl + Enter, aby uzyskać nowy wiersz).  
  ![schema1c](./media/active-directory-aadconnectsync-connector-genericsql/schema1c.png)
* **Zapytanie SQL**: Ta opcja umożliwia podanie zapytanie SQL, która zwraca pojedynczą kolumnę o typów obiektów, na przykład `SELECT [Column Name] FROM TABLENAME`. Zwrócony kolumna musi być typu String (varchar).

### <a name="schema-2-detect-attribute-types"></a>Schemat 2 (typy atrybutów Wykryj)
Na tej stronie zamierzasz skonfigurować sposób atrybutu nazwy i typy mają zostać wykryte. Opcje konfiguracji są wyświetlane dla każdego typu obiektu wykryte na poprzedniej stronie.

![schema2a](./media/active-directory-aadconnectsync-connector-genericsql/schema2a.png)

**Atrybut metody wykrywania typu**: łącznik obsługuje te metody wykrywania typu atrybutu z każdym typem obiektu wykrytych na ekranie 1 schematu.

* **Procedury przechowywanej-tabeli/widoku**: Podaj nazwę tabeli/widoku/przechowywane procedury, które mają być używane, aby znaleźć nazwy atrybutu. Użycie procedury składowanej, określ również parametry dla niego w formacie **[Name]: [kierunek]: [wartość]**. Podaj każdego parametru w osobnym wierszu (Użyj klawiszy Ctrl + Enter, aby uzyskać nowy wiersz). Do wykrywania atrybutu nazwy w atrybutu wielowartościowego, podaj rozdzielaną przecinkami listę tabel lub widoków. Wielowartościowy scenariusze nie są obsługiwane podczas ma tej samej nazwy kolumn w tabeli nadrzędnej i podrzędnej.
* **Zapytanie SQL**: Ta opcja umożliwia podanie zapytanie SQL, która zwraca pojedynczą kolumnę o nazwach atrybutów, na przykład `SELECT [Column Name] FROM TABLENAME`. Zwrócony kolumna musi być typu String (varchar).

### <a name="schema-3-define-anchor-and-dn"></a>Schemat 3 (zdefiniuj zakotwiczenia i DN)
Ta strona umożliwia konfigurowanie zakotwiczenia i atrybut nazwy domeny dla każdego typu obiektu wykryte. Możesz wybrać wiele atrybutów, aby zapewnić unikatowy zakotwiczenia.

![schema3a](./media/active-directory-aadconnectsync-connector-genericsql/schema3a.png)

* Atrybuty wielowartościowe i logiczna nie są wyświetlane.
* Tego samego atrybutu nie można użyć nazwy domeny i zakotwiczenia, chyba że **nazwa Wyróżniająca jest zakotwiczenia** jest zaznaczona na stronie łączność.
* Jeśli **nazwa Wyróżniająca jest zakotwiczenia** jest zaznaczona na stronie łączność ta strona wymaga tylko atrybut nazwy domeny. Ten atrybut będzie również służyć jako atrybut zakotwiczenia.

  ![schema3b](./media/active-directory-aadconnectsync-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>Schemat 4 (Zdefiniuj typ atrybutu, odwołania i kierunek)
Ta strona umożliwia konfigurowanie typ atrybutu, takie jak liczba całkowita, binary, lub wartość logiczną i kierunek dla każdego atrybutu. Wszystkie atrybuty ze strony **schematu 2** są wymienione w tym atrybutów wielowartościowych.

![schema4a](./media/active-directory-aadconnectsync-connector-genericsql/schema4a.png)

* **Typ danych**: służy do mapowania typ atrybutu do tych typów znane przez aparat synchronizacji. Wartość domyślna to można używać tego samego typu, jak wykryto w schemacie SQL, ale nie są łatwo odwołanie i DateTime. Dla osób, należy określić **DateTime** lub **odwołania**.
* **Kierunek**: importu, eksportu lub ImportExport można ustawić kierunku atrybutu. ImportExport jest domyślnym.

![schema4b](./media/active-directory-aadconnectsync-connector-genericsql/schema4b.png)

Uwagi:

* Jeśli typem atrybutu nie jest wykrywalny przez łącznik, używa String — typ danych.
* **Zagnieżdżone tabele** można uznać za tabel bazy danych z jednej kolumny. Oracle przechowuje wierszy zagnieżdżonej tabeli w określonej kolejności. Jednak po pobraniu zagnieżdżonej tabeli w zmiennej PL/SQL wiersze są podane kolejnych indeksy dolne rozpoczyna się od 1. Który umożliwia dostęp tablicy do poszczególnych wierszy.
* **VARRYS** nie są obsługiwane w łączniku.

### <a name="schema-5-define-partition-for-reference-attributes"></a>Schemat 5 (zdefiniuj partycja dla atrybutów odwołania)
Na tej stronie można skonfigurować dla wszystkich atrybutów odwołania, które partycji (typ obiektu) odwołuje się atrybut do.

![schema5](./media/active-directory-aadconnectsync-connector-genericsql/schema5.png)

Jeśli używasz **nazwa Wyróżniająca jest zakotwiczenia**, musi używać tego samego typu obiektu, odwołujesz się z. Nie można odwołać obiektu innego typu.

>[!NOTE]
Począwszy od aktualizacji 2017 marca jest teraz opcję "*", kiedy ta opcja jest wybrana, a następnie wszystkie typy elementów członkowskich możliwe zostanie zaimportowane.

![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/any-option.png)

>[!IMPORTANT]
 Począwszy od 2017 maja "*" alias **każda opcja** został zmieniony na obsługuje importowania i eksportowania przepływu. Jeśli chcesz użyć tej opcji wielowartościowe tabeli/widoku powinien mieć atrybut, który zawiera typ obiektu.

![](./media/active-directory-aadconnectsync-connector-genericsql/any-02.png)

 </br> Jeśli "*" jest zaznaczona, a następnie również należy określić nazwę kolumny z typem obiektu.</br> ![](./media/active-directory-aadconnectsync-connector-genericsql/any-03.png)

Po zaimportowaniu zostanie wyświetlony podobny do obraz poniżej:

  ![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/after-import.png)



### <a name="global-parameters"></a>Parametry globalne
Strona parametry globalne służy do konfigurowania Import zmian, format daty i godziny i metody hasła.

![globalparameters1](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters1.png)



Ogólny łącznik SQL obsługuje następujące metody Import zmian:

* **Wyzwalacz**: zobacz [generowania widoków zmian za pomocą wyzwalaczy](https://technet.microsoft.com/library/cc708665.aspx).
* **Znak wodny**: ogólne podejście, które mogą być używane z dowolną bazą danych. Zapytanie znaku wodnego jest wstępnie wypełniony oparte na dostawcy bazy danych. Kolumna znaku wodnego musi być obecny na każdym tabelę lub widok używane. Ta kolumna musi śledzić wstawia i aktualizacji tabel, jak i jego zależne od (wielowartościowe lub podrzędnej) tabel. Należy zsynchronizować zegary między usługą synchronizacji i serwer bazy danych. Jeśli nie może zostać pominięte niektóre wpisy import zmian.  
  Ograniczenie:
  * Znak wodny strategii nie nie obiekty usunięte pomocy technicznej.
* **Migawki**: (działa tylko z programem Microsoft SQL Server) [generowanie widoków zmian przy użyciu migawek](https://technet.microsoft.com/library/cc720640.aspx)
* **Śledzenie zmian**: (działa tylko z programem Microsoft SQL Server) [o śledzenie zmian](https://msdn.microsoft.com/library/bb933875.aspx)  
  Ograniczenia:
  * Zakotwiczenia & Nazwa Wyróżniająca atrybutu musi być częścią klucza podstawowego dla wybranego obiektu w tabeli.
  * Zapytania SQL nie jest obsługiwane podczas importowania i eksportowania z śledzenia zmian.

**Dodatkowe parametry**: Określ strefę czasową serwera bazy danych, wskazującą, w którym znajduje się serwer bazy danych. Ta wartość jest używana do obsługi różnych formatów daty i godziny atrybutów.

Łącznik zawsze przechowuje Data i godzina datę w formacie UTC. Aby można było poprawnie przekonwertować daty i godziny, strefy czasowej serwera bazy danych i format używany musi być określona. Format powinien zostać przedstawiony w formacie .net.

Podczas eksportowania do łącznika w formacie czasu UTC należy podać każdego atrybutu czasu daty.

![globalparameters2](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters2.png)

**Hasło konfiguracji**: łącznika zawiera funkcje synchronizacji hasła i obsługuje ustawić i zmienić hasło.

Łącznik udostępnia dwie metody w celu obsługi synchronizacji haseł:

* **Procedura składowana**: Ta metoda wymaga dwóch procedur składowanych do obsługi zestawu & zmiany hasła. Wszystkie parametry dla dodatku typu i Zmień hasło operacji w **ustawić hasło SP** i **zmienić hasło SP** parametrów odpowiednio jak na poniższym przykładzie.
  ![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters3.png)
* **Rozszerzenie hasło**: Ta metoda wymaga hasła rozszerzenia DLL (należy podać nazwę biblioteki DLL rozszerzenia, która implementuje [IMAExtensible2Password](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx) interfejs). Hasła zestawu rozszerzenia muszą znajdować się w folderze rozszerzenia, dzięki czemu łącznika można załadować biblioteki DLL w czasie wykonywania.
  ![globalparameters4](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters4.png)

Należy również włączyć zarządzanie hasłami w **skonfigurować rozszerzenia** strony.
![globalparameters5](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfiguruj partycje i hierarchie
Na stronie partycje i hierarchie wybierz wszystkie typy obiektów. Każdy typ obiektu jest jego własnej partycji.

![partitions1](./media/active-directory-aadconnectsync-connector-genericsql/partitions1.png)

Możesz też przesłonić wartości zdefiniowanych w **łączności** lub **parametry globalne** strony.

![partitions2](./media/active-directory-aadconnectsync-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>Konfiguruj zakotwiczenia
Ta strona jest tylko do odczytu, ponieważ zakotwiczenia został już zdefiniowany. Atrybut zakotwiczenia wybrany jest zawsze dołączany z typem obiektu w celu zapewnienia, że pozostaje on unikatowy różnych typów obiektów.

![punkty kontrolne](./media/active-directory-aadconnectsync-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>Konfigurowanie parametrów kroku wykonywania
Te kroki są konfigurowane na profile uruchamiania dla łącznika. Te konfiguracje wykonują rzeczywistą pracę importowania i eksportowania danych.

### <a name="full-and-delta-import"></a>Pełne i różnicowe importu
Ogólny Łącznik usług SQL wsparcie pełne i Import zmian za pomocą następujących metod:

* Tabela
* Widok
* Procedura składowana
* Zapytanie SQL

![runstep1](./media/active-directory-aadconnectsync-connector-genericsql/runstep1.png)

**Tabela/Widok**  
Aby zaimportować atrybuty wielowartościowe dla obiekt, należy podać nazwę tabeli lub widoku rozdzielone przecinkami w **widoków tabel nazwy z wieloma wartościami** i warunki sprzężenia odpowiednich w **warunek sprzężenia** z tabelą nadrzędną.

Przykład: Chcesz zaimportować obiekt pracowników i jego atrybuty wielowartościowe. Istnieją dwie tabele, o nazwie pracownika (główna tabela) i działu (wielowartościowe).
Wykonaj następujące czynności:

* Typ **pracownika** w **tabeli/widoku/SP**.
* Dział typu **widoków tabel nazwy z wieloma wartościami**.
* Wpisz warunek sprzężenia między pracownika & działu w **warunek sprzężenia**, na przykład `Employee.DEPTID=Department.DepartmentID`.
  ![runstep2](./media/active-directory-aadconnectsync-connector-genericsql/runstep2.png)

**Procedury składowane**  
![runstep3](./media/active-directory-aadconnectsync-connector-genericsql/runstep3.png)

* Jeśli masz dużą ilość danych, zaleca się wdrożenie podział na strony z własnych procedur przechowywanych.
* Dla procedury przechowywane do obsługi podział na strony musisz podać Start zakończenia indeks i. Zobacz: [wydajnie stronicowania z dużą ilością danych](https://msdn.microsoft.com/library/bb445504.aspx).
* @StartIndexi @EndIndex zastąpione w czasie wykonywania wartość rozmiaru strony odpowiednich skonfigurowanych na **kroku skonfigurować** strony. Na przykład, gdy łącznik pobiera pierwszej strony i rozmiar strony ma wartość 500, w takiej sytuacji @StartIndex 1 i @EndIndex 500. Zwiększenie tych wartości, gdy łącznik pobiera kolejnych stronach i zmień @StartIndex & @EndIndex wartość.
* Aby wykonać procedury składowanej sparametryzowane, należy podać parametry w `[Name]:[Direction]:[Value]` format. Wprowadź każdy parametr w osobnym wierszu (Użyj klawiszy Ctrl + Enter, aby uzyskać nowy wiersz).
* Ogólny Łącznik usług SQL obsługuje również operacji importu z połączonych serwerów w programie Microsoft SQL Server. Jeśli mają być pobrane informacje z tabeli w połączonej serwera, tabeli, powinny być podawane w formacie:`[ServerName].[Database].[Schema].[TableName]`
* Ogólny łącznik SQL obsługuje tylko te obiekty, które mają podobną strukturę (zarówno alias nazwę i typ danych) między wykonanie kroków wykrywania informacji i schematu. Jeśli wybrany obiekt ze schematu oraz informacje podane podczas wykonywania czynności jest inny, Łącznik usług SQL jest nie obsługuje tego typu scenariuszy.

**Zapytanie SQL**  
![runstep4](./media/active-directory-aadconnectsync-connector-genericsql/runstep4.png)

![runstep5](./media/active-directory-aadconnectsync-connector-genericsql/runstep5.png)

* Kwerend nieobsługiwana zestawy wielu wyników.
* Zapytanie SQL obsługuje podział na strony i podaj start zakończenia indeks i jako zmienną do obsługi podział na strony.

### <a name="delta-import"></a>Import zmian
![runstep6](./media/active-directory-aadconnectsync-connector-genericsql/runstep6.png)

Konfiguracja importu zmian wymaga pewnych dodatkowych czynności konfiguracyjnych w porównaniu z pełny Import.

* Jeśli wybierzesz wyzwalacza lub migawki podejście do śledzenia zmian różnicowych, należy podać tabeli historii lub migawki bazy danych w **nazwę bazy danych lub tabeli historii migawki** pole.
* Należy również podać warunek sprzężenia między tabelą historii i tabelą nadrzędną, na przykład`Employee.ID=History.EmployeeID`
* Aby śledzić tabeli historii transakcji w tabeli nadrzędnej, należy podać nazwę kolumny, który zawiera informacje o operacji (Dodaj/aktualizowania/usuwania).
* Jeśli wybierzesz znaku wodnego, aby śledzić zmiany różnicowe, należy podać nazwę kolumny, który zawiera informacje o operacji w **nazwa kolumny znacznik limitu górnego**.
* **Zmienić atrybut typu** kolumny jest wymagana dla typu zmiany. W tej kolumnie mapuje zmiany, która występuje w tabeli podstawowej lub tabela wielu wartości na typ zmiany w widoku delta. Ta kolumna zawiera typ zmiany Modify_Attribute Zmień poziom atrybutu lub dodawanie, modyfikowanie, lub usuń zmienić typu dla typu zmiany na poziomie obiektu. Jeśli jest inny niż wartość domyślna dodawania, modyfikowania, lub przycisk Usuń, a następnie można zdefiniować te wartości przy użyciu tej opcji.

### <a name="export"></a>Eksportowanie
![runstep7](./media/active-directory-aadconnectsync-connector-genericsql/runstep7.png)

Ogólny łącznik SQL obsługuje eksportu przy użyciu czterech obsługiwanych metod, takich jak:

* Tabela
* Widok
* Procedura składowana
* Zapytanie SQL

**Tabela/Widok**  
Jeśli zostanie wybrana opcja tabelę lub widok, łącznik generuje odpowiednich zapytań w celu eksportu.

**Procedury składowane**  
![runstep8](./media/active-directory-aadconnectsync-connector-genericsql/runstep8.png)

Jeśli wybierzesz opcję procedury składowanej eksportu wymaga trzy różne procedury przechowywane w celu wykonania operacji wstawiania/aktualizacji/usuwania.

* **Dodaj nazwę SP**: SP ten jest uruchamiany, jeśli dowolny obiekt jest dostarczany do łącznika do wstawienia w odpowiedniej tabeli.
* **Aktualizacja nazwy SP**: SP ten jest uruchamiany, jeśli dowolny obiekt zawiera łącznik na aktualizację w odpowiedniej tabeli.
* **Usuń nazwę SP**: SP ten jest uruchamiany, jeśli dowolny obiekt jest dostarczany do łącznika do usunięcia w odpowiedniej tabeli.
* Atrybut z Schemat używany jako wartość parametru procedury składowanej. Na przykład `@EmployeeName: INPUT: EmployeeName` (EmployeeName jest zaznaczona w schemacie łącznika i łącznik zastępuje odpowiednią wartość podczas wykonywania eksportu)
* Aby uruchomić sparametryzowane procedury składowanej, należy podać parametry w `[Name]:[Direction]:[Value]` format. Wprowadź każdy parametr w osobnym wierszu (Użyj klawiszy Ctrl + Enter, aby uzyskać nowy wiersz).

**Zapytanie SQL**  
![runstep9](./media/active-directory-aadconnectsync-connector-genericsql/runstep9.png)

Jeśli wybierzesz opcję zapytania SQL eksportu wymaga trzy różne zapytania w celu wykonania operacji wstawiania/aktualizacji/usuwania.

* **Wstaw kwerendę**: to zapytanie jest uruchamiany, jeśli dowolny obiekt jest dostarczany do łącznika do wstawienia w odpowiedniej tabeli.
* **Zaktualizuj zapytanie**: to zapytanie jest uruchamiany, jeśli dowolny obiekt zawiera łącznik na aktualizację w odpowiedniej tabeli.
* **Usuń kwerendę**: to zapytanie jest uruchamiany, jeśli dowolny obiekt jest dostarczany do łącznika do usunięcia w odpowiedniej tabeli.
* Atrybut wybrane ze schematu używane jako wartości parametru do zapytania, na przykład`Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>Rozwiązywanie problemów
* Aby uzyskać informacje o tym, jak włączyć rejestrowanie rozwiązywać problemy z łącznika, zobacz [jak włączyć ETW Tracing łączników](http://go.microsoft.com/fwlink/?LinkId=335731).
