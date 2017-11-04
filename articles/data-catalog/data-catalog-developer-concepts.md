---
title: "Koncepcje dla deweloperów wykazu danych | Dokumentacja firmy Microsoft"
description: "Wprowadzenie do podstawowych pojęć w modelu koncepcyjnym wykaz danych Azure, jako dostępnego za pośrednictwem interfejsu API REST katalogu."
services: data-catalog
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
tags: 
ms.assetid: 89de9137-a0a4-40d1-9f8d-625acad31619
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 10/15/2017
ms.author: spelluru
ms.openlocfilehash: e3c26c2358c15d18c71b82fe1f389c039ecbd97b
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="azure-data-catalog-developer-concepts"></a>Koncepcje dla deweloperów usługi Azure Data Catalog
Microsoft **Azure Data Catalog** to usługa w chmurze pełni zarządzana, która zapewnia możliwości odnajdywanie źródła danych oraz crowdsourcing metadanych źródła danych. Deweloperzy mogą używać usługi za pomocą ich interfejsów API REST. Zapoznanie się z pojęciami dotyczącymi usługi jest ważne dla deweloperów pomyślnie integracji z **Azure Data Catalog**.

## <a name="key-concepts"></a>Kluczowe pojęcia
**Azure Data Catalog** modelu koncepcyjnego opiera się na cztery podstawowe pojęcia: **katalogu**, **użytkowników**, **zasoby**, i **adnotacje**.

![Pojęcia][1]

*Rysunek 1 - Azure Data Catalog uproszczony model koncepcyjny*

### <a name="catalog"></a>Katalogu
A **katalogu** jest kontenerem najwyższego poziomu dla wszystkich metadanych, które są przechowywane w organizacji. Istnieje **katalogu** dozwolone na konto platformy Azure. Katalogi są powiązane z subskrypcją platformy Azure, ale tylko jeden **katalogu** mogą być tworzone dla danego konta Azure, nawet jeśli konta może mieć wiele subskrypcji.

Katalog zawiera **użytkowników** i **zasoby**.

### <a name="users"></a>Użytkownicy
Użytkownicy są podmiotów zabezpieczeń, które mają uprawnienia do wykonywania akcji (wyszukiwanie w katalogu, dodać, edytować lub usunąć elementy, itp.) w katalogu.

Istnieje kilka różnych ról, które użytkownik może mieć. Uzyskać na rolach zobacz sekcję ról i autoryzacji.

Można dodać poszczególnych użytkowników i grup zabezpieczeń.

Azure Data Catalog używa usługi Azure Active Directory do zarządzania tożsamościami i dostępem. Każdy użytkownik wykazu musi należeć do usługi Active Directory dla konta.

### <a name="assets"></a>Elementy zawartości
A **katalogu** zawiera zasobów danych. **Zasoby** jednostki zarządzane przez wykaz stopnia szczegółowości.

Poziom szczegółowości zasób zależy od źródła danych. Dla serwera SQL lub bazą danych Oracle zasób może być tabelą ani widokiem. Dla usług SQL Server Analysis Services zasób może być miary, wymiaru lub kluczowego wskaźnika wydajności (KPI). SQL Server Reporting Services zasób jest raportem.

**Zasobów** jest operacją dodawane lub usuwane z wykazu. Jest to jednostka wynik odzyskać z **wyszukiwania**.

**Zasobów** składa się z nazwy, lokalizacji oraz typ i adnotacje opisywać go.

### <a name="annotations"></a>Adnotacje
Adnotacje są elementy, które reprezentują metadanych dotyczących zasobów.

Przykłady adnotacje opisu, znaczników, schemat, dokumentacji itp. Pełną listę typów zasobów i typów adnotacji znajdują się w sekcji modelu obiektu trwałego.

## <a name="crowdsourcing-annotations-and-user-perspective-multiplicity-of-opinion"></a>Adnotacje Crowdsourcing i perspektywy użytkownika (liczebność opinii)
Kluczowym aspektem Azure Data Catalog jest sposób obsługuje crowdsourcing metadanych w systemie. W przeciwieństwie do podejścia wiki — gdzie istnieje tylko jeden opinii i ostatniego wins składnik zapisywania — model Azure Data Catalog pozwala wielu opinii na żywo jednocześnie w systemie.

Takie podejście odzwierciedla rzeczywistych danych przedsiębiorstwa gdzie różni użytkownicy mogą mieć różnych perspektyw w danym zasobów:

* Administrator bazy danych przewidują informacje dotyczące umów dotyczących poziomu usług lub okna przetwarzania dostępne operacje zbiorcze ETL
* Steward danych mogą zawierać informacje dotyczące procesów biznesowych, których dotyczy zasobu lub klasyfikacjami firmy została zastosowana do niego
* Analityk finansowego może dostarczyć informacji o sposobie korzystania z danych podczas okres zakończenia zadania raportowania

Aby zapewnić obsługę tego przykładu, każdego użytkownika — Administrator bazy danych, steward danych i analityków — można dodać opis do pojedynczej tabeli, która jest zarejestrowana w katalogu. Wszystkie opisy są obsługiwane w systemie, a w portalu wykazu danych Azure są wyświetlane wszystkie opisy.

Ten wzorzec jest stosowany do większości pozycji w modelu obiektów, typy obiektów w ładunku JSON są często tablice dla właściwości, gdzie może spodziewać się jako pojedyncza.

Na przykład w obszarze trwałego główny jest Tablica obiektów opis. Właściwości tablicy jest o nazwie "opis". Obiekt opis ma jedną właściwość — opis. Wzorzec jest, że każdy użytkownik, który opis typów pobiera obiekt opis utworzone dla wartości podane przez użytkownika.

Środowiska użytkownika można następnie wybrać sposób wyświetlania połączenie. Istnieją trzy różne wzorce do wyświetlenia.

* Najprostsza wzorzec jest "Pokaż wszystko". W tym wzorcu wszystkie obiekty są wyświetlane w widoku listy. Portal Azure Data Catalog UX używa tego wzorca opis.
* Inny wzorzec jest "Merge". W tym wzorcu wszystkie wartości z różnych użytkowników są scalane, ze zduplikowaną usunięte. Przykładem tego wzorca w portalu Azure Data Catalog UX są właściwości tagów i ekspertów.
* Trzeci wzorzec jest "ostatni składnik zapisywania usługi wins". W tym wzorcu tylko najnowsze wartość wpisaną w polu jest wyświetlana. friendlyName jest to przykład tego wzorca.

## <a name="asset-object-model"></a>Model obiektów zasobu
Wprowadzonego w sekcji kluczowe założenia **Azure Data Catalog** model obiektu zawiera elementy, które mogą być zasoby lub adnotacji. Elementy mają właściwości, które mogą być opcjonalne lub wymagane. Niektóre właściwości są stosowane do wszystkich elementów. Niektóre właściwości są stosowane do wszystkich zasobów. Niektóre właściwości dotyczą tylko typy określonych zasobów.

### <a name="system-properties"></a>Właściwości systemu
<table><tr><td><b>Nazwa właściwości</b></td><td><b>Typ danych</b></td><td><b>Komentarze</b></td></tr><tr><td>sygnatura czasowa</td><td>Data i godzina</td><td>Czas ostatniej modyfikacji elementu. To pole jest generowany przez serwer po wstawieniu elementu i każdej aktualizacji elementu. Wartość tej właściwości jest ignorowany w danych wejściowych dla operacji publikowania.</td></tr><tr><td>id</td><td>Identyfikator URI</td><td>Bezwzględny adres url elementu (tylko do odczytu). Jest unikatowy adresowanego identyfikator URI dla elementu.  Wartość tej właściwości jest ignorowany w danych wejściowych dla operacji publikowania.</td></tr><tr><td>type</td><td>Ciąg</td><td>Typ zasobu (tylko do odczytu).</td></tr><tr><td>Element etag</td><td>Ciąg</td><td>Ciąg odpowiadający wersji elementu, który może być używane do kontroli optymistycznej współbieżności podczas wykonywania operacji, które aktualizują elementów w katalogu. "*" może służyć do dopasowania do dowolnej wartości.</td></tr></table>

### <a name="common-properties"></a>Wspólne właściwości
Te właściwości mają zastosowanie do wszystkich typów zasobów głównego i wszystkie typy adnotacji.

<table>
<tr><td><b>Nazwa właściwości</b></td><td><b>Typ danych</b></td><td><b>Komentarze</b></td></tr>
<tr><td>fromSourceSystem</td><td>Wartość logiczna</td><td>Wskazuje, czy pochodzi z systemu źródłowego (na przykład baza danych Sql Server, bazy danych Oracle) lub utworzone przez użytkownika elementu danych.</td></tr>
</table>

### <a name="common-root-properties"></a>Wspólne właściwości katalogu głównego
<p>
Te właściwości stosowane do wszystkich typów zasobów katalogu głównego.

<table><tr><td><b>Nazwa właściwości</b></td><td><b>Typ danych</b></td><td><b>Komentarze</b></td></tr><tr><td>name</td><td>Ciąg</td><td>Nazwa utworzona na podstawie informacji o lokalizacji źródła danych</td></tr><tr><td>DSL</td><td>DataSourceLocation</td><td>Jednoznacznie zawiera opis źródła danych i jest jednym z identyfikatorami za dany zasób. (Patrz sekcja tożsamości dwóch).  Struktura dsl zależy od typu protokół i źródła.</td></tr><tr><td>źródło danych</td><td>Informacji o źródle</td><td>Więcej szczegółów na typie zasobów.</td></tr><tr><td>lastRegisteredBy</td><td>SecurityPrincipal</td><td>W tym artykule opisano użytkownika, który ostatnio zarejestrowany ten zasób.  Zawiera zarówno Unikatowy identyfikator dla użytkownika (upn) i nazwę wyświetlaną (nazwisko i imię).</td></tr><tr><td>identyfikatora kontenera</td><td>Ciąg</td><td>Identyfikator zasobu kontenera dla źródła danych. Ta właściwość nie jest obsługiwana dla typu kontenera.</td></tr></table>

### <a name="common-non-singleton-annotation-properties"></a>Wspólne właściwości niepojedynczej adnotacji
Te właściwości mają zastosowanie do wszystkich typów niepojedynczej adnotacji (adnotacje, które może być wiele według zasobu).

<table>
<tr><td><b>Nazwa właściwości</b></td><td><b>Typ danych</b></td><td><b>Komentarze</b></td></tr>
<tr><td>key</td><td>Ciąg</td><td>Użytkownik określony klucz, który unikatowo identyfikuje adnotacji w bieżącej kolekcji. Długość klucza nie może przekraczać 256 znaków.</td></tr>
</table>

### <a name="root-asset-types"></a>Typy zasobów głównego
Typy zasobów głównego są te typy, które reprezentują różnych typów zasobów danych, które mogą być rejestrowane w katalogu. Dla każdego typu głównego istnieje widok, w którym opisano zasobów i adnotacje uwzględnione w widoku. Należy użyć nazwy widoku w odpowiadającym segmencie adresu url {view_name} podczas publikowania zasobów przy użyciu interfejsu API REST.

<table><tr><td><b>Typ zasobu (nazwa widoku)</b></td><td><b>Dodatkowe właściwości</b></td><td><b>Typ danych</b></td><td><b>Dozwolone adnotacji</b></td><td><b>Komentarze</b></td></tr><tr><td>W tabeli ("tabele")</td><td></td><td></td><td>Opis<p>friendlyName<p>Tag<p>Schemat<p>ColumnDescription<p>ColumnTag<p> Ekspert<p>Wersja zapoznawcza<p>AccessInstruction<p>TableDataProfile<p>ColumnDataProfile<p>ColumnDataClassification<p>Dokumentacja<p></td><td>Tabela reprezentuje danych tabelarycznych.  Na przykład: tabeli SQL, Widok SQL, tabeli tabelaryczne usług analizy, wielowymiarowych usług Analysis Services wymiaru, Oracle tabeli itp.   </td></tr><tr><td>Miary ("działania")</td><td></td><td></td><td>Opis<p>friendlyName<p>Tag<p>Ekspert<p>AccessInstruction<p>Dokumentacja<p></td><td>Ten typ przedstawia miar usług Analysis Services.</td></tr><tr><td></td><td>Miary</td><td>Kolumna</td><td></td><td>Metadane opisujące miary</td></tr><tr><td></td><td>isCalculated </td><td>Wartość logiczna</td><td></td><td>Określa, czy miara jest obliczana lub nie.</td></tr><tr><td></td><td>Grupa miar</td><td>Ciąg</td><td></td><td>Fizycznych kontenerów miary</td></tr><td>Kluczowy wskaźnik wydajności "(KPI)</td><td></td><td></td><td>Opis<p>friendlyName<p>Tag<p>Ekspert<p>AccessInstruction<p>Dokumentacja</td><td></td></tr><tr><td></td><td>Grupa miar</td><td>Ciąg</td><td></td><td>Fizycznych kontenerów miary</td></tr><tr><td></td><td>goalExpression</td><td>Ciąg</td><td></td><td>Liczbowego wyrażenia MDX lub obliczeń, która zwraca docelowa wartość wskaźnika KPI.</td></tr><tr><td></td><td>valueExpression</td><td>Ciąg</td><td></td><td>Liczbowego wyrażenia MDX zwracające wartość rzeczywistą kluczowego wskaźnika wydajności.</td></tr><tr><td></td><td>statusExpression</td><td>Ciąg</td><td></td><td>Wyrażenie MDX, która reprezentuje stan kluczowego wskaźnika wydajności w określonym punkcie w czasie.</td></tr><tr><td></td><td>trendExpression</td><td>Ciąg</td><td></td><td>Wyrażenie MDX, która daje w wyniku wartość wskaźnika KPI w czasie. Trend może mieć żadnych oparte na czasie kryterium, które są przydatne w kontekście firmy.</td>
<tr><td>Raport ("raporty")</td><td></td><td></td><td>Opis<p>friendlyName<p>Tag<p>Ekspert<p>AccessInstruction<p>Dokumentacja<p></td><td>Ten typ przedstawia raportu usług SQL Server Reporting Services </td></tr><tr><td></td><td>assetCreatedDate</td><td>Ciąg</td><td></td><td></td></tr><tr><td></td><td>assetCreatedBy</td><td>Ciąg</td><td></td><td></td></tr><tr><td></td><td>assetModifiedDate</td><td>Ciąg</td><td></td><td></td></tr><tr><td></td><td>assetModifiedBy</td><td>Ciąg</td><td></td><td></td></tr><tr><td>Kontener ("kontenery")</td><td></td><td></td><td>Opis<p>friendlyName<p>Tag<p>Ekspert<p>AccessInstruction<p>Dokumentacja<p></td><td>Ten typ przedstawia kontener inne zasoby, takie jak bazy danych SQL, kontener obiektów blob Azure lub modelu usług Analysis Services.</td></tr></table>

### <a name="annotation-types"></a>Typy adnotacji
Adnotacja typy reprezentują rodzaje metadanych, które mogą być przypisane do innych typów w katalogu.

<table>
<tr><td><b>Typ adnotacji (nazwa widoku zagnieżdżone)</b></td><td><b>Dodatkowe właściwości</b></td><td><b>Typ danych</b></td><td><b>Komentarze</b></td></tr>

<tr><td>Opis elementu ("opis")</td><td></td><td></td><td>Ta właściwość zawiera opis elementu zawartości. Każdy użytkownik systemu można dodać własne opis.  Tylko ten użytkownik może edytować opis obiektu.  (Właścicieli Administratorzy i zasobów można usunąć obiektu opisu, ale nie można go edytować). System przechowuje opisy użytkowników oddzielnie.  W związku z tym jest tablicą opisów na poszczególnych zasobów (po jednej dla każdego użytkownika, który przyczynił się swoją wiedzą o zasobów, oprócz prawdopodobnie taki, który zawiera informacje pochodzące ze źródła danych).</td></tr>
<tr><td></td><td>description</td><td>Ciąg</td><td>Krótki opis elementu zawartości (2 – 3 wiersze)</td></tr>

<tr><td>Znaczników "(tagów)</td><td></td><td></td><td>Ta właściwość określa tag zasobu. Każdy użytkownik systemu można dodać wiele tagów dla zasobu.  Tylko użytkownik, który utworzył obiekty Tag je edytować.  (Właścicieli Administratorzy i zasobów można usunąć Tag obiekt, ale nie można go edytować). System przechowuje tagi użytkowników oddzielnie.  W związku z tym jest Tablica obiektów tagu na poszczególnych zasobów.</td></tr>
<tr><td></td><td>Tag</td><td>Ciąg</td><td>Tag opisujące element zawartości.</td></tr>

<tr><td>FriendlyName ("friendlyName")</td><td></td><td></td><td>Ta właściwość zawiera przyjazną nazwę dla zasobu. FriendlyName jest adnotacji pojedyncze — tylko jeden FriendlyName mogą być dodawane do elementu zawartości.  Tylko użytkownik, który utworzył obiekt FriendlyName, można go edytować. (Właścicieli Administratorzy i zasobów można usunąć obiektu FriendlyName, ale nie można go edytować). System przechowuje oddzielnie przyjaznych nazw użytkowników.</td></tr>
<tr><td></td><td>friendlyName</td><td>Ciąg</td><td>Przyjazna nazwa zasobu.</td></tr>

<tr><td>Schemat ("schema")</td><td></td><td></td><td>Schemat opisuje struktury danych.  On Wyświetla nazwy atrybut (kolumna, atrybut, pole itp.), typy również innych metadanych.  Te informacje jest pochodzące ze źródła danych.  Schemat jest adnotacji pojedyncze — można dodać tylko jeden schemat dla zasobu.</td></tr>
<tr><td></td><td>kolumny</td><td>[Kolumna]</td><td>Tablica obiektów kolumny. Opisano w nich kolumny informacje pochodzące ze źródła danych.</td></tr>

<tr><td>ColumnDescription ("columnDescriptions")</td><td></td><td></td><td>Ta właściwość zawiera opis kolumny.  Każdy użytkownik systemu można dodać opisami wielu kolumn (co najwyżej jeden na kolumny). Tylko użytkownik, który utworzył obiekty ColumnDescription je edytować.  (Właścicieli Administratorzy i zasobów można usunąć obiektu ColumnDescription, ale nie można go edytować). System przechowuje te użytkowników opisy kolumn oddzielnie.  W związku z tym jest Tablica obiektów ColumnDescription na poszczególnych zasobów (jeden na kolumnę dla każdego użytkownika, który przyczynił się swoją wiedzą o kolumn, oprócz prawdopodobnie taki, który zawiera informacje pochodzące ze źródła danych).  ColumnDescription słabo jest powiązana ze schematem, można uzyskać zsynchronizowane. ColumnDescription opisuje kolumny, która już nie istnieje w schemacie.  Jest składnik zapisywania do synchronizowania opis i schematu.  Źródło danych może również zawierać informacje opis kolumn i są dodatkowe obiekty ColumnDescription, które zostałyby utworzone podczas uruchamiania narzędzia.</td></tr>
<tr><td></td><td>Element columnName</td><td>Ciąg</td><td>Nazwa kolumny, której dotyczy ten opis.</td></tr>
<tr><td></td><td>description</td><td>Ciąg</td><td>Krótki opis (2 – 3 wiersze) kolumny.</td></tr>

<tr><td>ColumnTag ("columnTags")</td><td></td><td></td><td>Ta właściwość zawiera tag dla kolumny. Każdy użytkownik systemu można dodać wiele tagów dla podanej kolumny i dodać tagi do wielu kolumn. Tylko użytkownik, który utworzył obiekty ColumnTag je edytować. (Właścicieli Administratorzy i zasobów można usunąć obiektu ColumnTag, ale nie można go edytować). System przechowuje oddzielnie tagi kolumny tych użytkowników.  W związku z tym jest Tablica obiektów ColumnTag na poszczególnych zasobów.  ColumnTag słabo jest powiązana ze schematem, można uzyskać zsynchronizowane. ColumnTag opisuje kolumny, która już nie istnieje w schemacie.  Jest moduł zapisujący synchronizowania kolumny znacznika i schematu.</td></tr>
<tr><td></td><td>Element columnName</td><td>Ciąg</td><td>Nazwa kolumny, której dotyczy ten tag.</td></tr>
<tr><td></td><td>Tag</td><td>Ciąg</td><td>Tag zawierający opis kolumny.</td></tr>

<tr><td>Ekspert ("ekspertów")</td><td></td><td></td><td>Ta właściwość zawiera użytkownika, który jest uznawany za eksperta w zestawie danych. Ekspertów opinions(descriptions) bąbelków do góry UX podczas wyświetlania opisów. Każdy użytkownik może określić własne ekspertów. Tylko ten użytkownik może edytować obiekt ekspertów. (Właścicieli Administratorzy i zasobów można usunąć obiekty ekspertów, ale nie można go edytować).</td></tr>
<tr><td></td><td>Ekspert</td><td>SecurityPrincipal</td><td></td></tr>

<tr><td>Wersja zapoznawcza ("podglądy")</td><td></td><td></td><td>Wersja zapoznawcza zawiera migawkę górne 20 wiersze danych dla elementu zawartości. Podgląd tylko sensu dla niektórych typów zasobów (warto dla tabeli, ale nie do środka).</td></tr>
<tr><td></td><td>Wersja zapoznawcza</td><td>obiekt]</td><td>Tablica obiektów, które reprezentują kolumny.  Każdy obiekt ma właściwość mapowanie do kolumny zawierającej wartość dla tej kolumny wiersza.</td></tr>

<tr><td>AccessInstruction ("accessInstructions")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>mimeType</td><td>Ciąg</td><td>Typ mime zawartości.</td></tr>
<tr><td></td><td>Zawartość</td><td>Ciąg</td><td>Instrukcje dotyczące sposobu uzyskiwania dostępu do tego zasobu danych. Zawartość może być adres URL, adres e-mail lub zbiór instrukcji.</td></tr>

<tr><td>TableDataProfile ("tableDataProfiles")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>numberOfRows</td></td><td>int</td><td>Liczba wierszy w zestawie danych</td></tr>
<tr><td></td><td>Rozmiar</td><td>długa</td><td>Rozmiar w bajtach zestawu danych.  </td></tr>
<tr><td></td><td>schemaModifiedTime</td><td>Ciąg</td><td>Czas ostatniej modyfikacji schematu</td></tr>
<tr><td></td><td>dataModifiedTime</td><td>Ciąg</td><td>Czas ostatniej modyfikacji zestawu danych (danych zostało dodane, zmodyfikowane lub usuń)</td></tr>

<tr><td>ColumnsDataProfile ("columnsDataProfiles")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>kolumny</td></td><td>[ColumnDataProfile]</td><td>Tablica kolumn danych profilów.</td></tr>

<tr><td>ColumnDataClassification ("columnDataClassifications")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Element columnName</td><td>Ciąg</td><td>Nazwa ta klasyfikacja odnosi się do kolumny.</td></tr>
<tr><td></td><td>Klasyfikacja</td><td>Ciąg</td><td>Klasyfikacja danych w tej kolumnie.</td></tr>

<tr><td>Dokumentacja "(dokumentacja)</td><td></td><td></td><td>Dany zasobów może mieć tylko jeden dokumentacji skojarzonych z nim.</td></tr>
<tr><td></td><td>mimeType</td><td>Ciąg</td><td>Typ mime zawartości.</td></tr>
<tr><td></td><td>Zawartość</td><td>Ciąg</td><td>Zawartość dokumentacji.</td></tr>

</table>

### <a name="common-types"></a>Standardowe typy
Popularne typy może służyć jako typy właściwości, ale nie są elementami.

<table>
<tr><td><b>Wspólny typ</b></td><td><b>Właściwości</b></td><td><b>Typ danych</b></td><td><b>Komentarze</b></td></tr>
<tr><td>Informacji o źródle</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Źródłowa</td><td>Ciąg</td><td>Opisuje typ źródła danych.  Na przykład: SQL Server, baza danych Oracle, itp.  </td></tr>
<tr><td></td><td>Typ obiektu</td><td>Ciąg</td><td>Opisuje typ obiektu źródła danych. Na przykład: tabela, wyświetlanie dla programu SQL Server.</td></tr>

<tr><td>DataSourceLocation</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Protokół</td><td>Ciąg</td><td>Wymagany. Opisuje protokół używany do komunikowania się ze źródłem danych. Na przykład: "tds" dla programu SQl Server "oracle" Oracle itp. Zapoznaj się [Specyfikacja odwołanie - DSL struktura źródła danych](data-catalog-dsr.md) listę aktualnie obsługiwanych protokołów.</td></tr>
<tr><td></td><td>Adres</td><td>Słownik<string, object></td><td>Wymagany. Adres jest zestawem danych specyficznych dla protokołu, który służy do identyfikowania źródła danych, do którego nastąpiło odwołanie. Dane do konkretnego protokołu zakresu, czyli jest bez znaczenia bez wiedzy o protokół.</td></tr>
<tr><td></td><td>Uwierzytelnianie</td><td>Ciąg</td><td>Opcjonalny. Schemat uwierzytelniania używany do komunikowania się ze źródłem danych. Na przykład: windows oauth, itp.</td></tr>
<tr><td></td><td>connectionProperties</td><td>Słownik<string, object></td><td>Opcjonalny. Dodatkowe informacje na temat nawiązywania połączenia ze źródłem danych.</td></tr>

<tr><td>SecurityPrincipal</td><td></td><td></td><td>Wewnętrznej bazy danych nie wykonuje żadnych weryfikacji właściwości podana przed AAD podczas publikowania.</td></tr>
<tr><td></td><td>nazwy UPN</td><td>Ciąg</td><td>Unikatowy adres e-mail użytkownika. Należy podać identyfikator obiektu nie został dostarczony lub w kontekście dla właściwości "lastRegisteredBy", w przeciwnym razie opcjonalny.</td></tr>
<tr><td></td><td>Identyfikator obiektu</td><td>Identyfikator GUID</td><td>Tożsamość usługi AAD grupy użytkowników lub zabezpieczeń. Opcjonalny. Musi być określona, jeśli nazwy upn nie zostanie podany, w przeciwnym razie opcjonalne.</td></tr>
<tr><td></td><td>Imię</td><td>Ciąg</td><td>Imię użytkownika (na potrzeby wyświetlania). Opcjonalny. Jest to prawidłowy tylko w kontekście dla właściwości "lastRegisteredBy". Nie można określić podczas dostarczania podmiotu zabezpieczeń "role", "uprawnienia" i "ekspertów".</td></tr>
<tr><td></td><td>Nazwisko</td><td>Ciąg</td><td>Nazwisko użytkownika (na potrzeby wyświetlania). Opcjonalny. Jest to prawidłowy tylko w kontekście dla właściwości "lastRegisteredBy". Nie można określić podczas dostarczania podmiotu zabezpieczeń "role", "uprawnienia" i "ekspertów".</td></tr>

<tr><td>Kolumna</td><td></td><td></td><td></td></tr>
<tr><td></td><td>name</td><td>Ciąg</td><td>Nazwa kolumny lub atrybutu.</td></tr>
<tr><td></td><td>type</td><td>Ciąg</td><td>Typ danych kolumny lub atrybutu. Dopuszczalne typy są zależne od danych źródłowa środka trwałego.  Obsługiwane jest tylko podzestaw typów.</td></tr>
<tr><td></td><td>Element maxLength</td><td>int</td><td>Maksymalna dozwolona długość kolumny lub atrybutu. Pochodzące ze źródła danych. Dotyczy to tylko niektóre typy źródeł.</td></tr>
<tr><td></td><td>dokładność</td><td>Bajtów</td><td>Dokładność kolumny lub atrybutu. Pochodzące ze źródła danych. Dotyczy to tylko niektóre typy źródeł.</td></tr>
<tr><td></td><td>isNullable</td><td>Wartość logiczna</td><td>Określa, czy kolumna może mieć wartości null lub nie. Pochodzące ze źródła danych. Dotyczy to tylko niektóre typy źródeł.</td></tr>
<tr><td></td><td>wyrażenie</td><td>Ciąg</td><td>Jeśli wartość jest kolumną obliczaną, to pole zawiera wyrażenie, które określa wartość. Pochodzące ze źródła danych. Dotyczy to tylko niektóre typy źródeł.</td></tr>

<tr><td>ColumnDataProfile</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Element columnName </td><td>Ciąg</td><td>Nazwa kolumny</td></tr>
<tr><td></td><td>type </td><td>Ciąg</td><td>Typ kolumny</td></tr>
<tr><td></td><td>min. </td><td>Ciąg</td><td>Minimalna wartość w zestawie danych</td></tr>
<tr><td></td><td>Maksymalna </td><td>Ciąg</td><td>Maksymalna wartość w zestawie danych</td></tr>
<tr><td></td><td>Śr. </td><td>O podwójnej precyzji</td><td>Średnia wartość w zestawie danych</td></tr>
<tr><td></td><td>StDev </td><td>O podwójnej precyzji</td><td>Odchylenie standardowe dla zestawu danych</td></tr>
<tr><td></td><td>nullCount </td><td>int</td><td>Liczba wartości null w zestawie danych</td></tr>
<tr><td></td><td>distinctCount  </td><td>int</td><td>Liczba unikatowych wartości w zestawie danych</td></tr>


</table>

## <a name="asset-identity"></a>Tożsamości zasobów
Azure Data Catalog używa właściwości "Protokół" i tożsamości ze zbioru właściwości "address" właściwości "dsl" DataSourceLocation do generowania tożsamości zasobu, która jest wykorzystywana do adresowania zasobów w katalogu.
Na przykład protokołu "tds" ma właściwości identity "serwer", "baza danych" i "schema", "object". Kombinacja protokołu i właściwości tożsamości są używane do generowania tożsamość zasobów tabeli serwera SQL.
Azure Data Catalog zawiera kilka wbudowanych danych źródła protokołów, które są wymienione w [Specyfikacja odwołanie - DSL struktura źródła danych](data-catalog-dsr.md).
Zestaw obsługiwanych protokołów, które mogą zostać rozszerzone programowo (zajrzyj do dokumentacji interfejsu API REST wykazu danych). Administratorzy katalogu mogą rejestrować protokołów źródła danych niestandardowych. W poniższej tabeli opisano właściwości wymaganych do zarejestrowania protokołu niestandardowego.

### <a name="custom-data-source-protocol-specification"></a>Specyfikacja protokołu źródła danych niestandardowych
<table>
<tr><td><b>Typ</b></td><td><b>Właściwości</b></td><td><b>Typ danych</b></td><td><b>Komentarze</b></td></tr>

<tr><td>DataSourceProtocol</td><td></td><td></td><td></td></tr>
<tr><td></td><td>przestrzeń nazw</td><td>Ciąg</td><td>Przestrzeń nazw protokołu. Namespace musi mieć od 1 do 255 znaków długości, zawierać jedną lub więcej niepustym części oddzielone kropką (.). Każda część musi mieć długość od 1 do 255 znaków, zaczynać się literą i zawierać tylko litery i cyfry.</td></tr>
<tr><td></td><td>name</td><td>Ciąg</td><td>Nazwa protokołu. Nazwa musi mieć długość od 1 do 255 znaków, zaczynać się literą i zawierać tylko litery, cyfry i znak kreski (-).</td></tr>
<tr><td></td><td>identityProperties</td><td>[DataSourceProtocolIdentityProperty]</td><td>Lista właściwości tożsamości, musi zawierać co najmniej jeden, ale nie więcej niż 20 właściwości. Na przykład: "server", "baza danych", "schema", "obiektu" są właściwościami tożsamości protokołu "tds".</td></tr>
<tr><td></td><td>identitySets</td><td>[DataSourceProtocolIdentitySet]</td><td>Lista zestawów tożsamości. Definiuje ustawia właściwości tożsamości, które reprezentują tożsamość prawidłowy zasobów. Musi zawierać co najmniej jeden, ale nie więcej niż 20 zestawów. Na przykład: {"server", "bazy danych", "schema" i "object"} jest tożsamość zestawu dla protokołu "tds", który określa tożsamość zasobów tabeli programu Sql Server.</td></tr>

<tr><td>DataSourceProtocolIdentityProperty</td><td></td><td></td><td></td></tr>
<tr><td></td><td>name</td><td>Ciąg</td><td>Nazwa właściwości. Nazwa musi mieć od 1 do 100 znaków, rozpoczyna się od litery i może zawierać tylko litery i cyfry.</td></tr>
<tr><td></td><td>type</td><td>Ciąg</td><td>Typ właściwości. Obsługiwane wartości: "bool", wartość logiczna ","bajtów","guid","int","integer","long","string","url"</td></tr>
<tr><td></td><td>ignoreCase</td><td>wartość logiczna</td><td>Wskazuje, czy należy ją ignorować case, używając wartość właściwości. Można określić tylko dla właściwości typu "string". Wartość domyślna to false.</td></tr>
<tr><td></td><td>urlPathSegmentsIgnoreCase</td><td>[bool]</td><td>Wskazuje, czy należy ją ignorować przypadku dla każdego segmentu ścieżki adresu url. Można określić tylko dla właściwości typu "url". Wartość domyślna to [false].</td></tr>

<tr><td>DataSourceProtocolIdentitySet</td><td></td><td></td><td></td></tr>
<tr><td></td><td>name</td><td>Ciąg</td><td>Nazwa tożsamości zestawu.</td></tr>
<tr><td></td><td>properties</td><td>ciąg]</td><td>Lista właściwości tożsamości zawarte w tej tożsamości ustawiona. Nie może zawierać duplikatów. Każda właściwość odwołuje się zestaw tożsamość musi być zdefiniowany na liście "identityProperties" protokołu.</td></tr>

</table>

## <a name="roles-and-authorization"></a>Role i autoryzacji
Microsoft Azure Data Catalog oferuje możliwości autoryzacji dla operacji CRUD na zasoby i adnotacje.

## <a name="key-concepts"></a>Kluczowe pojęcia
Azure Data Catalog używa dwóch mechanizmów autoryzacji:

* Autoryzacji opartej na rolach
* Autoryzacji na podstawie uprawnień

### <a name="roles"></a>Role
Dostępne są trzy role: **administratora**, **właściciela**, i **współautora**.  Każda rola ma jego zakres i praw, które przedstawiono w poniższej tabeli.

<table><tr><td><b>Rola</b></td><td><b>Zakres</b></td><td><b>Prawa</b></td></tr><tr><td>Administrator</td><td>Katalog (wszystkie zasoby/adnotacje w katalogu)</td><td>ViewRoles Usuń odczytu

ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Właściciel</td><td>Poszczególnych zasobów (element główny)</td><td>ViewRoles Usuń odczytu

ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Współautor</td><td>Każdy poszczególnych zasobów i adnotacji</td><td>Odczytu aktualizacji Delete ViewRoles Uwaga: wszystkie prawa są odwołane po odebraniu odczytu w elemencie z Współautor</td></tr></table>

> [!NOTE]
> **Odczyt**, **aktualizacji**, **usunąć**, **ViewRoles** prawa są stosowane do dowolnego elementu (zasobów lub adnotacji) podczas **TakeOwnership**, **ChangeOwnership**, **ChangeVisibility**, **ViewPermissions** dotyczą tylko zasobów katalogu głównego.
> 
> **Usuń** prawo dotyczy element i wszystkie elementy podrzędne lub pojedynczy element podrzędne. Na przykład usunięcie elementu zawartości spowoduje również usunięcie adnotacji dla tego zasobu.
> 
> 

### <a name="permissions"></a>Uprawnienia
Uprawnienie jest jako listy wpisów kontroli dostępu. Każdego wpisu kontroli dostępu przypisuje zestaw praw do podmiotu zabezpieczeń. Uprawnienia można określić tylko dla elementu zawartości (to znaczy elementu głównego) i stosuje się do elementu zawartości i wszystkie elementy podrzędne.

Podczas **Azure Data Catalog** tylko podgląd **odczytu** prawo jest obsługiwana na liście uprawnień do scenariusza ograniczyć widoczność zasobów.

Domyślnie każdy użytkownik uwierzytelniony ma **odczytu** prawym przyciskiem myszy każdego elementu w katalogu, chyba że widoczność jest ograniczona do zestawu podmiotów zabezpieczeń, w obszarze uprawnienia.

## <a name="rest-api"></a>Interfejs API REST
**Umieść** i **POST** żądań widoku elementu można użyć do kontrolowania role i uprawnienia: oprócz ładunku elementu, można określić dwie właściwości systemu **ról** i **uprawnienia**.

> [!NOTE]
> **uprawnienia** dotyczy to tylko element główny.
> 
> **Właściciel** roli dotyczy to tylko element główny.
> 
> Domyślnie, gdy element jest tworzony w katalogu jego **współautora** ma ustawioną wartość aktualnie uwierzytelnionego użytkownika. Jeśli element ma być aktualizowalnych osoby, **współautora** powinien być ustawiony na &lt;wszyscy&gt; specjalne podmiot zabezpieczeń w **ról** właściwości, gdy element zostanie opublikowane (zobacz poniższy przykład). **Współautor** nie można zmienić i nie zmienia czasie życia elementu (nawet **administratora** lub **właściciela** nie ma uprawnień do zmiany **współautora**). Jedyna wartość obsługiwana w przypadku jawnego ustawienia **współautora** jest &lt;wszyscy&gt;: **współautora** może być tylko użytkownik, który utworzył element lub &lt;wszyscy&gt;.
> 
> 

### <a name="examples"></a>Przykłady
**Ustaw współautora &lt;wszyscy&gt; podczas publikowania elementu.**
Podmiot zabezpieczeń specjalne &lt;wszyscy&gt; ma objectId "00000000-0000-0000-0000-000000000201".
  **POST** https://api.azuredatacatalog.com/catalogs/default/views/tables/?api-version=2016-03-30

> [!NOTE]
> Niektórych implementacjach klienta HTTP może automatycznie ponownie wykonaj żądania w odpowiedzi 302 z serwera, ale zazwyczaj paska nagłówki autoryzacji w żądaniu. Nagłówek uwierzytelnienia jest wymagane na wysyłanie żądań do usługi Azure Data Catalog, należy się upewnić się, że nagłówek uwierzytelnienia nadal jest udostępniane po ponownym wystawieniu żądanie przekierowania lokalizacji określonej przez wykaz danych Azure. Następujący przykładowy kod przedstawia za pomocą obiektu .NET HttpWebRequest.
> 
> 

**Treść**

    {
        "roles": [
            {
                "role": "Contributor",
                "members": [
                    {
                        "objectId": "00000000-0000-0000-0000-000000000201"
                    }
                ]
            }
        ]
    }

  **Przypisz właścicieli i ograniczenia ich widoczności dla istniejącego elementu głównego**: **PUT** https://api.azuredatacatalog.com/catalogs/default/views/tables/042297b0...1be45ecd462a?api-version=2016-03-30

    {
        "roles": [
            {
                "role": "Owner",
                "members": [
                    {
                        "objectId": "c4159539-846a-45af-bdfb-58efd3772b43",
                        "upn": "user1@contoso.com"
                    },
                    {
                        "objectId": "fdabd95b-7c56-47d6-a6ba-a7c5f264533f",
                        "upn": "user2@contoso.com"
                    }
                ]
            }
        ],
        "permissions": [
            {
                "principal": {
                    "objectId": "27b9a0eb-bb71-4297-9f1f-c462dab7192a",
                    "upn": "user3@contoso.com"
                },
                "rights": [
                    {
                        "right": "Read"
                    }
                ]
            },
            {
                "principal": {
                    "objectId": "4c8bc8ce-225c-4fcf-b09a-047030baab31",
                    "upn": "user4@contoso.com"
                },
                "rights": [
                    {
                        "right": "Read"
                    }
                ]
            }
        ]
    }

> [!NOTE]
> W PUT nie jest to wymagane do określenia elementu ładunku w treści: PUT może posłużyć do zaktualizowania właśnie ról lub uprawnień.
> 
> 

<!--Image references-->
[1]: ./media/data-catalog-developer-concepts/concept2.png
