---
title: 'Wyniki analizy strumienia: opcje magazynu, analizy | Dokumentacja firmy Microsoft'
description: "Informacje o przeznaczonych dla opcji dane wyjściowe analiza strumienia danych w tym usługi Power BI dla wyników analizy."
keywords: "przekształcenia danych, wyniki analizy, opcje magazynu danych"
services: stream-analytics,documentdb,sql-database,event-hubs,service-bus,storage
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: ba6697ac-e90f-4be3-bafd-5cfcf4bd8f1f
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 33d0b9aa37cc92dda27f1cf21f1d393b42b8c09b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="stream-analytics-outputs-options-for-storage-analysis"></a>Wyniki analizy strumienia: opcje magazynu, analizy
Podczas tworzenia zadania usługi analiza strumienia, należy rozważyć, jak będą działały danych. Jak będzie można wyświetlić wyniki zadania usługi analiza strumienia i będzie przechowywania go?

Aby włączyć różnych wzorców aplikacji, usługi Azure Stream Analytics ma inne opcje do przechowywania danych wyjściowych i wyświetlania wyników analizy. To ułatwia wyświetlić dane wyjściowe zadania i zapewnia elastyczność w użycie i przechowywania danych wyjściowych zadania dla magazynów danych i innych celów. Żadnych danych wyjściowych konfigurowane w zadaniu musi istnieć przed rozpoczęcia zadania i uruchomić zdarzeń przepływu. Na przykład jeśli używasz magazynu obiektów Blob jako dane wyjściowe zadania nie utworzy konta magazynu automatycznie. Musi być utworzony przez użytkownika, przed uruchomieniem zadania ASA.

## <a name="azure-data-lake-store"></a>Azure Data Lake Store
Strumienia Analytics obsługuje [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). Ten magazyn umożliwia przechowywanie danych dowolnego rozmiar, typ i wprowadzanie szybkość analiz operacyjnych i poznawczych. Ponadto analiza strumienia musi mieć możliwość dostępu do usługi Data Lake Store. Więcej informacji dotyczących autoryzacji i jak zarejestrować się do usługi Data Lake Store (w razie potrzeby), omówiono w [usługi Data Lake output artykułu](stream-analytics-data-lake-output.md).

### <a name="authorize-an-azure-data-lake-store"></a>Autoryzowanie usługi Azure Data Lake Store
Po wybraniu jako dane wyjściowe w portalu Azure Data Lake Storage pojawi się monit autoryzować połączenia do istniejącej usługi Data Lake Store.  

![Autoryzowanie usługi Data Lake Store](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

Wypełnij właściwości dla danych wyjściowych usługi Data Lake Store jak pokazano poniżej:

![Autoryzowanie usługi Data Lake Store](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

W poniższej tabeli wymieniono nazwy właściwości i ich opisy niezbędny do utworzenia wyjście usługi Data Lake Store.

<table>
<tbody>
<tr>
<td><B>NAZWA WŁAŚCIWOŚCI</B></td>
<td><B>OPIS ELEMENTU</B></td>
</tr>
<tr>
<td>Alias wyjściowy</td>
<td>Jest to przyjazna nazwa używana w zapytaniach do kierowania wyników zapytania do tej usługi Data Lake Store.</td>
</tr>
<tr>
<td>Nazwa konta</td>
<td>Nazwa konta usługi Data Lake Storage, gdzie wysyłania danych wyjściowych. Zostaną wyświetlone z rozwijanej listy, do których użytkownik zalogował się do portalu ma dostęp do kont usługi Data Lake Store.</td>
</tr>
<tr>
<td>Wzorzec ścieżki prefiksu</td>
<td>Nazywanie pliku będzie postępuj zgodnie z następującą konwencją: <BR>{Ścieżka prefiks Pattern}/schemaHashcode_Guid_Number.extension <BR> <BR>Przykład danych wyjściowych plików:<BR>Myoutput/20170901/00/45434_gguid_1.csv <BR>Myoutput/20170901/01/45434_gguid_1.csv <BR> <BR>Ponadto w tym miejscu są sytuacje, w którym zostanie utworzony nowy plik:<BR>1.Zmiany w schemacie danych wyjściowych <BR>2.Zewnętrznym lub wewnętrznym ponownego uruchomienia zadania<BR><BR>Ponadto jeśli wzorzec ścieżki plików nie zawiera końcowe "/", ostatni wzorca w ścieżce pliku będzie traktowany jako prefiksu nazwy pliku.<BR><BR>Przykład:<BR>Dla wzorzec ścieżki: folder1/dzienniki/HH wygenerowanego pliku może wyglądać tak: folder1/logs/02_134343_gguid_1.csv</td>
</tr>
<tr>
<td>Data w formacie [<I>opcjonalne</I>]</td>
<td>Jeśli token daty jest używany w ścieżce prefiks, można wybrać format daty, w którym pliki są organizowane. Przykład: RRRR/MM/dd.</td>
</tr>
<tr>
<td>Format czasu [<I>opcjonalne</I>]</td>
<td>Jeśli czas nie jest używany w ścieżce prefiks, określ format czasu, w którym pliki są organizowane. Obecnie jedyna obsługiwana wartość to HH.</td>
</tr>
<tr>
<td>Format serializacji zdarzeń</td>
<td>Format serializacji w danych wyjściowych. JSON, CSV i Avro są obsługiwane.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Jeśli formatu CSV lub JSON, należy określić kodowania. UTF-8 to jedyny obsługiwany obecnie format kodowania.</td>
</tr>
<tr>
<td>Ogranicznik</td>
<td>Dotyczy tylko serializacji woluminów CSV. Analiza strumienia obsługuje różne ograniczniki dla serializacji danych CSV. Obsługiwane wartości to przecinek, średnik, miejsca, karta i pionowy pasek.</td>
</tr>
<tr>
<td>Format</td>
<td>Dotyczy tylko serializacji JSON. Rozdzielone Określa, czy dane wyjściowe będą formatowane przez poszczególne obiekty JSON rozdzielone znakiem nowego wiersza. Tablica Określa, czy dane wyjściowe będą formatowane jako tablica obiektów JSON. Ta tablica zostanie zamknięty, tylko wtedy, gdy zadanie zostanie zatrzymane lub Stream Analytics została przeniesiona następne okno czasu. Ogólnie rzecz biorąc, zaleca się używania wiersza oddzielone JSON, ponieważ nie wymaga żadnej specjalnej obsługi, gdy wciąż trwa zapisywanie pliku wyjściowego do.</td>
</tr>
</tbody>
</table>

### <a name="renew-data-lake-store-authorization"></a>Odnów autoryzacji usługi Data Lake Store
Konieczne będzie ponownego uwierzytelnienia konta usługi Data Lake Store, jeśli jego hasło uległ zmianie od czasu utworzenia lub ostatniej uwierzytelniony zadania.

![Autoryzowanie usługi Data Lake Store](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  

## <a name="sql-database"></a>SQL Database
[Baza danych SQL Azure](https://azure.microsoft.com/services/sql-database/) może służyć jako dane wyjściowe, w charakterze relacyjnym lub aplikacje zależne od hostowanych w relacyjnej bazie danych. Zadania usługi analiza strumienia będą zapisywane do istniejącej tabeli w bazie danych SQL Azure.  Należy pamiętać, że schemat tabeli musi dokładnie odpowiadać pola i ich typy są dane wyjściowe z zadania. [Magazyn danych SQL Azure](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) można również określić jako dane wyjściowe za pośrednictwem bazy danych SQL opcji output również (jest to funkcja w wersji zapoznawczej). W poniższej tabeli wymieniono nazwy właściwości i ich opis tworzenie wyjścia bazy danych SQL.

| Nazwa właściwości | Opis |
| --- | --- |
| Alias wyjściowy |Jest to przyjazna nazwa używana w zapytaniach do kierowania wyników zapytania do tej bazy danych. |
| Database (Baza danych) |Nazwa bazy danych, w którym wysyłania danych wyjściowych |
| Nazwa serwera |Nazwa serwera bazy danych SQL |
| Nazwa użytkownika |Nazwa użytkownika, który ma dostęp do zapisu w bazie danych |
| Hasło |Hasło do łączenia z bazą danych |
| Tabela |Nazwa tabeli, w którym będą zapisywane dane wyjściowe. Nazwa tabeli jest uwzględniana wielkość liter i schemat tabeli powinien być w pełni zgodny liczba pól i ich typy generowane przez dane wyjściowe zadania. |

> [!NOTE]
> Obecnie oferty bazy danych SQL Azure jest obsługiwana dla danych wyjściowych zadania w Stream Analytics. Jednak maszyny wirtualnej platformy Azure, programem SQL Server z bazą danych dołączona nie jest obsługiwane. To może ulec zmianie w przyszłych wersjach.
> 
> 

## <a name="blob-storage"></a>Blob Storage
Magazyn obiektów blob oferuje ekonomiczne i skalowalne rozwiązanie do przechowywania dużych ilości danych bez struktury w chmurze.  Aby obejrzeć wprowadzenie magazynu obiektów Blob platformy Azure i jego użycia, zobacz dokumentację w [sposób użycia obiekty BLOB](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

W poniższej tabeli wymieniono nazwy właściwości i ich opis tworzenia wyjściowego obiektu blob.

<table>
<tbody>
<tr>
<td>NAZWA WŁAŚCIWOŚCI</td>
<td>OPIS</td>
</tr>
<tr>
<td>Alias wyjściowy</td>
<td>Jest to przyjazna nazwa używana w zapytaniach do kierowania wyników zapytania do tego magazynu obiektów blob.</td>
</tr>
<tr>
<td>Konto magazynu</td>
<td>Nazwa konta magazynu, w którym wysyłania danych wyjściowych.</td>
</tr>
<tr>
<td>Klucz konta magazynu</td>
<td>Klucz tajny, skojarzone z kontem magazynu.</td>
</tr>
<tr>
<td>Kontener magazynu</td>
<td>Kontenery umożliwiają logiczne grupowanie dla obiektów blob przechowywanych w usłudze Microsoft Azure Blob. Przekazywanie obiektu blob do usługi Blob, należy określić kontener dla tego obiektu blob.</td>
</tr>
<tr>
<td>Wzorzec prefiksu ścieżki [opcjonalnie]</td>
<td>Wzorzec ścieżki pliku używany do zapisywania obiektów blob w określonym kontenerze. <BR> We wzorcu ścieżkę można umożliwia określenie częstotliwości, które obiekty BLOB są zapisywane w co najmniej jedno wystąpienie następujących zmiennych 2: <BR> {date} {time} <BR> Przykład 1: Klaster1/dzienniki / {date} / {time} <BR> Przykład 2: Klaster1/dzienniki / {date} <BR> <BR> Nazywanie pliku będzie postępuj zgodnie z następującą konwencją: <BR> {Ścieżka prefiks Pattern}/schemaHashcode_Guid_Number.extension <BR> <BR> Przykład danych wyjściowych plików: <BR> Myoutput/20170901/00/45434_gguid_1.csv <BR> Myoutput/20170901/01/45434_gguid_1.csv <BR> <BR> Ponadto w tym miejscu są sytuacje, w którym zostanie utworzony nowy plik: <BR> 1.Bieżący plik przekracza dopuszczalną wartość maksymalną liczbę bloków (obecnie 50 000) <BR> 2.Zmiany w schemacie danych wyjściowych <BR> 3.Zewnętrznym lub wewnętrznym ponownego uruchomienia zadania  </td>
</tr>
<tr>
<td>Format daty [opcjonalnie]</td>
<td>Jeśli token daty jest używany w ścieżce prefiks, można wybrać format daty, w którym pliki są organizowane. Przykład: RRRR/MM/dd.</td>
</tr>
<tr>
<td>Format czasu [opcjonalnie]</td>
<td>Jeśli czas nie jest używany w ścieżce prefiks, określ format czasu, w którym pliki są organizowane. Obecnie jedyna obsługiwana wartość to HH.</td>
</tr>
<tr>
<td>Format serializacji zdarzeń</td>
<td>Format serializacji w danych wyjściowych.  JSON, CSV i Avro są obsługiwane.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Jeśli formatu CSV lub JSON, należy określić kodowania. UTF-8 to jedyny obsługiwany obecnie format kodowania.</td>
</tr>
<tr>
<td>Ogranicznik</td>
<td>Dotyczy tylko serializacji woluminów CSV. Analiza strumienia obsługuje różne ograniczniki dla serializacji danych CSV. Obsługiwane wartości to przecinek, średnik, miejsca, karta i pionowy pasek.</td>
</tr>
<tr>
<td>Format</td>
<td>Dotyczy tylko serializacji JSON. Rozdzielone Określa, czy dane wyjściowe będą formatowane przez poszczególne obiekty JSON rozdzielone znakiem nowego wiersza. Tablica Określa, czy dane wyjściowe będą formatowane jako tablica obiektów JSON. Ta tablica zostanie zamknięty, tylko wtedy, gdy zadanie zostanie zatrzymane lub Stream Analytics została przeniesiona następne okno czasu. Ogólnie rzecz biorąc, zaleca się używania wiersza oddzielone JSON, ponieważ nie wymaga żadnej specjalnej obsługi, gdy wciąż trwa zapisywanie pliku wyjściowego do.</td>
</tr>
</tbody>
</table>

## <a name="event-hub"></a>Centrum zdarzeń
[Centra zdarzeń](https://azure.microsoft.com/services/event-hubs/) jest wysoce skalowalna publikowania / subskrypcji systemem zbierania zdarzeń. Może on zbierać miliony zdarzeń na sekundę.  Użycie jednego Centrum zdarzeń jako danych wyjściowych jest w przypadku dane wyjściowe zadania Stream Analytics będzie dane wejściowe zadania przesyłania strumieniowego innego.

Istnieje kilka parametrów, które są potrzebne do skonfigurowania strumieni danych w Centrum zdarzeń jako dane wyjściowe.

| Nazwa właściwości | Opis |
| --- | --- |
| Alias wyjściowy |Jest to przyjazna nazwa używana w zapytaniach do kierowania wyników zapytania do tego Centrum zdarzeń. |
| Namespace magistrali usług |Przestrzeń nazw magistrali usług to kontener dla zestawu jednostek do obsługi komunikatów. Podczas tworzenia nowego Centrum zdarzeń, utworzono przestrzeni nazw usługi Service Bus |
| Centrum zdarzeń |Nazwa danych wyjściowych Centrum zdarzeń |
| Nazwa zasad Centrum zdarzeń |Zasady dostępu współdzielonego, można utworzyć na karcie Konfigurowanie Centrum zdarzeń. Wszystkie zasady dostępu współdzielonego zostanie mają nazwy, możesz ustawić uprawnienia i klucze dostępu |
| Klucz zasad Centrum zdarzeń |Dostęp współdzielony klucz używany do uwierzytelniania dostępu do przestrzeni nazw usługi Service Bus |
| Kolumna klucza partycji [opcjonalnie] |Ta kolumna zawiera klucz partycji dla danych wyjściowych Centrum zdarzeń. |
| Format serializacji zdarzeń |Format serializacji w danych wyjściowych.  JSON, CSV i Avro są obsługiwane. |
| Encoding |Dla woluminu CSV i JSON UTF-8 to jedyny obsługiwany format kodowania w tej chwili |
| Ogranicznik |Dotyczy tylko serializacji woluminów CSV. Usługa Stream Analytics obsługuje różne ograniczniki dla serializacji danych w formacie CSV. Obsługiwane wartości to przecinek, średnik, miejsca, karta i pionowy pasek. |
| Format |Dotyczy tylko serializacji JSON. Rozdzielone Określa, czy dane wyjściowe będą formatowane przez poszczególne obiekty JSON rozdzielone znakiem nowego wiersza. Tablica Określa, czy dane wyjściowe będą formatowane jako tablica obiektów JSON. Ta tablica zostanie zamknięty, tylko wtedy, gdy zadanie zostanie zatrzymane lub Stream Analytics została przeniesiona następne okno czasu. Ogólnie rzecz biorąc, zaleca się używania wiersza oddzielone JSON, ponieważ nie wymaga żadnej specjalnej obsługi, gdy wciąż trwa zapisywanie pliku wyjściowego do. |

## <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com/) może służyć jako dane wyjściowe zadania usługi analiza strumienia do zapewnienia środowisko sformatowanego wizualizacja wyników analizy. Ta możliwość może służyć do operacyjnej pulpity nawigacyjne, Generowanie raportu i metryki na zgłoszenie.

### <a name="authorize-a-power-bi-account"></a>Autoryzuj konto usługi Power BI
1. Po wybraniu usługi Power BI jako dane wyjściowe w portalu Azure pojawi się monit autoryzować istniejącego użytkownika Power BI lub Utwórz nowe konto usługi Power BI.  
   
   ![Autoryzowanie Power BI użytkownika](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  
2. Utwórz nowe konto, jeśli nie zostały jeszcze mieć jeden, kliknij przycisk Autoryzuj teraz.  Ekran, tak jak przedstawiono poniżej.  
   
   ![Usługa Power BI konto platformy Azure](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  
3. W tym kroku podaj konto służbowe autoryzacji wyjście usługi Power BI. Jeśli użytkownik nie jest już zarejestrowany na usługę Power BI, wybierz logowania teraz. Konto służbowe, którego używasz usługi Power BI może różnić się od konta subskrypcji platformy Azure, które są obecnie zalogowani przy użyciu.

### <a name="configure-the-power-bi-output-properties"></a>Skonfiguruj właściwości wyjście usługi Power BI
Po utworzeniu konta usługi Power BI uwierzytelniony, można skonfigurować właściwości dla danych wyjściowych usługi Power BI. W poniższej tabeli jest listę nazw właściwości i ich opis, aby skonfigurować dane wyjściowe usługi Power BI.

| Nazwa właściwości | Opis |
| --- | --- |
| Alias wyjściowy |Jest to przyjazna nazwa używana w zapytaniach do kierowania wyników zapytania do tego raportu usługi Power BI. |
| Obszaru roboczego grupy |Aby włączyć udostępniania danych innym użytkownikom usługi Power BI można wybrać grupy wewnątrz koncie usługi Power BI lub wybierz opcję "Mój obszar roboczy", jeśli nie chcesz zapisać do grupy.  Aktualizowanie istniejącej grupy wymaga ponowne uwierzytelnianie usługi Power BI. |
| Nazwa zestawu danych |Podaj nazwę zestawu danych żądanej dla danych wyjściowych usługi Power BI do użycia |
| Nazwa tabeli |Podaj nazwę tabeli w obszarze zestawu danych wyjściowych usługi Power BI. Obecnie usługa Power BI dane wyjściowe zadania usługi analiza strumienia może mieć tylko jedną tabelę w zestawie danych |

Przewodnik konfigurowania usługi Power BI w danych wyjściowych i pulpitu nawigacyjnego, zobacz [Azure Stream Analytics & usługi Power BI](stream-analytics-power-bi-dashboard.md) artykułu.

> [!NOTE]
> Nie należy jawnie tworzyć zestawu danych i tabeli na pulpicie nawigacyjnym usługi Power BI. Zestaw danych i tabeli zostaną wypełnione automatycznie po uruchomieniu zadania i rozpoczęcia zadania pompującego dane wyjściowe do usługi Power BI. Należy pamiętać, że jeśli zapytanie zadania nie generuje żadnych wyników, zestaw danych i tabeli nie zostaną utworzone. Wziąć pod uwagę, że jeśli usługi Power BI ma już zestaw danych i tabeli o takiej samej nazwie co podane w tym zadaniu Stream Analytics, istniejące dane zostaną zastąpione.
> 
> 

### <a name="schema-creation"></a>Tworzenie schematu
Usługa Azure Stream Analytics tworzy zestaw danych usługi Power BI i tabeli w imieniu użytkownika, jeśli już nie istnieje. We wszystkich innych przypadkach tabela jest aktualizowana nowymi wartościami. Obecnie istnieje ograniczenie, które tylko jednej tabeli może istnieć w elemencie dataset.

### <a name="data-type-conversion-from-asa-to-power-bi"></a>Konwersja typu danych z ASA do usługi Power BI
Usługa Azure Stream Analytics aktualizacji modelu danych dynamicznie w czasie wykonywania w przypadku zmiany schematu danych wyjściowych. Zmiany nazwy kolumny, zmian w kolumnach typu i dodanie lub usunięcie kolumny są wszystkie śledzone.

W tej tabeli podano konwersje typów danych z [Stream Analytics typy danych](https://msdn.microsoft.com/library/azure/dn835065.aspx) do zasilania BIs [typy modelu danych jednostki (EDM)](https://powerbi.microsoft.com/documentation/powerbi-developer-walkthrough-push-data/) do usługi POWER BI zestawu danych i tabeli nie istnieją.


Z usługi Stream Analytics | Do usługi Power BI
-----|-----|------------
bigint | Int64
nvarchar(max) | Ciąg
Data i godzina | Data i godzina
Float | O podwójnej precyzji
Tablica rekordu | Ciąg typu, wartości stałej "IRecord" lub "IArray"

### <a name="schema-update"></a>Aktualizacja schematu
Analiza strumienia wnioskuje schemat modelu danych, na podstawie pierwszego zestawu zdarzeń w danych wyjściowych. Później w razie potrzeby schematu modelu danych zostało zaktualizowane do uwzględnienia zdarzeń przychodzących, które mogą nie pasować do oryginalnego schematu.

`SELECT *` Zapytania należy unikać zapobiec aktualizacji dynamicznej schematu w wierszach. Oprócz potencjalnego wpływu na wydajność również może powodować indeterminacy czas poświęcony na wyniki. Należy wybrać dokładnie pola, które muszą być wyświetlane na pulpicie nawigacyjnym usługi Power BI. Ponadto wartości danych powinny być zgodne z wybranym typem danych.


Poprzednie/bieżącym | Int64 | Ciąg | Data i godzina | O podwójnej precyzji
-----------------|-------|--------|----------|-------
Int64 | Int64 | Ciąg | Ciąg | O podwójnej precyzji
O podwójnej precyzji | O podwójnej precyzji | Ciąg | Ciąg | O podwójnej precyzji
Ciąg | Ciąg | Ciąg | Ciąg |  | Ciąg | 
Data i godzina | Ciąg | Ciąg |  Data i godzina | Ciąg


### <a name="renew-power-bi-authorization"></a>Odnów Power BI autoryzacji
Konieczne będzie ponownego uwierzytelnienia konta usługi Power BI, jeśli jego hasło uległ zmianie od czasu utworzenia lub ostatniej uwierzytelniony zadania. Jeśli uwierzytelnianie wieloskładnikowe (MFA) jest skonfigurowany w dzierżawie usługi Azure Active Directory (AAD), należy również odnowić autoryzacji usługi Power BI co 2 tygodnie. Objawem tego problemu jest Brak danych wyjściowych zadania i "uwierzytelnić użytkownika błąd" w dzienniki operacji:

  ![Błąd tokenu odświeżania Power BI](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

Aby rozwiązać ten problem, Zatrzymaj uruchomione zadania i przejdź do usługi Power BI dane wyjściowe.  Kliknij łącze "Odnawiania autoryzacji", a następnie uruchom ponownie zadanie z czas ostatniego zatrzymania w celu uniknięcia utraty danych.

  ![Usługa Power BI odnowić autoryzacji](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Table Storage
[Magazyn tabel Azure](../storage/common/storage-introduction.md) oferuje magazynu wysokiej dostępności, skalowalna na ogromną skalę, dzięki czemu aplikacja może automatycznie skalować, aby spełnić wymagania użytkownika. Magazyn tabel jest magazynem kluczy/atrybutów NoSQL firmy Microsoft które z nich mogą korzystać z danych strukturalnych z mniej ograniczeń w schemacie. Magazyn tabel Azure może służyć do przechowywania danych na potrzeby trwałości i efektywnego odzyskiwania.

W poniższej tabeli wymieniono nazwy właściwości i ich opis tworzenia tabeli danych wyjściowych.

| Nazwa właściwości | Opis |
| --- | --- |
| Alias wyjściowy |Jest to przyjazna nazwa używana w zapytaniach do kierowania wyników zapytania do magazynu w tej tabeli. |
| Konto magazynu |Nazwa konta magazynu, w którym wysyłania danych wyjściowych. |
| Klucz konta magazynu |Klucz dostępu skojarzone z kontem magazynu. |
| Nazwa tabeli |Nazwa tabeli. Jeśli nie istnieje tabela zostaną utworzone. |
| Klucz partycji |Nazwa kolumny wyjściowej zawierającej klucz partycji. Klucz partycji to unikatowy identyfikator partycji w danej tabeli, który wchodzi w skład pierwszej części klucza podstawowego jednostki. Jest to wartość ciągu o maksymalnym rozmiarze 1 KB. |
| Klucz wiersza |Nazwa kolumny wyjściowej zawierającej klucz wiersza. Klucz wiersza to unikatowy identyfikator jednostki w danej partycji. Wchodzi w skład drugiej części klucza podstawowego jednostki. Klucz wiersza to wartość ciągu o maksymalnym rozmiarze 1 KB. |
| Rozmiar partii |Liczba rekordów dla operacji zbiorczej. Zazwyczaj wartość domyślna to wystarczające dla większości zadań, zapoznaj się [specyfikacji operacji zbiorczej tabeli](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) Aby uzyskać więcej informacji na temat modyfikowania to ustawienie. |
 
## <a name="service-bus-queues"></a>Kolejki usługi Service Bus
[Kolejek usługi Service Bus](https://msdn.microsoft.com/library/azure/hh367516.aspx) oferują pierwszy na wejściu — pierwszy na wyjściu (FIFO) dostarczanie komunikatów dla co najmniej jeden konkurujących konsumentów. Zazwyczaj wiadomości powinny były odbierane i przetwarzane przez odbiorców w kolejności, w której zostały dodane do kolejki, a każdy komunikat jest odbierany i przetwarzany przez tylko jednego odbiorcę komunikatów.

W poniższej tabeli wymieniono nazwy właściwości i ich opis tworzenia kolejki dane wyjściowe.

| Nazwa właściwości | Opis |
| --- | --- |
| Alias wyjściowy |Jest to przyjazna nazwa używana w zapytaniach do kierowania wyników zapytania do tej kolejki magistrali usług. |
| Namespace magistrali usług |Przestrzeń nazw magistrali usług to kontener dla zestawu jednostek do obsługi komunikatów. |
| Nazwa kolejki |Nazwa kolejki magistrali usług. |
| Nazwa zasad kolejki |Podczas tworzenia kolejki można też utworzyć zasady dostępu współużytkowanego na karcie Konfigurowanie kolejki. Wszystkie zasady dostępu współużytkowanego mają nazwy, uprawnienia, które można ustawić, a klucze dostępu. |
| Klucz zasad kolejki |Dostęp współdzielony klucz używany do uwierzytelniania dostępu do przestrzeni nazw usługi Service Bus |
| Format serializacji zdarzeń |Format serializacji w danych wyjściowych.  JSON, CSV i Avro są obsługiwane. |
| Encoding |Dla woluminu CSV i JSON UTF-8 to jedyny obsługiwany format kodowania w tej chwili |
| Ogranicznik |Dotyczy tylko serializacji woluminów CSV. Usługa Stream Analytics obsługuje różne ograniczniki dla serializacji danych w formacie CSV. Obsługiwane wartości to przecinek, średnik, miejsca, karta i pionowy pasek. |
| Format |Dotyczy tylko typu JSON. Rozdzielone Określa, czy dane wyjściowe będą formatowane przez poszczególne obiekty JSON rozdzielone znakiem nowego wiersza. Tablica Określa, czy dane wyjściowe będą formatowane jako tablica obiektów JSON. |

## <a name="service-bus-topics"></a>Tematy dotyczące usługi Service Bus
Gdy kolejek usługi Service Bus udostępnia metody komunikacji jeden-do-jednego od nadawcy do odbiornika, [tematów usługi Service Bus](https://msdn.microsoft.com/library/azure/hh367516.aspx) Podaj jeden do wielu formę komunikacji.

W poniższej tabeli wymieniono nazwy właściwości i ich opis tworzenia tabeli danych wyjściowych.

| Nazwa właściwości | Opis |
| --- | --- |
| Alias wyjściowy |Jest to przyjazna nazwa używana w zapytaniach do kierowania wyników zapytania w tym temacie magistrali usługi. |
| Namespace magistrali usług |Przestrzeń nazw magistrali usług to kontener dla zestawu jednostek do obsługi komunikatów. Podczas tworzenia nowego Centrum zdarzeń, utworzono przestrzeni nazw usługi Service Bus |
| Nazwa tematu |Tematy są obsługi komunikatów przypominające centra zdarzeń i kolejki, jednostek. Służą one do zbierania strumieni zdarzeń z różnych urządzeń i usług. Podczas tworzenia tematu podano również określonej nazwy. Komunikaty wysłane do tematu nie będą dostępne, chyba że jest tworzony subskrypcji, dlatego upewnij się, co najmniej jedna subskrypcja w temacie |
| Nazwa zasad tematu |Podczas tworzenia tematu można też utworzyć zasady dostępu współużytkowanego na karcie Konfigurowanie tematu. Wszystkie zasady dostępu współdzielonego zostanie mają nazwy, możesz ustawić uprawnienia i klucze dostępu |
| Klucz tematu zasad |Dostęp współdzielony klucz używany do uwierzytelniania dostępu do przestrzeni nazw usługi Service Bus |
| Format serializacji zdarzeń |Format serializacji w danych wyjściowych.  JSON, CSV i Avro są obsługiwane. |
 | Encoding |Jeśli formatu CSV lub JSON, należy określić kodowania. UTF-8 w tej chwili jest obsługiwany tylko format kodowania |
| Ogranicznik |Dotyczy tylko serializacji woluminów CSV. Usługa Stream Analytics obsługuje różne ograniczniki dla serializacji danych w formacie CSV. Obsługiwane wartości to przecinek, średnik, miejsca, karta i pionowy pasek. |

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure DB rozwiązania Cosmos](https://azure.microsoft.com/services/documentdb/) jest globalnie rozproszone i wiele modeli bazy danych usługi zagwarantowanie oferty nieograniczona elastycznego skalowania wokół globu, zaawansowane zapytania i automatycznego indeksowania za pośrednictwem modeli danych pochodzącego od dowolnego schematu, małe opóźnienia i branży kompleksowe umów SLA.

Poniższa lista zawiera szczegóły nazwy właściwości i ich opis tworzenia bazy danych Azure rozwiązania Cosmos danych wyjściowych.

* **Dane wyjściowe Alias** — jako alias, aby znaleźć te dane wyjściowe w kwerendzie ASA  
* **Nazwa konta** — nazwa lub identyfikator URI konta DB rozwiązania Cosmos punktu końcowego.  
* **Klucz konta** — współużytkowanego klucza dostępu dla konta DB rozwiązania Cosmos.  
* **Baza danych** — Nazwa bazy danych DB rozwiązania Cosmos.  
* **Wzorzec nazwy kolekcji** — Nazwa kolekcji lub ich wzorca kolekcji do użycia. Format nazw kolekcji można skonstruować przy użyciu tokenu opcjonalne {partition}, gdzie partycje zaczynają się od 0. Poniżej przedstawiono przykładowe prawidłowe wartości wejściowe:  
  1\) MyCollection — jedną kolekcję o nazwie "MyCollection" musi istnieć.  
  2\) MyCollection {partition} — takie kolekcje muszą istnieć — "MyCollection0", "MyCollection1", "MyCollection2" itd.  
* **Klucz partycji** — jest to opcjonalne. Jest to potrzebne tylko, jeśli używasz tokenu {partycjonowania} we wzorcu nazwy Twojej kolekcji. Nazwa pola w zdarzeniach wyjściowych służąca do określenia klucza do partycjonowania danych wyjściowych na kolekcje. Dla danych wyjściowych jednej kolekcji, wszystkie kolumny wyjściowej dowolnego mogą być używane np. PartitionId.  
* **Identyfikator dokumentu** — jest to opcjonalne. Nazwa pola w zdarzeniach wyjściowych służąca do określenia klucza podstawowego, na które insert lub update bazują operacje.  

## <a name="azure-functions-in-preview"></a>Środowisko Azure Functions (w wersji zapoznawczej)
Azure Functions to bezserwerowa usługa obliczeniowa, która umożliwia uruchamianie kodu na żądanie bez konieczności jawnego przydzielania infrastruktury ani zarządzania tą infrastrukturą. Umożliwia ona implementowania kodu wyzwalane przez zdarzenia występujące w Azure lub usług innych firm.  Tę możliwość usługi Azure Functions odpowiedzieć wyzwalaczy ułatwia fizycznych dane wyjściowe do usługi Azure Stream Analytics. Ta karta danych wyjściowych umożliwia użytkownikom Stream Analytics nawiązać połączenia z usługi Azure Functions, a następnie uruchom skrypt lub fragmentu kodu w odpowiedzi na szereg zdarzeń.

Usługa Azure Stream Analytics wywołuje usługi Azure Functions za pośrednictwem protokołu HTTP wyzwalaczy. Nowa karta danych wyjściowych funkcji Azure jest dostępna w programie można skonfigurować następujące właściwości:

| Nazwa właściwości | Opis |
| --- | --- |
| Funkcja aplikacji |Nazwa aplikacji Azure Functions |
| Funkcja |Nazwa funkcji w funkcje aplikacji Azure |
| Rozmiar partii maksymalna |Tej właściwości można ustawić maksymalny rozmiar poszczególnych partii danych wyjściowych, który zostanie wysłany do funkcji Azure. Domyślnie ta wartość jest 256 KB |
| Maksymalna liczba wsadów  |Jak wskazuje nazwa, ta właściwość pozwala określić maksymalną liczbę zdarzeń w każdej z partii, które są wysyłane do usługi Azure Functions. Wartość domyślna partii maksymalna liczba to 100 |
| Klucz |Jeśli chcesz użyć funkcji platformy Azure z inną subskrypcję, możesz to zrobić, podając klucza, który ma dostęp do funkcji |

Należy pamiętać, że gdy Azure Stream Analytics odbiera 413 wyjątek (http żądania jednostki zbyt duży) z funkcji platformy Azure, zmniejsza rozmiar partii, który wysyła do usługi Azure Functions. W kodzie funkcji platformy Azure Użyj tego wyjątku aby upewnić się, że usługi Azure Stream Analytics nie wysyła partie zbyt duży. Ponadto upewnij się, że wartości liczby i rozmiaru partii max używany w funkcji są zgodne z wartościami w portalu usługi analiza strumienia. 

Ponadto w sytuacji, gdy nie ma żadnego zdarzenia, kierowanych do przedział czasu, jest generowane żadne dane wyjściowe. W związku z tym nie zostanie wywołana funkcja computeResult. To zachowanie jest zgodne z wbudowanych funkcji agregujących w oknie.


## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dalszą pomoc, skorzystaj z naszego [forum usługi Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Następne kroki
W niniejszym artykule przedstawiono usługę Stream Analytics — zarządzaną usługę służącą do analizy danych przesyłanych strumieniowo z Internetu rzeczy. Aby dowiedzieć się więcej na temat tej usługi, zobacz:

* [Get started using Azure Stream Analytics (Rozpoczynanie pracy z usługą Azure Stream Analytics)](stream-analytics-real-time-fraud-detection.md)
* [Scale Azure Stream Analytics jobs (Skalowanie zadań usługi Azure Stream Analytics)](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics Query Language Reference (Dokumentacja dotycząca języka zapytań usługi Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Management REST API Reference (Dokumentacja interfejsu API REST zarządzania usługą Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
