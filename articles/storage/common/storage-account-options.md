---
title: "Opcje konta usługi Azure Storage | Microsoft Docs"
description: "Opis opcji korzystania z usługi Azure Storage."
services: storage
documentationcenter: 
author: jirwin
manager: jwillis
editor: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/11/2017
ms.author: jirwin
ms.openlocfilehash: 7f07734433694999d38429ca264c58c5f3c619e1
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/19/2017
---
# <a name="azure-storage-account-options"></a>Opcje konta usługi Azure Storage

## <a name="overview"></a>Omówienie
Usługa Azure Storage udostępnia trzy różne opcje kont z różnymi cenami i obsługiwanymi funkcjami. Ważne jest, aby użytkownicy wzięli pod uwagę te różnice, wybierając opcję najlepszą w przypadku używanych aplikacji.  Oto trzy różne opcje:

* Konta **ogólnego przeznaczenia, wersja 2 (GPv2)** oferują wszystkie najnowsze funkcje i obsługują obiekty blob, pliki, kolejki i tabele. Obecnie te najnowsze funkcje obejmują obsługę warstw na poziomie obiektów blob, magazyn w warstwie Archiwum, limity konta w wyższej skali i zdarzenia magazynu. Cennik został zaprojektowany tak, aby udostępniać najniższe ceny za GB oraz konkurencyjne w branży ceny transakcji.

* Konta **usługi Blob Storage** oferują najnowsze funkcje blokowych obiektów blob, ale obsługują tylko blokowe obiekty blob.  Cennik jest w znacznej mierze podobny do cennika kont ogólnego przeznaczenia w wersji 2. Zachęcamy większość użytkowników do korzystania z kont ogólnego przeznaczenia w wersji 2, zamiast kont usługi Blob Storage.

* Konta **ogólnego przeznaczenia, wersja 1 (GPv1)** oferują możliwość korzystania ze wszystkich usług Azure Storage, ale mogą nie zapewniać dostępu do najnowszych funkcji lub najniższych cen za GB. Na przykład warstwy magazynowania Chłodna i Archiwum nie są obsługiwane w przypadku kont GPv1.  Ceny transakcji są niższe, dlatego obciążenia z dużą liczbą zmian lub operacji odczytu mogą skorzystać z zalet tego typu konta.

### <a name="changing-account-kind"></a>Zmienianie rodzaju konta
Użytkownicy można uaktualnić konta GPv1 albo konta usługi Blob Storage do konta GPv2 w dowolnej chwili za pomocą portalu, interfejsu wiersza polecenia lub programu PowerShell. Tej zmiany nie można cofnąć i żadne inne zmiany nie są dozwolone.

## <a name="general-purpose-v2"></a>Ogólnego przeznaczenia, wersja 2
Konta **ogólnego przeznaczenia, wersja 2 (GPv2)** to konta magazynu, które obsługują wszystkie funkcje wszystkich usług magazynu, w tym obiekty blob, pliki, kolejki i tabele. W przypadku blokowych obiektów blob można wybrać warstwę magazynowania Gorąca lub Chłodna na poziomie konta. Na podstawie wzorców dostępu można też wybrać warstwę Gorąca, Chłodna lub Archiwum na poziomie obiektu blob. Przechowuj dane, do których uzyskujesz dostęp często, niezbyt często i rzadko, odpowiednio w warstwach magazynowania Gorąca, Chłodna i Archiwum, aby zoptymalizować koszty. Należy pamiętać, że dowolne konto GPv1 można uaktualnić do konta GPv2 za pomocą portalu, interfejsu wiersza polecenia lub programu PowerShell. Konta GPv2 obsługują wszystkie interfejsy API i funkcje obsługiwane w przypadku kont usługi Blob Storage i GPv1 oraz oferują te same atrakcyjne funkcje dotyczące trwałości, dostępności, skalowalności i wydajności, co te typy kont.

Konta usługi Blob Storage uwidaczniają atrybut **Warstwa dostępu** na poziomie konta. Określa on domyślną warstwę konta magazynu: **Gorąca** lub **Chłodna**. Domyślna warstwa konta magazynu jest stosowana do każdego obiektu blob, który nie ma jawnie ustawionej warstwy na poziomie obiektu blob. W przypadku zmiany wzorca użycia danych można także w dowolnym momencie przełączyć się między tymi warstwami magazynowania. **Warstwę Archiwum** można stosować tylko na poziomie obiektu blob.

> [!NOTE]
> Zmiana warstwy magazynowania może spowodować naliczenie dodatkowych opłat. Więcej szczegółów zawiera sekcja dotycząca [cennika i rozliczeń](#pricing-and-billing).

## <a name="blob-storage-accounts"></a>Konta usługi Blob Storage

**Konta usługi Blob Storage** obsługują te same funkcje blokowych obiektów blob co konta GPv2, są jednak ograniczone tylko do obsługi blokowych obiektów blob. Klienci powinni zapoznać się z różnicami cen kont usługi Blob Storage i GPv2, a następnie rozważyć uaktualnienie do konta GPv2. Należy pamiętać, że tego uaktualnienia nie można cofnąć.

> [!NOTE]
> Konta usługi Blob Storage obsługują tylko blokowe obiekty blob i uzupełnialne obiekty blob — stronicowe obiekty blob nie są obsługiwane.

## <a name="general-purpose-v1"></a>Ogólnego przeznaczenia, wersja 1
Konta **ogólnego przeznaczenia, wersja 1 (GPv1)** to najstarsze konta magazynu i jedyny rodzaj, który może być używany w klasycznym modelu wdrażania. Funkcje, takie jak magazyn w warstwie Chłodna i Archiwum, są niedostępne w przypadku kont GPv1. Konta GPv1 mają zazwyczaj wyższe koszty magazynowania za GB, ale niższe koszty transakcji niż konta GPv2 lub Blob Storage.

## <a name="recommendations"></a>Zalecenia

Aby uzyskać więcej informacji dotyczących kont magazynu, zobacz [Informacje o kontach magazynu Azure](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

W przypadku aplikacji wymagających tylko magazynu blokowych obiektów blob lub magazynu uzupełnialnych obiektów blob zaleca się użycie kont GPv2, co pozwoli na korzystanie ze zróżnicowanego modelu cenowego magazynu warstwowego. Zdajemy sobie jednak sprawę z tego, że może to nie być możliwe w pewnych okolicznościach, gdy najlepszym rozwiązaniem jest użycie kont magazynu GPv1. Przykład:

* Nadal musisz korzystać z klasycznego modelu wdrożenia. Konta magazynu Blob Storage są dostępne tylko za pośrednictwem modelu wdrażania przy użyciu usługi Azure Resource Manager.

* Używasz dużej liczby transakcji lub dużej przepustowości replikacji geograficznej, które są droższe w przypadku kont GPv2 i Blob Storage niż kont GPv1, i nie masz wystarczająco dużego magazynu, aby skorzystać z zalet niższych kosztów magazynu za GB.

* Używasz wersji [interfejsu API REST usług Storage](https://msdn.microsoft.com/library/azure/dd894041.aspx) wcześniejsza niż 2014-02-14 lub biblioteka klienta w wersji wcześniejszej niż 4.x i nie można uaktualnić aplikacji.

> [!NOTE]
> Konta usługi Blob Storage są obecnie obsługiwane we wszystkich regionach platformy Azure.

## <a name="pricing-and-billing"></a>Cennik i rozliczenia
Wszystkie konta magazynu używają modelu cenowego dla magazynu obiektów blob opartego na warstwie każdego obiektu blob. W przypadku korzystania z konta magazynu mają zastosowanie następujące zagadnienia dotyczące rozliczeń:

* **Koszty usługi Storage**: oprócz ilości przechowywanych danych koszt przechowywania danych różni się w zależności od warstwy magazynowania. Koszt za gigabajt zmniejsza się w miarę, jak warstwa staje się chłodniejsza.

* **Koszty dostępu do danych**: opłaty za dostęp do danych wzrastają w miarę, jak warstwa staje się chłodniejsza. W przypadku danych w warstwie magazynowania Chłodna i Archiwum naliczana jest opłata za dostęp do danych za każdy gigabajt dla operacji odczytu.

* **Koszty transakcji**: w przypadku wszystkich warstw naliczana jest opłata za transakcję, która wzrasta w miarę, jak warstwa staje się chłodniejsza.

* **Koszty transferu danych replikacji geograficznej**: dotyczy to tylko kont ze skonfigurowaną replikacją geograficzną, w tym GRS i RA-GRS. Transfer danych w ramach replikacji geograficznej powoduje naliczanie opłaty za każdy gigabajt.

* **Koszty transferu danych wychodzących**: transfery danych wychodzących (dane przesyłane poza region platformy Azure) powodują naliczanie opłat za zużycie przepustowości za każdy gigabajt, co jest spójne z kontami magazynu ogólnego przeznaczenia.

* **Zmiana warstwy magazynowania**: zmiana warstwy magazynowania z Chłodna na Gorąca spowoduje naliczenie opłaty równej opłacie za odczytanie wszystkich danych istniejących w ramach konta magazynu. Natomiast zmiana warstwy magazynowania konta z Gorąca na Chłodna spowoduje naliczenie opłaty równej opłacie za zapisanie wszystkich danych w warstwie Chłodna (tylko konta GPv2).

> [!NOTE]
> Dodatkowe szczegóły dotyczące modelu cenowego dla kont usługi Blob Storage można znaleźć na stronie [Cennik usługi Azure Storage](https://azure.microsoft.com/pricing/details/storage/). Dodatkowe szczegóły dotyczące opłat za transfer danych wychodzących można znaleźć na stronie [Szczegóły cennika transferów danych](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="quickstart-scenarios"></a>Scenariusze typu Szybki start

W tej sekcji przedstawiono następujące scenariusze obejmujące użycie witryny Azure Portal:

* Tworzenie konta magazynu GPv2.
* Konwertowanie konta GPv1 lub usługi Blob Storage na konto magazynu GPv2.
* Ustawianie warstwy obiektu blob i konta na koncie magazynu GPv2.

W poniższych przykładach nie można ustawić warstwy dostępu na Archiwum, ponieważ to ustawienie dotyczy całego konta magazynu. Warstwę archiwalną można ustawić tylko dla określonego obiektu blob.

### <a name="create-a-gpv2-storage-account-using-the-azure-portal"></a>Tworzenie konta magazynu GPv2 w witrynie Azure Portal

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. W menu Centrum wybierz kolejno pozycje **Nowe** > **Dane i usługa Storage** > **Konto usługi Storage**.

3. Wprowadź nazwę konta magazynu.

    Nazwa musi być globalnie unikatowa, ponieważ jest używana jako część adresu URL, z którego korzysta się, aby uzyskać dostęp do obiektów na koncie magazynu.  

4. Wybierz pozycję **Resource Manager** jako model wdrażania.

    Magazynu warstwowego można używać tylko z kontami magazynu usługi Resource Manager — jest to zalecany model wdrażania dla nowych zasobów. Aby uzyskać więcej informacji, zobacz temat [Omówienie usługi Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).  

5. Z listy rozwijanej Rodzaj konta wybierz pozycję **Ogólnego przeznaczenia, wersja 2**.

    Tutaj wybierasz typ konta magazynu. Magazyn warstwowy nie jest dostępny w magazynie ogólnego przeznaczenia — jest on dostępny tylko w ramach konta typu Blob Storage.     

    Po wybraniu tej opcji warstwa wydajności jest ustawiana na Standardowa. Magazyn warstwowy nie jest dostępny z warstwą wydajności Premium.

6. Wybierz opcję replikacji dla konta magazynu: **LRS**, **GRS** lub **RA-GRS**. Wartość domyślna to **RA-GRS**.

    LRS = magazyn lokalnie nadmiarowy; GRS = magazyn geograficznie nadmiarowy (dwa regiony); RA-GRS to magazyn geograficznie nadmiarowy z dostępem do odczytu (2 regiony z dostępem do odczytu do drugiego).

    Aby uzyskać więcej informacji na temat opcji replikacji usługi Azure Storage, zobacz [Azure Storage replication](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) (Replikacja usługi Azure Storage).

7. Wybierz odpowiednią dla Twoich potrzeb warstwę magazynu: ustaw pozycję **Warstwa dostępu** na wartość **Chłodna** lub **Gorąca**. Wartość domyślna to **Gorąca**.

8. Wybierz subskrypcję, w ramach której chcesz utworzyć nowe konto magazynu.

9. Określ nową grupę zasobów lub wybierz istniejącą grupę zasobów. Więcej informacji na temat grup zasobów znajduje się w temacie [Omówienie usługi Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

10. Wybierz region dla swojego konta magazynu.

11. Kliknij pozycję **Utwórz**, aby utworzyć konto magazynu.

### <a name="convert-a-gpv1-or-blob-storage-account-to-a-gpv2-storage-account-using-the-azure-portal"></a>Konwertowanie konta GPv1 lub usługi Blob Storage na konto magazynu GPv2 w witrynie Azure Portal

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. Aby przejść do konta magazynu, zaznacz pozycję Wszystkie zasoby, a następnie wybierz konto magazynu.

3. W bloku Ustawienia kliknij pozycję **Konfiguracja**.

4. W obszarze rodzaju konta kliknij pozycję **Uaktualnij**.

5. Po prawej stronie pojawi się nowy blok na potrzeby potwierdzenia. W obszarze potwierdzania uaktualnienia wpisz nazwę konta. 

5. Kliknij pozycję Uaktualnij w dolnej części bloku.

### <a name="change-the-storage-tier-of-a-gpv2-storage-account-using-the-azure-portal"></a>Dokonywanie zmiany warstwy magazynowania dla konta GPv2 w witrynie Azure Portal

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. Aby przejść do konta magazynu, zaznacz pozycję Wszystkie zasoby, a następnie wybierz konto magazynu.

3. W bloku Ustawienia kliknij pozycję **Konfiguracja**, aby wyświetlić i/lub zmienić konfigurację konta.

4. Wybierz odpowiednią dla Twoich potrzeb warstwę magazynu: ustaw pozycję **Warstwa dostępu** na wartość **Chłodna** lub **Gorąca**.

5. Kliknij pozycję Zapisz w górnej części bloku.

### <a name="change-the-storage-tier-of-a-blob-using-the-azure-portal"></a>Zmiana warstwy magazynowania obiektu blob za pośrednictwem witryny Azure Portal

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. Aby przejść do obiektu blob na koncie magazynu, wybierz pozycję Wszystkie zasoby, wybierz konto magazynu, wybierz kontener, a następnie wybierz obiekt blob.

3. W bloku Właściwości obiektu blob kliknij menu rozwijane **Warstwa dostępu**, aby wybrać warstwę magazynowania **Gorąca**, **Chłodna** lub **Archiwum**.

5. Kliknij pozycję Zapisz w górnej części bloku.

> [!NOTE]
> Zmiana warstwy magazynowania może spowodować naliczenie dodatkowych opłat. Więcej szczegółów zawiera sekcja dotycząca [cennika i rozliczeń](#pricing-and-billing).


## <a name="evaluating-and-migrating-to-gpv2-storage-accounts"></a>Ocenianie i migrowanie do kont magazynu GPv2
Informacje przedstawione w tej sekcji mają ułatwić użytkownikom płynne przejście do kont usługi GPv2 (w odróżnieniu od kont GPv1). Istnieją dwa scenariusze dotyczące użytkowników:

* Masz już istniejące konto magazynu GPv1 i chcesz ocenić jego zamianę na konto magazynu GPv2 z odpowiednio dobraną warstwą magazynowania.
* Zamierzasz używać konta magazynu GPv2 lub masz już takie konto i chcesz ocenić, czy będzie Ci bardziej odpowiadać warstwa magazynowania Gorąca czy Chłodna.

W obu przypadkach priorytetem jest oszacowanie kosztu magazynowania i uzyskiwania dostępu do danych przechowywanych na koncie GPv2 i porównanie go z obecnie ponoszonymi kosztami.

## <a name="evaluating-gpv2-storage-account-tiers"></a>Ocenianie warstw konta magazynu GPv2

Aby oszacować koszty magazynowania i uzyskiwania dostępu do danych przechowywanych na koncie GPv2, konieczne będzie dokonanie oceny istniejącego wzorca użycia lub określenie w przybliżeniu oczekiwanego wzorca użycia. Ogólnie potrzebne są odpowiedzi na następujące pytania:

* Wykorzystanie magazynu — jaka ilość danych jest magazynowana i jak ta ilość zmienia się w ciągu miesiąca?

* Wzorzec dostępu do magazynu — jak dużo danych jest odczytywanych z konta i zapisywanych na nim (w tym nowych danych)? Ile transakcji jest przeprowadzanych w celu uzyskania dostępu do danych i jakiego rodzaju są to transakcje?

## <a name="monitoring-existing-storage-accounts"></a>Monitorowanie istniejących kont magazynu

Aby monitorować istniejące konta magazynu i gromadzić uzyskane dane, można skorzystać z usługi Azure Storage Analytics, która umożliwia rejestrowanie i dostarcza danych metryk dotyczących konta magazynu. Usługa Storage Analytics może przechowywać metryki, które obejmują zagregowane statystyki transakcji oraz dane dyspozycyjności o żądaniach do usługi Storage dla następujących rodzajów kont: GPv1, GPv2 i Blob Storage. Te dane są przechowywane w dobrze znanych tabelach tego samego konta magazynu.

Aby uzyskać więcej szczegółowych informacji, zobacz artykuły [About Storage Analytics Metrics (Metryki w usłudze Storage Analytics)](https://msdn.microsoft.com/library/azure/hh343258.aspx) i [Storage Analytics Metrics Table Schema (Schemat tabeli metryk usługi Storage Analytics)](https://msdn.microsoft.com/library/azure/hh343264.aspx).

> [!NOTE]
> Konta usługi Blob Storage ujawniają punkt końcowy usługi tabel tylko w odniesieniu do przechowywania i uzyskiwania dostępu do danych metryk dla tego konta. Konta magazynu ZRS typu GPv1 nie obsługują danych metryk.

Aby monitorować użycie magazynu dla usługi Blob Storage, należy włączyć metryki pojemności.
Dzięki włączeniu tej opcji dane pojemności są rejestrowane codziennie dla konta usługi Blob Storage oraz rejestrowane jako wpis tabeli, który jest zapisywany w tabeli *$MetricsCapacityBlob* w obrębie tego samego konta magazynu.

Aby monitorować wzorzec dostępu do danych dla usługi Blob Storage, należy włączyć godzinowe metryki transakcji na poziomie interfejsu API. Dzięki włączeniu tej opcji transakcje interfejsu API są agregowane co godzinę i rejestrowane jako wpis tabeli, który jest zapisywany w tabeli *$MetricsHourPrimaryTransactionsBlob* w obrębie tego samego konta magazynu. W przypadku używania kont magazynu RA-GRS tabela *$MetricsHourSecondaryTransactionsBlob* rejestruje transakcje kierowane do pomocniczego punktu końcowego.

> [!NOTE]
> Jeśli masz konto magazynu ogólnego przeznaczenia, w którym są przechowywane stronicowe obiekty blob i dyski maszyny wirtualnej albo kolejki, pliki lub tabele (obok blokowych obiektów blob i danych uzupełnialnych obiektów blob), ten proces szacowania nie ma zastosowania. Dzieje się tak, ponieważ dane wydajności nie odróżniają blokowych obiektów blob od innych typów i nie oferują danych wydajności dla innych typów danych. Jeśli używasz tych typów, alternatywną metodologią będzie zapoznanie się z ilościami na najnowszym rachunku.

Aby uzyskać najbardziej zbliżone do prawdziwych informacje o użyciu danych i wzorcu dostępu, zalecamy wybranie takiego okresu przechowywania dla metryk, który odzwierciedla normalne użycie, i ekstrapolację. Jedną z opcji jest przechowywanie danych metryk przez siedem dni i zbieranie danych co tydzień, aby przeprowadzić analizę pod koniec miesiąca. Innym rozwiązaniem jest przechowywanie danych metryk z ostatnich 30 dni i zbieranie oraz analizowanie danych z końcem 30-dniowego okresu.

Aby uzyskać szczegółowe informacje na temat włączania metryk oraz gromadzenia i wyświetlania danych metryk, zobacz temat [Włączanie metryk usługi Azure Storage i wyświetlanie danych metryk](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

> [!NOTE]
> Za przechowywanie i pobieranie danych analitycznych oraz uzyskiwanie dostępu do nich również są naliczane opłaty, podobnie jak za zwykłe dane użytkowników.

### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Używanie metryk użycia do szacowania kosztów

### <a name="storage-costs"></a>Koszty magazynowania

Najnowszy wpis w tabeli metryk pojemności *$MetricsCapacityBlob* z kluczem wiersza *„data”* pokazuje pojemność magazynu zajętą przez dane użytkownika. Najnowszy wpis w tabeli metryk pojemności *$MetricsCapacityBlob* z kluczem wiersza *„analytics”* pokazuje pojemność magazynu zajętą przez dzienniki analiz.

Łączna pojemność zajęta przez dane użytkownika i dzienniki analiz (jeśli są włączone) może być użyta do szacowania kosztów magazynowania danych w ramach konta magazynu. Tę samą metodę można również użyć do szacowania kosztów magazynowania na kontach magazynu GPv1.

### <a name="transaction-costs"></a>Koszty transakcji

Suma *„TotalBillableRequests”* dla wszystkich wpisów interfejsu API w tabeli metryk transakcji wskazuje całkowitą liczbę transakcji dla tego konkretnego interfejsu API. *Na przykład* całkowitą liczbę transakcji *„GetBlob”* w danym okresie można obliczyć, sumując liczbę płatnych żądań dla wszystkich wpisów z kluczem wiersza *'user;GetBlob'*.

Aby oszacować koszty transakcji dla kont usługi Blob Storage, konieczne będzie podzielenie transakcji na trzy grupy, ponieważ mają one różne ceny.

* Transakcje zapisu, takie jak *„PutBlob”*, *„PutBlock”*, *„PutBlockList”*, *„AppendBlock"*, *„ListBlobs”*, *„ListContainers”*, *„CreateContainer”*, *„SnapshotBlob”* i *„CopyBlob”*.
* Transakcje usuwania, takie jak *„DeleteBlob”* i *„DeleteContainer”*.
* Wszystkie inne transakcje.

W celu oszacowania kosztów transakcji dla kont magazynu GPv1 należy zagregować wszystkie transakcje niezależnie od operacji/interfejsu API.

### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Dostęp do danych i koszty transferu danych replikacji geograficznej

Usługa Storage Analytics nie udostępnia informacji na temat ilości odczytywanych i zapisywanych danych na koncie magazynu, ale można je w przybliżeniu oszacować, analizując tabelę metryk transakcji. Suma *„TotalIngress”* dla wszystkich wpisów interfejsu API w tabeli metryk transakcji wskazuje całkowitą ilość danych przychodzących w bajtach dla tego konkretnego interfejsu API. Podobnie suma *„TotalEgress”* wskazuje całkowitą ilość danych wychodzących w bajtach.

Aby oszacować koszty dostępu do danych dla kont usługi Blob Storage, konieczne będzie podzielenie transakcji na dwie grupy.

* Ilość danych pobieranych z konta magazynu można oszacować, przyglądając się sumie *„TotalEgress”* przede wszystkim dla operacji *„GetBlob”* i *„CopyBlob”*.

* Ilość danych zapisywanych na koncie magazynu można oszacować, przyglądając się sumie *„TotalIngress”* przede wszystkim dla operacji *„PutBlob”*, *„PutBlock”*, *„CopyBlob”* i *„AppendBlock”*.

Koszt transferu danych replikacji geograficznej dla kont usługi Blob Storage można obliczyć, korzystając z kalkulacji ilości danych zapisanych w przypadku używania konta magazynu GRS lub RA-GRS.

> [!NOTE]
> Aby dowiedzieć się więcej na temat obliczania kosztów używania gorącej lub chłodnej warstwy magazynowania, zobacz *„What are Hot and Cool access tiers and how should I determine which one to use?” („Co to są warstwy dostępu Gorąca i Chłodna i jak określić, której użyć?”)* w FAQ (Często zadawanych pytaniach). na stronie z [cennikiem usługi Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

## <a name="migrating-existing-data"></a>Migrowanie istniejących danych

Konto GPv1 lub usługi Blob Storage można łatwo uaktualnić do wersji GPv2 bez przestojów i zmian interfejsów API oraz bez konieczności przenoszenia danych. Jest to jedna z podstawowych zalet kont GPv2 w porównaniu do kont usługi Blob Storage.

Jednak jeśli chcesz przeprowadzić migrację do konta usługi Blob Storage, możesz skorzystać z poniższych instrukcji.

Konto usługi Blob Storage jest przeznaczone do przechowywania tylko blokowych obiektów blob i uzupełnialnych obiektów blob. Istniejących kont magazynu ogólnego przeznaczenia, które umożliwiają przechowywanie tabel, kolejek, plików, dysków, a także obiektów blob, nie można konwertować na konta usługi Blob Storage. Aby użyć warstw magazynowania, należy utworzyć nowe konta usługi Blob Storage i przeprowadzić migrację istniejących danych do nowo utworzonych kont.

Poniższych metod można użyć do migrowania istniejących danych do kont usługi Blob Storage z lokalnych urządzeń magazynujących, od innych dostawców magazynu w chmurze lub z istniejących kont magazynu ogólnego przeznaczenia na platformie Azure:

### <a name="azcopy"></a>Narzędzie AzCopy

Narzędzie AzCopy to narzędzie wiersza polecenia systemu Windows przeznaczone do kopiowania z wysoką wydajnością danych z i do usługi Azure Storage. Narzędzia AzCopy można użyć do skopiowania danych na konto usługi Blob Storage z istniejących kont magazynu ogólnego przeznaczenia lub do przekazania danych z lokalnych urządzeń magazynujących na konto usługi Blob Storage.

Aby uzyskać więcej informacji, zobacz [Transfer danych za pomocą narzędzia wiersza polecenia AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="data-movement-library"></a>Biblioteka przenoszenia danych

Biblioteka przenoszenia danych usługi Azure Storage dla programu .NET jest oparta na podstawowym środowisku przenoszenia danych, w którym działa narzędzie AzCopy. Biblioteka została zaprojektowana na potrzeby przeprowadzania wysokowydajnych, niezawodnych i prostych operacji transferu danych w sposób podobny do narzędzia AzCopy. Pozwala to w pełni wykorzystać funkcje oferowane przez narzędzie AzCopy natywnie w aplikacji bez konieczności uruchamiania i monitorowania zewnętrznych wystąpień narzędzia AzCopy.

Aby uzyskać więcej informacji, zobacz [Azure Storage Data Movement Library for .Net](https://github.com/Azure/azure-storage-net-data-movement) (Biblioteka przenoszenia danych usługi Magazyn Azure dla platformy .NET).

### <a name="rest-api-or-client-library"></a>Interfejs API REST lub biblioteka klienta

Można utworzyć aplikację niestandardową służącą do migracji danych do konta usługi Blob Storage przy użyciu jednej z bibliotek klienta platformy Azure lub interfejsu API REST usług magazynu platformy Azure. Usługa Azure Storage udostępnia rozbudowane biblioteki dla wielu języków programowania i platform, takich jak .NET, Java, C++, Node.JS, PHP, Ruby i Python. Biblioteki klienta oferują zaawansowane możliwości, takie jak logika ponowień, rejestrowanie i przekazywanie równoległe. Możliwe jest również programowanie bezpośrednio przy użyciu interfejsu API REST, który może być wywoływany przez dowolny język programowania mający możliwość wysyłania żądań HTTP lub HTTPS.

Aby uzyskać więcej szczegółów, zobacz [Rozpoczynanie pracy z usługą Azure Blob Storage](../blobs/storage-dotnet-how-to-use-blobs.md).

> [!NOTE]
> Obiekty blob zaszyfrowane za pomocą szyfrowania po stronie klienta przechowują metadane związane z szyfrowaniem przechowywane w ramach obiektu blob. Jest absolutnie krytyczne, aby każdy mechanizm kopiowania zapewnił, że metadane obiektu blob, a w szczególności metadane związane z szyfrowaniem, zostały zachowane. Jeśli obiekty blob zostaną skopiowane bez takich metadanych, nie będzie można ponownie pobrać zawartości obiektu blob. Aby uzyskać więcej informacji na temat metadanych związanych z szyfrowaniem, zobacz [Azure Storage Client-Side Encryption](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) (Szyfrowanie po stronie klienta usługi Azure Storage).

## <a name="faq"></a>Często zadawane pytania

**Czy istniejące konta magazynu są nadal dostępne?**

Tak, istniejące konta magazynu są nadal dostępne, a ich ceny i funkcje nie zostały zmienione.  W przypadku takich kont nie ma możliwości wyboru warstwy magazynowania, a funkcje obsługi warstw nie zostaną wprowadzone w przyszłości.

**Kiedy i dlaczego należy rozpocząć używanie kont magazynu GPv2?**

Konta magazynu GPv2 zostały stworzone specjalnie tak, aby oferować najniższe koszty magazynowania za GB przy równoczesnym zachowaniu konkurencyjnych w branży kosztów dostępu do danych i transakcji. Idąc dalej, konta magazynu GPv2 to zalecany sposób przechowywania obiektów blob, ponieważ przyszłe możliwości, takie powiadomienia o zmianach, zostaną wprowadzone na podstawie tego typu konta. Decyzja o uaktualnieniu należy jednak do użytkownika i zależy od jego potrzeb biznesowych.  Może on na przykład zdecydować, że przed uaktualnieniem zoptymalizuje wzorce swoich transakcji.

**Czy mogę uaktualnić istniejące konto magazynu do konta GPv2?**

Tak. Konta GPv1 lub usługi Blob Storage można łatwo uaktualnić do wersji GPv2 w portalu.

**Czy mogę przechowywać obiekty w obu warstwach magazynowania w ramach tego samego konta?**

Tak. Atrybut **Warstwa dostępu** ustawiany na poziomie konta to domyślna warstwa mająca zastosowanie do wszystkich obiektów na tym koncie bez jawnie ustawionej warstwy. Jednak obsługa warstw na poziomie obiektów blob umożliwia ustawienie warstwy dostępu na poziomie obiektu niezależnie od ustawienia warstwy dostępu konta. Obiekty blob mogą istnieć w ramach tego samego konta w jednej z trzech warstw magazynowania (Gorąca, Chłodna lub Archiwum).

**Czy mogę zmienić warstwę magazynowania na moim koncie magazynu GPv2?**

Tak, zmiana warstwy magazynowania konta jest możliwa przez ustawienie atrybutu **Warstwa dostępu** dla konta magazynu. Zmiana warstwy magazynowania konta będzie miała zastosowanie do wszystkich obiektów przechowywanych na koncie, które nie mają jawnie ustawionej warstwy. Zmiana warstwy magazynowania z Gorąca na Chłodna spowoduje naliczenie opłat za operacje zapisu (za 10 000 operacji) (tylko konta magazynu GPv2). Natomiast zmiana warstwy z Chłodna na Gorąca spowoduje naliczenie opłat za operacje odczytu (za 10 000 operacji) i pobieranie danych (za GB), ponieważ konieczne jest odczytanie wszystkich danych w ramach konta.

**Jak często mogę zmieniać warstwę magazynowania na moim koncie usługi Blob Storage?**

Nie wymuszamy limitu częstotliwości zmiany warstwy magazynowania, ale należy pamiętać, że zmiana warstwy magazynowania z chłodnej na gorącą spowoduje naliczenie znaczących opłat. Nie zaleca się częstych zmian warstwy magazynowania.

**Czy obiekty blob w chłodnej warstwie magazynowania działają inaczej niż obiekty blob w gorącej warstwie magazynowania?**

Obiekty blob w warstwie magazynowania Gorąca na kontach GPv2 i Blob Storage mają takie samo opóźnienie jak obiekty blob na kontach magazynu GPv1. Obiekty blob w warstwie magazynowania Chłodna mają podobne opóźnienie (w milisekundach) jak obiekty blob w warstwie Gorąca. Obiekty blob w warstwie magazynowania Archiwum mają kilka godzin opóźnienia.

Obiekty blob w chłodnej warstwie magazynowania będą miały nieco niższy poziom dostępności usług (umowa SLA) niż obiekty blob przechowywane w gorącej warstwie magazynowania. Aby uzyskać więcej szczegółów, zobacz [Magazyn — umowa SLA](https://azure.microsoft.com/support/legal/sla/storage).

**Czy mogę przechowywać stronicowe obiekty i dyski maszyny wirtualnej na kontach usługi Blob Storage?**

Nie. Konta usługi Blob Storage obsługują tylko blokowe obiekty blob i uzupełnialne obiekty blob — stronicowe obiekty blob nie są obsługiwane. Dyski maszyny wirtualnej platformy Azure są oparte na stronicowych obiektach blob, w związku z czym konta usługi Blob Storage nie mogą być używane do przechowywania dysków maszyny wirtualnej. Istnieje jednak możliwość przechowywania kopii zapasowych dysków maszyny wirtualnej jako blokowych obiektów blob na kontach usługi Blob Storage. Jest to jedna z przyczyn, dla których warto rozważyć użycie kont GPv2 zamiast kont usługi Blob Storage.

**Czy muszę zmienić swoje istniejące aplikacje, aby umożliwić korzystanie z kont magazynu GPv2?**

Konta magazynu GPv2 są w 100% spójne z kontami GPv1 i kontami usługi Blob Storage w zakresie interfejsu API. Twoja aplikacja powinna działać, dopóki korzysta z blokowych obiektów blob lub uzupełnialnych obiektów blob i używasz wersji 2014-02-14 lub nowszej [interfejsu API REST usług Storage](https://msdn.microsoft.com/library/azure/dd894041.aspx). Jeśli używasz starszej wersji protokołu, musisz zaktualizować aplikację do nowej wersji, tak aby bezproblemowo współpracowała z oboma typami kont magazynu. Ogólnie zawsze zalecamy używanie najnowszej wersji niezależnie od używanego typu konta magazynu.

**Czy w środowisku użytkownika zostaną wprowadzone zmiany?**

Konta magazynu GPv2 są bardzo podobne do kont magazynu GPv1 i obsługują wszystkie kluczowe funkcje usługi Azure Storage, w tym funkcje związane z wysoką trwałością i dostępnością, skalowalnością, wydajnością i zabezpieczeniami. Wszystkie inne elementy niż opisane powyżej funkcje oraz ograniczenia dotyczące kont usługi Blob Storage i ich warstw magazynowania pozostają takie same w przypadku uaktualniania do konta GPv2 lub Blob Storage.

## <a name="next-steps"></a>Następne kroki

### <a name="evaluate-blob-storage-accounts"></a>Ocena kont usługi Blob Storage

[Sprawdzanie dostępności kont usługi Blob Storage według regionu](https://azure.microsoft.com/regions/#services)

[Ocena użycia bieżących kont magazynu przez włączenie metryk usługi Azure Storage](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Sprawdzanie cen usługi Blob Storage według regionu](https://azure.microsoft.com/pricing/details/storage/)

[Sprawdzanie ceny transferu danych](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-gpv2-storage-accounts"></a>Rozpoczynanie korzystania z kont magazynu GPv2

[Rozpoczynanie pracy z usługą Azure Blob Storage](../blobs/storage-dotnet-how-to-use-blobs.md)

[Przenoszenie danych do i z usługi Azure Storage](../common/storage-moving-data.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Transfer danych za pomocą narzędzia wiersza polecenia AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Przeglądaj i eksploruj konta magazynu](http://storageexplorer.com/)
