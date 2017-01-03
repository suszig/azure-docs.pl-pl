---
title: "Chłodny magazyn platformy Azure dla obiektów blob | Microsoft Docs"
description: "Warstwy magazynowania dla usługi Azure Blob Storage oferują efektywny kosztowo sposób magazynowania danych obiektu w oparciu o wzorce dostępu. Warstwa magazynu chłodnego została zoptymalizowana pod kątem danych, do których rzadziej uzyskuje się dostęp."
services: storage
documentationcenter: 
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: eb33ed4f-1b17-4fd6-82e2-8d5372800eef
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: mihauss
translationtype: Human Translation
ms.sourcegitcommit: c75658d173bcb3438d6f2725ec9ef2c4127013d7
ms.openlocfilehash: 0d0ca29a4733c681e044884697030ccd2916b6cb


---
# <a name="azure-blob-storage-hot-and-cool-storage-tiers"></a>Azure Blob Storage: warstwy magazynu gorącego i chłodnego
## <a name="overview"></a>Omówienie
Usługa Azure Storage oferuje teraz dwie warstwy magazynowania dla magazynu obiektów blob, dzięki czemu możliwe jest najbardziej ekonomiczne przechowywanie danych w zależności od sposobu ich używania. **Warstwa magazynu gorącego** platformy Azure została zoptymalizowana pod kątem przechowywania danych, do których często uzyskuje się dostęp. **Warstwa magazynu chłodnego** platformy Azure została zoptymalizowana pod kątem przechowywania danych, do których rzadko uzyskuje się dostęp i które mają długi okres życia. Dla danych w warstwie magazynu chłodnego nie ma znaczenia nieco niższa dostępność, ale nadal są wymagane wysoka trwałość oraz podobny czas dostępu i parametry przepływności jak w przypadku gorących danych. W przypadku chłodnych danych umowa SLA dotycząca nieco niższej dostępności i wyższe koszty dostępu to dopuszczalne wady, biorąc pod uwagę znacznie niższe koszty magazynowania.

Obecnie ilość danych przechowywanych w chmurze rośnie w tempie wykładniczym. Aby zarządzać kosztami zwiększających się potrzeb dotyczących magazynowania, warto zorganizować dane na podstawie atrybutów, takich jak częstotliwość dostępu i planowany okres przechowywania. Dane przechowywane w chmurze mogą być różne pod względem sposobu ich generowania i przetwarzania oraz uzyskiwania do nich dostępu przez cały okres ich istnienia. Do niektórych danych często uzyskuje się dostęp. Są one również często modyfikowane w trakcie całego okresu istnienia. Do niektórych danych często uzyskuje się dostęp na początkowym etapie istnienia, a z czasem już zdecydowanie rzadziej. Niektóre dane pozostają nieużywane w chmurze i dostęp do nich uzyskuje się rzadko (lub w ogóle) po umieszczeniu ich w magazynie.

Dla każdego z opisanych powyżej scenariuszy dostępu do danych istnieją korzyści ze zróżnicowanych warstw magazynowania zoptymalizowanych pod kątem określonego wzorca dostępu. Wraz z wprowadzeniem warstw magazynu gorącego i chłodnego usługa Azure Blob Storage zaspokaja tę potrzebę posiadania zróżnicowanych warstw magazynowania z oddzielnymi modelami cenowymi.

## <a name="blob-storage-accounts"></a>Konta usługi Blob Storage
**Konta usługi Blob Storage** to specjalne konta magazynu służące do przechowywania danych bez struktury jako obiekty blob (obiekty) w usłudze Azure Storage. Za pomocą kont usługi Blob Storage można teraz wybierać między warstwą magazynu gorącego a warstwą magazynu chłodnego w celu przechowywania rzadziej używanych chłodnych danych z niższym kosztem magazynowania i częściej używanych gorących danych z niższym kosztem dostępu. Konta usługi Blob Storage są podobne do istniejących kont magazynu ogólnego przeznaczenia i udostępniają wszystkie używane obecnie funkcje doskonałej trwałości, dostępności, skalowalności i wydajności, łącznie z pełną spójnością interfejsu API na potrzeby blokowych obiektów blob i uzupełnialnych obiektów blob.

> [!NOTE]
> Konta Magazynu obiektów blob obsługują tylko blokowe obiekty blob i uzupełnialne obiekty blob — stronicowe obiekty blob nie są obsługiwane.
> 
> 

Konta usługi Blob Storage udostępniają atrybut **Warstwa dostępu**, który umożliwia określenie warstwy magazynowania jako **Gorąca** lub **Chłodna** w zależności od danych przechowywanych w ramach konta. W przypadku zmiany wzorca użycia danych można także w dowolnym momencie przełączyć się między tymi warstwami magazynowania.

> [!NOTE]
> Zmiana warstwy magazynowania może spowodować naliczenie dodatkowych opłat. Więcej szczegółów zawiera sekcja dotycząca [cennika i rozliczeń](storage-blob-storage-tiers.md#pricing-and-billing).
> 
> 

Przykładowe scenariusze użycia dotyczące warstwy magazynu gorącego obejmują:

* Dane, które są często używane lub przewiduje się do nich częsty dostęp (odczyt i zapis danych).
* Dane, które są przygotowywane do przetwarzania i ewentualnej migracji do warstwy magazynu chłodnego.

Przykładowe scenariusze użycia dotyczące warstwy magazynu chłodnego obejmują:

* Zestawy danych usługi Backup, archiwizacji i odzyskiwania po awarii.
* Starszą zawartość nośników, która nie jest już często wyświetlana, ale oczekiwane jest, że będzie ona natychmiast dostępna, gdy będzie to wymagane.
* Duże zbiory danych, które muszą być przechowywane w sposób ekonomiczny, podczas gdy na potrzeby przyszłego przetwarzania zbierana jest większa ilość danych (*np.* długoterminowe magazynowanie danych naukowych lub nieprzetworzonych danych telemetrycznych z zakładu produkcyjnego).
* Oryginalne (nieprzetworzone) dane, które muszą zostać zachowane, nawet po przetworzeniu ich do ostatecznej użytecznej postaci (*np.* nieprzetworzone pliki multimedialne po transkodowaniu do innych formatów).
* Dane zgodności i dane archiwalne, które muszą być przechowywane przez długi czas, i do których bardzo rzadko uzyskuje się dostęp (*np.* zapisy z kamer monitorujących, stare zdjęcia rentgenowskie lub zdjęcia z rezonansu magnetycznego dla organizacji opieki zdrowotnej, nagrania audio i zapisy rozmów telefonicznych z klientami dla firm z branży usług finansowych).

Aby uzyskać więcej informacji dotyczących kont magazynu, zobacz [Informacje o kontach magazynu Azure](storage-create-storage-account.md).

Dla aplikacji wymagających tylko magazynu blokowych obiektów blob lub magazynu uzupełnialnych obiektów blob zaleca się użycie kont Magazynu obiektów blob, aby móc korzystać ze zróżnicowanego modelu cenowego magazynu warstwowego. Zdajemy sobie jednak sprawę z tego, że może to nie być możliwe w pewnych okolicznościach, gdy najlepszym rozwiązaniem jest użycie kont magazynu ogólnego przeznaczenia. Przykład:

* Musisz korzystać z tabel, kolejek lub plików, a obiekty blob mają być przechowywane na tym samym koncie magazynu. Należy pamiętać, że przechowywanie takich elementów w ramach tego samego konta nie przynosi żadnej korzyści technicznej oprócz posiadania takich samych udostępnionych kluczy.
* Nadal musisz korzystać z klasycznego modelu wdrożenia. Konta Magazynu obiektów blob są dostępne tylko za pośrednictwem modelu wdrażania przy użyciu usługi Azure Resource Manager.
* Musisz korzystać ze stronicowych obiektów blob. Konta Magazynu obiektów blob nie obsługują stronicowych obiektów blob. Ogólnie zaleca się używanie blokowych obiektów blob, chyba że istnieje konkretna potrzeba użycia stronicowych obiektów blob.
* Używasz wersji [interfejsu API REST usług Storage](https://msdn.microsoft.com/library/azure/dd894041.aspx) wcześniejsza niż 2014-02-14 lub biblioteka klienta w wersji wcześniejszej niż 4.x i nie można uaktualnić aplikacji.

> [!NOTE]
> Konta usługi Blob Storage są obecnie obsługiwane we wszystkich regionach platformy Azure.
> 
> 

## <a name="comparison-between-the-storage-tiers"></a>Porównanie warstw magazynowania
W poniższej tabeli znajduje się porównanie dwóch warstw magazynowania:

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250">
<col width="250">
<col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>Warstwa magazynu gorącego</center></strong></td>
    <td><strong><center>Warstwa magazynu chłodnego</center></strong></td
</tr>
<tr>
    <td><strong><center>Dostępność</center></strong></td>
    <td><center>99,9%</center></td>
    <td><center>99%</center></td>
</tr>
<tr>
    <td><strong><center>Dostępność<br>(odczyty RA GRS)</center></strong></td>
    <td><center>99,99%</center></td>
    <td><center>99,9%</center></td>
</tr>
<tr>
    <td><strong><center>Opłaty za zużycie</center></strong></td>
    <td><center>Wyższe koszty magazynowania<br>Niższe koszty dostępu i transakcji</center></td>
    <td><center>Niższe koszty magazynowania<br>Wyższe koszty dostępu i transakcji</center></td>
</tr>
<tr>
    <td><strong><center>Minimalny rozmiar obiektu<center></strong></td>
    <td colspan="2"><center>Nie dotyczy</center></td>
</tr>
<tr>
    <td><strong><center>Minimalny czas magazynowania<center></strong></td>
    <td colspan="2"><center>Nie dotyczy</center></td>
</tr>
<tr>
    <td><strong><center>Opóźnienie<br>(czas do pierwszego bajtu)<center></strong></td>
    <td colspan="2"><center>milisekundy</center></td>
</tr>
<tr>
    <td><strong><center>Cele dotyczące skalowalności i wydajności<center></strong></td>
    <td colspan="2"><center>Takie same jak w przypadku kont magazynu ogólnego przeznaczenia</center></td>
</tr>
</tbody>
</table>

> [!NOTE]
> Konta Magazynu obiektów blob obsługują te same cele wydajności i skalowalności co konta magazynu ogólnego przeznaczenia. Aby uzyskać więcej informacji, zobacz [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) (Cele dotyczące skalowalności i wydajności usługi Magazyn Azure).
> 
> 

## <a name="pricing-and-billing"></a>Cennik i rozliczenia
Konta usługi Blob Storage używają nowego modelu cenowego opartego na warstwie magazynowania. W przypadku korzystania z konta usługi Blob Storage mają zastosowanie następujące zagadnienia dotyczące rozliczeń:

* **Koszty usługi Storage**: oprócz ilości przechowywanych danych koszt przechowywania danych różni się w zależności od warstwy magazynowania. Koszt za każdy gigabajt jest niższy dla warstwy magazynu chłodnego niż dla warstwy magazynu gorącego.
* **Koszty dostępu do danych**: w przypadku danych w warstwie magazynu chłodnego zostanie naliczona opłata za dostęp do danych za każdy gigabajt dla operacji odczytu i zapisu.
* **Koszty transakcji**: dla obu warstw istnieje opłata za każdą transakcję. Jednak koszt każdej transakcji dla warstwy magazynu chłodnego jest wyższy niż w przypadku warstwy magazynu gorącego.
* **Koszty transferu danych replikacji geograficznej**: dotyczy to tylko kont ze skonfigurowaną replikacją geograficzną, w tym GRS i RA-GRS. Transfer danych w ramach replikacji geograficznej powoduje naliczanie opłaty za każdy gigabajt.
* **Koszty transferu danych wychodzących**: transfery danych wychodzących (dane przesyłane poza region platformy Azure) powodują naliczanie opłat za zużycie przepustowości za każdy gigabajt, co jest spójne z kontami magazynu ogólnego przeznaczenia.
* **Zmiana warstwy magazynowania**: zmiana warstwy magazynowania z chłodnej na gorącą spowoduje naliczenie opłaty równej opłacie za odczytanie wszystkich danych z konta magazynu dla każdego przejścia. Z drugiej strony zmiana warstwy magazynowania z gorącej na chłodną będzie bezpłatna.

> [!NOTE]
> Aby umożliwić użytkownikom wypróbowanie nowych warstw magazynowania i zweryfikowanie ich funkcjonalności po jej udostępnieniu, opłata za zmianę warstwy z chłodnej na gorącą nie będzie naliczana do 30 czerwca 2016 r. Od 1 lipca 2016 r. opłata będzie naliczana dla wszystkich przejść z warstwy chłodnej do gorącej. Dodatkowe szczegóły dotyczące modelu cenowego dla kont usługi Blob Storage można znaleźć na stronie [Cennik usługi Azure Storage](https://azure.microsoft.com/pricing/details/storage/). Dodatkowe szczegóły dotyczące opłat za transfer danych wychodzących można znaleźć na stronie [Szczegóły cennika transferów danych](https://azure.microsoft.com/pricing/details/data-transfers/).
> 
> 

## <a name="quick-start"></a>Szybki start
W tej sekcji zostaną przedstawione następujące scenariusze obejmujące użycie witryny Azure Portal:

* Tworzenie konta usługi Blob Storage.
* Zarządzanie kontem usługi Blob Storage.

### <a name="using-the-azure-portal"></a>Korzystanie z witryny Azure Portal
#### <a name="create-a-blob-storage-account-using-the-azure-portal"></a>Tworzenie konta usługi Blob Storage za pośrednictwem witryny Azure Portal
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. W menu Centrum wybierz kolejno pozycje **Nowe** > **Dane i usługa Storage** > **Konto usługi Storage**.
3. Wprowadź nazwę konta magazynu.
   
    Nazwa musi być globalnie unikatowa, ponieważ jest używana jako część adresu URL, z którego korzysta się, aby uzyskać dostęp do obiektów na koncie magazynu.  
4. Wybierz pozycję **Resource Manager** jako model wdrażania.
   
    Magazynu warstwowego można używać tylko z kontami magazynu usługi Resource Manager — jest to zalecany model wdrażania dla nowych zasobów. Aby uzyskać więcej informacji, zobacz temat [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).  
5. Z listy rozwijanej Rodzaj konta wybierz pozycję **Blob Storage**.
   
    Tutaj wybierasz typ konta magazynu. Magazyn warstwowy nie jest dostępny w magazynie ogólnego przeznaczenia — jest on dostępny tylko w ramach konta typu usługi Blob Storage.     
   
    Należy zauważyć, że po wybraniu tej opcji warstwa wydajności jest ustawiona na Standardowa. Magazyn warstwowy nie jest dostępny z warstwą wydajności Premium.
6. Wybierz opcję replikacji dla konta magazynu: **LRS**, **GRS** lub **RA-GRS**. Wartość domyślna to **RA-GRS**.
   
    LRS = magazyn lokalnie nadmiarowy; GRS = magazyn geograficznie nadmiarowy (2 regiony); RA-GRS to magazyn geograficznie nadmiarowy z dostępem do odczytu (2 regiony z dostępem do odczytu do drugiego).
   
    Aby uzyskać więcej informacji na temat opcji replikacji usługi Azure Storage, zobacz [Azure Storage replication](storage-redundancy.md) (Replikacja usługi Azure Storage).
7. Wybierz odpowiednią dla Twoich potrzeb warstwę magazynu: ustaw pozycję **Warstwa dostępu** na wartość **Chłodna** lub **Gorąca**. Wartość domyślna to **Gorąca**.
8. Wybierz subskrypcję, w ramach której chcesz utworzyć nowe konto magazynu.
9. Określ nową grupę zasobów lub wybierz istniejącą grupę zasobów. Więcej informacji na temat grup zasobów znajduje się w temacie [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
10. Wybierz region dla swojego konta magazynu.
11. Kliknij pozycję **Utwórz**, aby utworzyć konto magazynu.

#### <a name="change-the-storage-tier-of-a-blob-storage-account-using-the-azure-portal"></a>Dokonywanie zmiany warstwy magazynowania dla konta usługi Blob Storage za pośrednictwem witryny Azure Portal
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Aby przejść do konta magazynu, zaznacz pozycję Wszystkie zasoby, a następnie wybierz konto magazynu.
3. W bloku Ustawienia kliknij pozycję **Konfiguracja**, aby wyświetlić i/lub zmienić konfigurację konta.
4. Wybierz odpowiednią dla Twoich potrzeb warstwę magazynu: ustaw pozycję **Warstwa dostępu** na wartość **Chłodna** lub **Gorąca**.
5. Kliknij pozycję Zapisz w górnej części bloku.

> [!NOTE]
> Zmiana warstwy magazynowania może spowodować naliczenie dodatkowych opłat. Więcej szczegółów zawiera sekcja dotycząca [cennika i rozliczeń](storage-blob-storage-tiers.md#pricing-and-billing).
> 
> 

## <a name="evaluating-and-migrating-to-blob-storage-accounts"></a>Ocenianie i migrowanie do kont usługi Blob Storage
Informacje przedstawione w tej sekcji mają ułatwić użytkownikom płynne przejście do kont usługi Blob Storage. Istnieją dwa scenariusze dotyczące użytkowników:

* Masz już istniejące konto magazynu ogólnego przeznaczenia i chcesz ocenić jego zamianę na konto usługi Blob Storage z odpowiednio dobraną warstwą magazynowania.
* Zamierzasz używać konta usługi Blob Storage lub masz już takie konto i chcesz ocenić, czy będzie Ci bardziej odpowiadać gorąca czy chłodna warstwa magazynowania.

W obu przypadkach najpierw należy oszacować koszt magazynowania i uzyskiwania dostępu do danych przechowywanych na koncie usługi Blob Storage i porównać go z obecnie ponoszonymi kosztami.

### <a name="evaluating-blob-storage-account-tiers"></a>Ocenianie warstw dla konta usługi Blob Storage
Aby oszacować koszty magazynowania i uzyskiwania dostępu do danych przechowywanych na koncie usługi Blob Storage, konieczne będzie dokonanie oceny istniejącego wzorca użycia lub określenie w przybliżeniu oczekiwanego wzorca użycia. Ogólnie potrzebne są odpowiedzi na następujące pytania:

* Wykorzystanie magazynu — jaka ilość danych jest magazynowana i jak ta ilość zmienia się w ciągu miesiąca?
* Wzorzec dostępu do magazynu — jak dużo danych jest odczytywanych z konta i zapisywanych na nim (w tym nowych danych)? Ile transakcji jest przeprowadzanych w celu uzyskania dostępu do danych i jakiego rodzaju są to transakcje?

#### <a name="monitoring-existing-storage-accounts"></a>Monitorowanie istniejących kont magazynu
Aby monitorować istniejące konta magazynu i gromadzić uzyskane dane, można skorzystać z usługi Azure Storage Analytics, która umożliwia rejestrowanie i dostarcza danych metryk dotyczących konta magazynu.
Usługa Storage Analytics może przechowywać metryki, które obejmują zagregowane statystyki transakcji oraz dane dyspozycyjności o żądaniach do usługi Blob Storage, zarówno dla konta magazynu ogólnego przeznaczenia, jak i kont usługi Blob Storage.
Te dane są przechowywane w dobrze znanych tabelach tego samego konta magazynu.

Aby uzyskać więcej szczegółowych informacji, zobacz artykuły [About Storage Analytics Metrics](https://msdn.microsoft.com/library/azure/hh343258.aspx) (Metryki w usłudze Storage Analytics) i [Storage Analytics Metrics Table Schema](https://msdn.microsoft.com/library/azure/hh343264.aspx) (Schemat tabeli metryk usługi Storage Analytics)

> [!NOTE]
> Konta usługi Blob Storage ujawniają punkt końcowy usługi tabel tylko w odniesieniu do przechowywania i uzyskiwania dostępu do danych metryk dla tego konta.
> 
> 

Aby monitorować użycie magazynu dla usługi Blob Storage, należy włączyć metryki pojemności.
Dzięki włączeniu tej opcji dane pojemności są rejestrowane codziennie dla konta usługi Blob Storage oraz rejestrowane jako wpis tabeli, który jest zapisywany w tabeli *$MetricsCapacityBlob* w obrębie tego samego konta magazynu.

Aby monitorować wzorzec dostępu do danych dla usługi Blob Storage, należy włączyć godzinowe metryki transakcji na poziomie interfejsu API.
Dzięki włączeniu tej opcji transakcje interfejsu API są agregowane co godzinę i rejestrowane jako wpis tabeli, który jest zapisywany w tabeli *$MetricsHourPrimaryTransactionsBlob* w obrębie tego samego konta magazynu. W przypadku kont magazynu RA-GRS tabela *$MetricsHourSecondaryTransactionsBlob* rejestruje transakcje do pomocniczego punktu końcowego.

> [!NOTE]
> Jeśli masz konto magazynu ogólnego przeznaczenia, w którym są przechowywane stronicowe obiekty blob i dyski maszyny wirtualnej (obok blokowych obiektów blob i danych uzupełnialnych obiektów blob), ten proces szacowania nie ma zastosowania. Dzieje się tak, ponieważ nie ma możliwości odróżnienia metryk pojemności i transakcji na podstawie typu obiektu blob tylko dla blokowych obiektów blob i uzupełnialnych obiektów blob, które można poddać migracji do konta usługi Blob Storage.
> 
> 

Aby uzyskać najbardziej zbliżone do prawdziwych informacje o użyciu danych i wzorcu dostępu, zalecamy wybranie takiego okresu przechowywania dla metryk, który odzwierciedla normalne użycie i ekstrapolację.
Jedną z opcji jest przechowywanie danych metryk przez 7 dni i zbieranie danych co tydzień, aby przeprowadzić analizę pod koniec miesiąca.
Innym rozwiązaniem jest przechowywanie danych metryk z ostatnich 30 dni i zbieranie oraz analizowanie danych z końcem 30-dniowego okresu.

Aby uzyskać szczegółowe informacje na temat włączania, gromadzenia i wyświetlania danych metryk, zobacz temat [Enabling Azure Storage metrics and viewing metrics data](storage-enable-and-view-metrics.md) (Włączanie metryk usługi Azure Storage i wyświetlanie danych metryk).

> [!NOTE]
> Za przechowywanie, uzyskiwanie dostępu i pobieranie danych analitycznych są również naliczane opłaty, podobnie jak za zwykłe dane użytkowników.
> 
> 

#### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Używanie metryk użycia do szacowania kosztów
##### <a name="storage-costs"></a>Koszty magazynowania
Najnowszy wpis w tabeli metryk pojemności *$MetricsCapacityBlob* z kluczem wiersza *„data”* pokazuje pojemność magazynu zajętą przez dane użytkownika.
Najnowszy wpis w tabeli metryk pojemności *$MetricsCapacityBlob* z kluczem wiersza *„analytics”* pokazuje pojemność magazynu zajętą przez dzienniki analiz.

Łączna pojemność zajęta przez dane użytkownika i dzienniki analiz (jeśli są włączone) może być użyta do szacowania kosztów magazynowania danych w ramach konta magazynu.
Tę samą metodę można również użyć do szacowania kosztów magazynowania dla blokowych obiektów blob i uzupełnialnych obiektów blob w ramach konta magazynu ogólnego przeznaczenia.

##### <a name="transaction-costs"></a>Koszty transakcji
Suma *„TotalBillableRequests”* dla wszystkich wpisów interfejsu API w tabeli metryk transakcji wskazuje całkowitą liczbę transakcji dla tego konkretnego interfejsu API. *np.* całkowitą liczbę transakcji *„GetBlob”* w danym okresie można obliczyć, sumując liczbę płatnych żądań dla wszystkich wpisów z kluczem wiersza *'user;GetBlob'*.

Aby oszacować koszty transakcji dla kont magazynu usługi Blob Storage, konieczne będzie podzielenie transakcji na trzy grupy, ponieważ mają one różne ceny.

* Transakcje zapisu, takie jak *„PutBlob”*, *„PutBlock”*, *„PutBlockList”*, *„AppendBlock"*, *„ListBlobs”*, *„ListContainers”*, *„CreateContainer”*, *„SnapshotBlob”* i *„CopyBlob”*.
* Transakcje usuwania, takie jak *„DeleteBlob”* i *„DeleteContainer”*.
* Wszystkie inne transakcje.

W celu oszacowania kosztów transakcji dla kont magazynu ogólnego przeznaczenia należy zagregować wszystkie transakcje niezależnie od operacji/interfejsu API.

##### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Dostęp do danych i koszty transferu danych replikacji geograficznej
Usługa Storage Analytics nie udostępnia informacji na temat ilości odczytywanych i zapisywanych danych na koncie magazynu, ale można je w przybliżeniu oszacować, analizując tabelę metryk transakcji.
Suma *„TotalIngress”* dla wszystkich wpisów interfejsu API w tabeli metryk transakcji wskazuje całkowitą ilość danych przychodzących w bajtach dla tego konkretnego interfejsu API.
Podobnie suma *„TotalEgress”* wskazuje całkowitą ilość danych wychodzących w bajtach.

Aby oszacować koszty dostępu do danych dla kont magazynu usługi Blob Storage, konieczne będzie podzielenie transakcji na dwie grupy.

* Ilość danych pobieranych z konta magazynu można oszacować, przyglądając się sumie *„TotalEgress”* przede wszystkim dla operacji *„GetBlob”* i *„CopyBlob”*.
* Ilość danych zapisywanych na koncie magazynu można oszacować, przyglądając się sumie *„TotalIngress”* przede wszystkim dla operacji *„PutBlob”*, *„PutBlock”*, *„CopyBlob”* i *„AppendBlock”*.

Koszt transferu danych replikacji geograficznej dla kont magazynu usługi Blob Storage można obliczyć, korzystając z kalkulacji ilości danych zapisanych w przypadku konta magazynu GRS lub RA-GRS.

> [!NOTE]
> Aby dowiedzieć się więcej na temat obliczania kosztów używania gorącej lub chłodnej warstwy magazynowania, zobacz *„What are Hot and Cool access tiers and how should I determine which one to use?”* („Co to są warstwy dostępu Gorąca i Chłodna i jak określić, której użyć?”) w FAQ (Często zadawanych pytaniach) na stronie z [cennikiem usługi Azure Storage](https://azure.microsoft.com/pricing/details/storage/).
> 
> 

### <a name="migrating-existing-data"></a>Migrowanie istniejących danych
Konto usługi Blob Storage jest przeznaczone do przechowywania tylko blokowych obiektów blob i uzupełnialnych obiektów blob. Istniejące konta magazynu ogólnego przeznaczenia, które umożliwiają przechowywanie tabel, kolejek, plików, dysków, a także obiektów blob, nie mogą być konwertowane na konta usługi Blob Storage. Aby użyć warstw magazynowania, należy utworzyć nowe konta usługi Blob Storage i przeprowadzić migrację istniejących danych do nowo utworzonych kont.
Poniższych metod można użyć do migrowania istniejących danych do kont usługi Blob Storage z lokalnych urządzeń magazynujących, od innych dostawców magazynu w chmurze lub z istniejących kont magazynu ogólnego przeznaczenia na platformie Azure:

#### <a name="azcopy"></a>Narzędzie AzCopy
Narzędzie AzCopy to narzędzie wiersza polecenia systemu Windows przeznaczone do kopiowania z wysoką wydajnością danych z i do usługi Azure Storage. Narzędzia AzCopy można użyć do skopiowania danych na konto usługi Magazynu obiektów blob z istniejących kont magazynu ogólnego przeznaczenia lub do przekazania danych z lokalnych urządzeń magazynujących na konto Magazynu obiektów blob.

Aby uzyskać więcej informacji, zobacz [Transfer danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md).

#### <a name="data-movement-library"></a>Biblioteka przenoszenia danych
Biblioteka przenoszenia danych usługi Azure Storage dla programu .NET jest oparta na podstawowym środowisku przenoszenia danych, w którym działa narzędzie AzCopy. Biblioteka została zaprojektowana na potrzeby przeprowadzania wysokowydajnych, niezawodnych i prostych operacji transferu danych w sposób podobny do narzędzia AzCopy. Pozwala to w pełni wykorzystać funkcje oferowane przez narzędzie AzCopy natywnie w aplikacji bez konieczności uruchamiania i monitorowania zewnętrznych wystąpień narzędzia AzCopy.

Aby uzyskać więcej informacji, zobacz [Azure Storage Data Movement Library for .Net](https://github.com/Azure/azure-storage-net-data-movement) (Biblioteka przenoszenia danych usługi Magazyn Azure dla platformy .NET).

#### <a name="rest-api-or-client-library"></a>Interfejs API REST lub biblioteka klienta
Można utworzyć aplikację niestandardową służącej do migracji danych do konta Magazynu obiektów blob przy użyciu jednej z bibliotek klienta platformy Azure lub interfejsu API REST usług magazynu platformy Azure. Usługa Azure Storage udostępnia rozbudowane biblioteki dla wielu języków programowania i platform, takich jak .NET, Java, C++, Node.JS, PHP, Ruby i Python. Biblioteki klienta oferują zaawansowane możliwości, takie jak logika ponowień, rejestrowanie i przekazywanie równoległe. Możliwe jest również programowanie bezpośrednio przy użyciu interfejsu API REST, który może być wywoływany przez dowolny język programowania mający możliwość wysyłania żądań HTTP lub HTTPS.

Aby uzyskać więcej szczegółów, zobacz [Rozpoczynanie pracy z Magazynem obiektów blob Azure](storage-dotnet-how-to-use-blobs.md).

> [!NOTE]
> Obiekty blob zaszyfrowane za pomocą szyfrowania po stronie klienta przechowują metadane związane z szyfrowaniem przechowywane w ramach obiektu blob. Jest absolutnie krytyczne, aby każdy mechanizm kopiowania zapewnił, że metadane obiektu blob, a w szczególności metadane związane z szyfrowaniem, zostały zachowane. Jeśli obiekty blob zostaną skopiowane bez takich metadanych, nie będzie można ponownie pobrać zawartości obiektu blob. Aby uzyskać więcej informacji na temat metadanych związanych z szyfrowaniem, zobacz [Azure Storage Client-Side Encryption](storage-client-side-encryption.md) (Szyfrowanie po stronie klienta usługi Azure Storage).
> 
> 

## <a name="faqs"></a>Często zadawane pytania
1. **Czy istniejące konta magazynu są nadal dostępne?**
   
    Tak, istniejące konta magazynu są nadal dostępne, a ich ceny i funkcje nie zostały zmienione.  W przypadku takich kont nie ma możliwości wyboru warstwy magazynowania, a funkcje obsługi warstw nie zostaną wprowadzone w przyszłości.
2. **Kiedy i dlaczego należy rozpocząć używanie kont usługi Blob Storage?**
   
    Konta Magazynu obiektów blob są przeznaczone do przechowywania obiektów blob i umożliwiają wprowadzenie nowych funkcji związanych z obiektami blob. Idąc dalej, konta Magazynu obiektów blob to zalecany sposób przechowywania obiektów blob, ponieważ przyszłe możliwości, takie jak magazyn hierarchiczny czy obsługa warstw, zostaną wprowadzone na podstawie tego typu konta. Decyzja o migracji należy jednak do użytkownika i zależy od jego potrzeb biznesowych.
3. **Czy mogę przekonwertować istniejące konto magazynu na konto usługi Blob Storage?**
   
    Nie. Konto usługi Blob Storage to inny typ konta magazynu i należy utworzyć je na nowo, a następnie przeprowadzić migrację danych w sposób opisany powyżej.
4. **Czy mogę przechowywać obiekty w obu warstwach magazynowania w ramach tego samego konta?**
   
    Atrybut *Warstwa dostępu*, który określa warstwę magazynowania, jest ustawiony na poziomie konta i dotyczy wszystkich obiektów w ramach tego konta. Nie można ustawić atrybutu warstwy dostępu na poziomie obiektu.
5. **Czy mogę zmienić warstwę magazynowania na moim koncie usługi Blob Storage?**
   
    Tak. Zmiana warstwy magazynowania jest możliwa po ustawieniu atrybutu *Warstwa dostępu* na koncie magazynu. Zmiana warstwy magazynowania będzie miała zastosowanie do wszystkich obiektów przechowywanych na koncie. Zmiana warstwy magazynowania z gorącej na chłodną nie spowoduje naliczenia żadnych opłat, ale zmiana warstwy magazynowania z chłodnej na gorącą spowoduje naliczenie opłaty za każdy gigabajt związany z odczytem wszystkich danych w ramach konta.
6. **Jak często mogę zmieniać warstwę magazynowania na moim koncie usługi Blob Storage?**
   
    Nie wymuszamy limitu częstotliwości zmiany warstwy magazynowania, ale należy pamiętać, że zmiana warstwy magazynowania z chłodnej na gorącą spowoduje naliczenie znaczących opłat. Nie zaleca się częstych zmian warstwy magazynowania.
7. **Czy obiekty blob w chłodniej warstwie magazynowania będą działały inaczej niż obiekty blob w gorącej warstwie magazynowania?**
   
    Obiekty blob w gorącej warstwie magazynowania mają takie samo opóźnienie jak obiekty blob na kontach magazynu ogólnego przeznaczenia. Obiekty blob w chłodniej warstwie magazynowania mają podobne opóźnienie (w milisekundach) jak obiekty blob na kontach magazynu ogólnego przeznaczenia.
   
    Obiekty blob w chłodniej warstwie magazynowania będą miały nieco niższy poziom dostępności usług (umowa SLA) niż obiekty blob przechowywane w gorącej warstwie magazynowania. Aby uzyskać więcej szczegółów, zobacz [Magazyn — umowa SLA](https://azure.microsoft.com/support/legal/sla/storage).
8. **Czy mogę przechowywać stronicowe obiekty i dyski maszyny wirtualnej na kontach usługi Blob Storage?**
   
    Konta Magazynu obiektów blob obsługują tylko blokowe obiekty blob i uzupełnialne obiekty blob — stronicowe obiekty blob nie są obsługiwane. Dyski maszyny wirtualnej platformy Azure są oparte na stronicowych obiektach blob, w związku z czym konta Magazynu obiektów blob nie mogą być używane do przechowywania dysków maszyny wirtualnej. Istnieje jednak możliwość przechowywania kopii zapasowych dysków maszyny wirtualnej jako blokowych obiektów blob na kontach Magazynu obiektów blob.
9. **Czy muszę zmienić swoje istniejące aplikacje, aby umożliwić korzystanie z kont usługi Blob Storage?**
   
    Konta Magazynu obiektów blob są w pełni spójne pod względem interfejsu API z kontami magazynu ogólnego przeznaczenia dla blokowych obiektów blob i uzupełnialnych obiektów blob. Twoja aplikacja powinna działać, dopóki korzysta z blokowych obiektów blob lub uzupełnialnych obiektów blob i używasz wersji 2014-02-14 lub nowszej [interfejsu API REST usług Storage](https://msdn.microsoft.com/library/azure/dd894041.aspx). Jeśli używasz starszej wersji protokołu, musisz zaktualizować aplikację do nowej wersji, tak aby bezproblemowo współpracowała z oboma typami kont magazynu. Ogólnie zawsze zalecamy używanie najnowszej wersji niezależnie od używanego typu konta magazynu.
10. **Czy w środowisku użytkownika zostaną wprowadzone zmiany?**
    
    Konta usługi Blob Storage są bardzo podobne do kont magazynu ogólnego przeznaczenia służących do przechowywania blokowych obiektów blob oraz uzupełnialnych obiektów blob i obsługują wszystkie kluczowe funkcje usługi Azure Storage, w tym funkcje związane z wysoką trwałością i dostępnością, skalowalnością, wydajnością i zabezpieczeniami. Wszystkie inne elementy niż opisane powyżej funkcje oraz ograniczenia dotyczące kont usługi Blob Storage i ich warstw magazynowania pozostają takie same.

## <a name="next-steps"></a>Następne kroki
### <a name="evaluate-blob-storage-accounts"></a>Ocena konta Magazynu obiektów blob
[Sprawdzanie dostępności kont usługi Blob Storage według regionu](https://azure.microsoft.com/regions/#services)

[Ocena użycia bieżących kont magazynu przez włączenie metryk usługi Azure Storage](storage-enable-and-view-metrics.md)

[Sprawdzanie cen usługi Blob Storage według regionu](https://azure.microsoft.com/pricing/details/storage/)

[Sprawdzanie ceny transferu danych](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>Rozpoczynanie korzystania z kont Magazynu obiektów blob
[Rozpoczynanie pracy z usługą Azure Blob Storage przy użyciu platformy .NET](storage-dotnet-how-to-use-blobs.md)

[Przenoszenie danych do i z usługi Azure Storage](storage-moving-data.md)

[Transfer danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md)

[Przeglądaj i eksploruj konta magazynu](http://storageexplorer.com/)




<!--HONumber=Dec16_HO5-->


