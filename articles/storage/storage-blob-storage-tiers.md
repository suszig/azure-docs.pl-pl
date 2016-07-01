<properties
    pageTitle="Chłodna usługa Storage platformy Azure dla obiektów blob | Microsoft Azure"
    description="Warstwy usługi Storage dla usługi Blob Storage oferują ekonomiczny sposób magazynowania danych obiektów w oparciu o wzorce dostępu. Chłodny magazyn Azure jest zoptymalizowany pod kątem danych, do których rzadziej uzyskuje się dostęp."
    services="storage"
    documentationCenter=""
    authors="sribhat-msft"
    manager=""
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/09/2016"
    ms.author="sribhat"/>


# Azure Blob Storage: warstwy magazynu gorącego i chłodnego

## Omówienie

Usługa Azure Storage oferuje teraz dwie warstwy magazynowania dla magazynu obiektów blob, dzięki czemu możliwe jest najbardziej ekonomiczne przechowywanie danych w zależności od sposobu ich używania. **Warstwa magazynu gorącego** platformy Azure została zoptymalizowana pod kątem przechowywania danych, do których często uzyskuje się dostęp. **Warstwa magazynu chłodnego** platformy Azure została zoptymalizowana pod kątem przechowywania danych, do których rzadko uzyskuje się dostęp i które mają długi okres życia. Dla danych w warstwie magazynu chłodnego nie ma znaczenia nieco niższa dostępność, ale nadal wymagana jest wysoka trwałość oraz podobny czas dostępu i parametry przepływności jak w przypadku gorących danych. W przypadku chłodnych danych umowa SLA dotycząca nieco niższej dostępności i wyższe koszty dostępu to dopuszczalne wady, biorąc pod uwagę znacznie niższe koszty magazynowania.

Obecnie ilość danych przechowywanych w chmurze rośnie w tempie wykładniczym. Aby zarządzać kosztami zwiększających się potrzeb dotyczących magazynowania, warto zorganizować dane na podstawie atrybutów, takich jak częstotliwość dostępu do danych i planowany okres przechowywania. Dane przechowywane w chmurze mogą być zupełnie różne pod względem sposobu ich generowania, przetwarzania i uzyskiwania do nich dostępu przez cały okres ich istnienia. Do niektórych danych często uzyskuje się dostęp. Są one również często modyfikowane w trakcie całego okresu istnienia. Do niektórych danych często uzyskuje się dostęp na początkowym etapie istnienia, a z czasem już zdecydowanie rzadziej. Niektóre dane pozostają nieużywane w chmurze i dostęp do nich uzyskuje się rzadko (lub w ogóle) po umieszczeniu ich w magazynie.

Dla każdego z opisanych powyżej scenariuszy dostępu do danych istnieją korzyści ze zróżnicowanych warstw magazynowania zoptymalizowanych pod kątem określonego wzorca dostępu. Wraz z wprowadzeniem warstw dostępu do magazynu gorącego i chłodnego Magazyn obiektów blob Azure zaspokaja tę potrzebę posiadania zróżnicowanych warstw magazynowania z oddzielnymi modelami cenowymi.

## Konta Magazynu obiektów blob

**Konta usługi Blob Storage** to specjalne konta magazynu służące do przechowywania danych bez struktury jako obiekty blob (obiekty) w usłudze Azure Storage. Za pomocą kont Magazynu obiektów blob można teraz wybierać między warstwą dostępu do magazynu chłodnego a warstwą dostępu do magazynu ciepłego w celu przechowywania rzadziej używanych chłodnych danych z niższym kosztem magazynowania i częściej używanych gorących danych z niższym kosztem dostępu. Konta Magazynu obiektów blob są podobne do istniejących kont magazynu ogólnego przeznaczenia i udostępniają wszystkie używane obecnie funkcje doskonałej trwałości, dostępności, skalowalności i wydajności, łącznie z pełną spójnością interfejsu API na potrzeby blokowych obiektów blob i uzupełnialnych obiektów blob.

> [AZURE.NOTE] Konta Magazynu obiektów blob obsługują tylko blokowe obiekty blob i uzupełnialne obiekty blob — stronicowe obiekty blob nie są obsługiwane.

Konta Magazynu obiektów blob udostępniają atrybut **Warstwa dostępu**, który umożliwia określenie warstwy magazynowania jako **Gorąca** lub **Chłodna** w zależności od danych przechowywanych w ramach konta. W przypadku zmiany wzorca użycia danych można także w dowolnym momencie przełączyć się między tymi warstwami dostępu.

> [AZURE.NOTE] Zmiana warstwy dostępu może spowodować naliczenie dodatkowych opłat. Więcej szczegółów zawiera sekcja dotycząca [cennika i rozliczeń](storage-blob-storage-tiers.md#pricing-and-billing).

Przykładowe scenariusze użycia dotyczące warstwy dostępu do magazynu gorącego obejmują:

- Dane, które są często używane lub przewiduje się do nich częsty dostęp (odczyt i zapis danych).
- Dane, które są przygotowywane do przetwarzania i ewentualnej migracji do warstwy magazynu chłodnego.

Przykładowe scenariusze użycia dotyczące warstwy dostępu do magazynu chłodnego obejmują:

- Zestawy danych usługi Backup, archiwizacji i odzyskiwania po awarii.
- Starszą zawartość nośników, która nie jest już często wyświetlana, ale oczekiwane jest, że będzie ona natychmiast dostępna, gdy będzie to wymagane.
- Duże zbiory danych, które muszą być przechowywane w sposób ekonomiczny, podczas gdy na potrzeby przyszłego przetwarzania zbierana jest większa ilość danych (*np.* długoterminowe magazynowanie danych naukowych lub nieprzetworzonych danych telemetrycznych z zakładu produkcyjnego).
- Oryginalne (nieprzetworzone) dane, które muszą zostać zachowane, nawet po przetworzeniu ich do ostatecznej użytecznej postaci (*np.* nieprzetworzone pliki multimedialne po transkodowaniu do innych formatów).
- Dane zgodności i dane archiwalne, które muszą być przechowywane przez długi czas, i do których bardzo rzadko uzyskuje się dostęp (*np.* zapisy z kamer monitorujących, stare zdjęcia rentgenowskie lub zdjęcia z rezonansu magnetycznego dla organizacji opieki zdrowotnej, nagrania audio i zapisy rozmów telefonicznych z klientami dla firm z branży usług finansowych).

Aby uzyskać więcej informacji dotyczących kont magazynu, zobacz [Informacje o kontach magazynu Azure](storage-create-storage-account.md).

Dla aplikacji wymagających tylko magazynu blokowych obiektów blob lub magazynu uzupełnialnych obiektów blob zaleca się użycie kont Magazynu obiektów blob, aby móc korzystać ze zróżnicowanego modelu cenowego magazynu warstwowego. Zdajemy sobie jednak sprawę z tego, że może to nie być możliwe w pewnych okolicznościach, gdy najlepszym rozwiązaniem jest użycie kont magazynu ogólnego przeznaczenia. Przykład:

- Musisz korzystać z tabel, kolejek lub plików, a obiekty blob mają być przechowywane na tym samym koncie magazynu. Należy pamiętać, że przechowywanie takich elementów w ramach tego samego konta nie przynosi żadnej korzyści technicznej oprócz posiadania takich samych udostępnionych kluczy.
- Nadal musisz korzystać z klasycznego modelu wdrożenia. Konta Magazynu obiektów blob są dostępne tylko za pośrednictwem modelu wdrażania przy użyciu usługi Azure Resource Manager.
- Musisz korzystać ze stronicowych obiektów blob. Konta Magazynu obiektów blob nie obsługują stronicowych obiektów blob. Ogólnie zaleca się używanie blokowych obiektów blob, chyba że istnieje konkretna potrzeba użycia stronicowych obiektów blob.
- Używasz wersji [interfejsu API REST usług Storage](https://msdn.microsoft.com/library/azure/dd894041.aspx) wcześniejsza niż 2014-02-14 lub biblioteka klienta w wersji wcześniejszej niż 4.x i nie można uaktualnić aplikacji.

> [AZURE.NOTE] Konta Magazynu obiektów blob są obecnie obsługiwane w większości regionów platformy Azure i mają być obsługiwane w kolejnych regionach. Zaktualizowaną listę dostępnych regionów możesz znaleźć na stronie [regionów platformy Azure](https://azure.microsoft.com/regions/#services).

## Porównanie warstw dostępu

W poniższej tabeli znajduje się porównanie dwóch warstw dostępu:

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250">
<col width="250">
<col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>Warstwa dostępu do magazynu gorącego</center></strong></td>
    <td><strong><center>Warstwa dostępu do magazynu chłodnego</center></strong></td
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

> [AZURE.NOTE] Konta Magazynu obiektów blob obsługują te same cele wydajności i skalowalności co konta magazynu ogólnego przeznaczenia. Aby uzyskać więcej informacji, zobacz [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) (Cele dotyczące skalowalności i wydajności usługi Magazyn Azure).

## Cennik i rozliczenia

Konta Magazynu obiektów blob używają nowego modelu cenowego opartego na warstwie dostępu. W przypadku korzystania z konta Magazynu obiektów blob mają zastosowanie następujące zagadnienia dotyczące rozliczeń:

- **Koszty usługi Storage**: oprócz ilości przechowywanych danych koszt przechowywania danych różni się w zależności od warstwy dostępu. Koszt za każdy gigabajt jest niższy dla warstwy dostępu do magazynu chłodnego niż dla warstwy dostępu do magazynu gorącego.
- **Koszty dostępu do danych**: w przypadku danych w warstwie dostępu do magazynu chłodnego zostanie naliczona opłata dostępu do danych za każdy gigabajt dla operacji odczytu i zapisu.
- **Koszty transakcji**: dla obu warstw istnieje opłata za każdą transakcję. Jednak koszt każdej transakcji dla warstwy dostępu do magazynu chłodnego jest wyższy niż w przypadku warstwy dostępu do magazynu gorącego.
- **Koszty transferu danych replikacji geograficznej**: dotyczy to tylko kont ze skonfigurowaną replikacją geograficzną, w tym GRS i RA-GRS. Transfer danych w ramach replikacji geograficznej powoduje naliczanie opłaty za każdy gigabajt.
- **Koszty transferu danych wychodzących**: transfery danych wychodzących (dane przesyłane poza region platformy Azure) powodują naliczanie opłat za zużycie przepustowości za każdy gigabajt, co jest spójne z kontami magazynu ogólnego przeznaczenia.
- **Zmiana warstwy dostępu**: zmiana warstwy dostępu z chłodnej na gorącą spowoduje naliczenie opłaty równej opłacie za odczytanie wszystkich danych z konta magazynu dla każdego przejścia. Z drugiej strony zmiana warstwy dostępu z gorącej na chłodną będzie bezpłatna.

> [AZURE.NOTE] Aby umożliwić użytkownikom wypróbowanie nowych warstw magazynowania i zweryfikowanie funkcji po jej udostępnieniu, opłata za zmianę warstwy dostępu z chłodnej na gorącą nie będzie naliczana do 30 czerwca 2016 r. Od 1 lipca 2016 r. opłata będzie naliczana dla wszystkich przejść z warstwy chłodnej do gorącej. Dodatkowe szczegóły dotyczące modelu cenowego dla kont usługi Blob Storage można znaleźć na stronie [Cennik usługi Azure Storage](https://azure.microsoft.com/pricing/details/storage/). Dodatkowe szczegóły dotyczące opłat za transfer danych wychodzących można znaleźć na stronie [Szczegóły cennika transferów danych](https://azure.microsoft.com/pricing/details/data-transfers/).

## Szybki start

W tej sekcji zostaną przedstawione następujące scenariusze obejmujące użycie Portalu Azure:

- Tworzenie konta Magazynu obiektów blob.
- Zarządzanie kontem Magazynu obiektów blob.

### Korzystanie z Portalu Azure

#### Tworzenie konta Magazynu obiektów blob za pośrednictwem Portalu Azure

1. Zaloguj się do [Portalu Azure](https://portal.azure.com).

2. W menu Centrum wybierz kolejno pozycje **Nowe** -> **Dane i usługa Storage** -> **Konto usługi Storage**.

3. Wprowadź nazwę konta magazynu.

4. Wybierz pozycję **Resource Manager** jako model wdrażania.

5. Wybierz pozycję **Magazyn obiektów blob** jako typ konta magazynu.

6. Wybierz warstwę dostępu: **Gorąca** lub **Chłodna**. Wartość domyślna to **Gorąca**.

7. Wybierz opcję replikacji dla konta magazynu: **LRS**, **GRS** lub **RA-GRS**. Wartość domyślna to **RA-GRS**. Aby uzyskać więcej szczegółów na temat opcji replikacji usługi Azure Storage, zobacz [Replikacja usługi Azure Storage](storage-redundancy.md).

8. Wybierz subskrypcję, w ramach której chcesz utworzyć nowe konto magazynu.

9. Określ nową grupę zasobów lub wybierz istniejącą grupę zasobów. Aby uzyskać więcej informacji dotyczących grup zasobów, zobacz [Korzystanie z Portalu Azure do zarządzania zasobami Azure](../azure-portal/resource-group-portal.md).

10. Wybierz lokalizację geograficzną dla swojego konta magazynu.

11. Kliknij pozycję **Utwórz**, aby utworzyć konto magazynu.

#### Zmienianie warstwy dostępu konta Magazynu obiektów blob za pośrednictwem Portalu Azure

1. Zaloguj się do [Portalu Azure](https://portal.azure.com) i przejdź do konta magazynu.

2. Kliknij pozycję **Wszystkie ustawienia**, a następnie kliknij pozycję **Konfiguracja**, aby wyświetlić i/lub zmienić konfigurację konta.

3. Określ żądaną warstwę dostępu: **Gorąca** lub **Chłodna**.

    > [AZURE.NOTE] Zmiana warstwy dostępu może spowodować naliczenie dodatkowych opłat. Więcej szczegółów zawiera sekcja dotycząca [cennika i rozliczeń](storage-blob-storage-tiers.md#pricing-and-billing).

## Migrowanie do kont Magazynu obiektów blob

Informacje przedstawione w tej sekcji mają ułatwić użytkownikom płynne przejście do kont Magazynu obiektów blob. Konto Magazynu obiektów blob jest przeznaczone do przechowywania tylko blokowych obiektów blob i uzupełnialnych obiektów blob. Istniejących kont magazynu ogólnego przeznaczenia, które umożliwiają przechowywanie tabel, kolejek, plików, dysków, a także obiektów blob, nie można konwertować na konta Magazynu obiektów blob. Aby użyć warstw magazynowania, należy utworzyć nowe konta Magazynu obiektów blob i przeprowadzić migrację istniejących danych do nowo utworzonych kont.

### Planowanie migracji istniejących danych

Jeśli przenosisz dane na konto Magazynu obiektów blob, prawdopodobnie chcesz skorzystać z warstwy dostępu do magazynu chłodnego w celu obniżenia kosztów magazynowania rzadziej używanych danych. Pierwszym krokiem podczas planowania migracji danych znajdujących się na koncie Magazynu obiektów blob w warstwie dostępu do magazynu chłodnego jest ocena istniejącego wzorca użycia w celu określenia, czy migracja do konta Magazynu obiektów blob będzie korzystna. Ogólnie potrzebne są odpowiedzi na następujące pytania:

- Wzorzec wykorzystania magazynu — jaka ilość danych jest magazynowana i jak ta ilość zmienia się w ciągu miesiąca?
- Wzorce dostępu do magazynu — jak dużo danych jest odczytywanych z konta i zapisywanych na nim (w tym nowych danych)? Ile transakcji i które z nich są używane do uzyskiwania dostępu do danych?

W celu monitorowania istniejących kont magazynu i zbierania tych danych zobacz [Enabling Azure Storage metrics and viewing metrics data](storage-enable-and-view-metrics.md) (Włączanie metryk usługi Magazyn Azure i wyświetlanie danych metryk). Teraz przy użyciu tych danych możesz użyć [Kalkulatora cen usługi Azure Storage](https://azure.microsoft.com/pricing/calculator/?scenario=data-management) w celu ułatwienia szacowania kosztów.

### Migrowanie istniejących danych

Poniższych metod można użyć do migrowania istniejących danych do kont Magazynu obiektów blob z lokalnych urządzeń magazynujących, od innych dostawców magazynu w chmurze lub z istniejących kont magazynu ogólnego przeznaczenia na platformie Azure:

#### Narzędzie AzCopy

Narzędzie AzCopy to narzędzie wiersza polecenia systemu Windows przeznaczone do kopiowania z wysoką wydajnością danych z i do usługi Azure Storage. Narzędzia AzCopy można użyć do skopiowania danych na konto usługi Magazynu obiektów blob z istniejących kont magazynu ogólnego przeznaczenia lub do przekazania danych z lokalnych urządzeń magazynujących na konto Magazynu obiektów blob.

Aby uzyskać więcej informacji, zobacz [Transfer danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md).

#### Biblioteka przenoszenia danych

Biblioteka przenoszenia danych usługi Azure Storage dla programu .NET jest oparta na podstawowym środowisku przenoszenia danych, w którym działa narzędzie AzCopy. Biblioteka została zaprojektowana na potrzeby przeprowadzania wysokowydajnych, niezawodnych i prostych operacji transferu danych w sposób podobny do narzędzia AzCopy. Pozwala to w pełni wykorzystać funkcje oferowane przez narzędzie AzCopy natywnie w aplikacji bez konieczności uruchamiania i monitorowania zewnętrznych wystąpień narzędzia AzCopy.

Aby uzyskać więcej informacji, zobacz [Azure Storage Data Movement Library for .Net](https://github.com/Azure/azure-storage-net-data-movement) (Biblioteka przenoszenia danych usługi Magazyn Azure dla platformy .NET).

#### Interfejs API REST lub biblioteka klienta

Można utworzyć aplikację niestandardową służącej do migracji danych do konta Magazynu obiektów blob przy użyciu jednej z bibliotek klienta platformy Azure lub interfejsu API REST usług magazynu platformy Azure. Usługa Azure Storage udostępnia rozbudowane biblioteki dla wielu języków programowania i platform, takich jak .NET, Java, C++, Node.JS, PHP, Ruby i Python. Biblioteki klienta oferują zaawansowane możliwości, takie jak logika ponowień, rejestrowanie i przekazywanie równoległe. Możliwe jest również programowanie bezpośrednio przy użyciu interfejsu API REST, który może być wywoływany przez dowolny język programowania mający możliwość wysyłania żądań HTTP lub HTTPS.

Aby uzyskać więcej szczegółów, zobacz [Rozpoczynanie pracy z Magazynem obiektów blob Azure](storage-dotnet-how-to-use-blobs.md).

> [AZURE.NOTE] Obiekty blob zaszyfrowane za pomocą szyfrowania po stronie klienta przechowują metadane związane z szyfrowaniem przechowywane w ramach obiektu blob. Jest absolutnie krytyczne, aby każdy mechanizm kopiowania zapewnił, że metadane obiektu blob, a w szczególności metadane związane z szyfrowaniem, zostały zachowane. Jeśli obiekty blob zostaną skopiowane bez takich metadanych, nie będzie można ponownie pobrać zawartości obiektu blob. Aby uzyskać więcej informacji dotyczących metadanych związanych z szyfrowaniem, zobacz [Azure Storage client side encryption](storage-client-side-encryption.md) (Szyfrowanie po stronie klienta usługi Magazyn Azure).

## Często zadawane pytania

1. **Czy istniejące konta magazynu są nadal dostępne?**

    Tak, istniejące konta magazynu są nadal dostępne, a ich ceny i funkcje nie zostały zmienione.  W przypadku takich kont nie ma możliwości wyboru warstwy dostępu, a funkcje obsługi warstw nie zostaną wprowadzone w przyszłości.

2. **Kiedy i dlaczego należy rozpocząć używanie kont Magazynu obiektów blob?**

    Konta Magazynu obiektów blob są przeznaczone do przechowywania obiektów blob i umożliwiają wprowadzenie nowych funkcji związanych z obiektami blob. Idąc dalej, konta Magazynu obiektów blob to zalecany sposób przechowywania obiektów blob, ponieważ przyszłe możliwości, takie jak magazyn hierarchiczny czy obsługa warstw, zostaną wprowadzone na podstawie tego typu konta. Decyzja o migracji należy jednak do użytkownika i zależy od jego potrzeb biznesowych.

3. **Czy mogę przekonwertować istniejące konto magazynu na konto Magazynu obiektów blob?**

    Nie. Konto Magazynu obiektów blob to inny typ konta magazynu i należy utworzyć je na nowo, a następnie przeprowadzić migrację danych w sposób opisany powyżej.

4. **Czy mogę przechowywać obiekty w obu warstwach dostępu w ramach tego samego konta?**

    Atrybut warstwy dostępu jest ustawiany na poziomie konta i dotyczy wszystkich obiektów w ramach tego konta. Nie można ustawić warstwy dostępu na poziomie obiektu.

5. **Czy mogę zmienić warstwę dostępu na moim koncie Magazynu obiektów blob?**

    Tak. Zmiana warstwy dostępu na koncie magazynu jest możliwa. Zmiana warstwy dostępu na poziomie konta będzie miała zastosowanie do wszystkich obiektów przechowywanych w ramach konta. Zmiana warstwy dostępu z gorącej na chłodną nie spowoduje naliczenia żadnych opłat, ale zmiana warstwy dostępu z chłodnej na gorącą spowoduje naliczenie opłaty za każdy gigabajt związany z odczytem wszystkich danych w ramach konta.

6. **Jak często mogę zmieniać warstwę dostępu na moim koncie Magazynu obiektów blob?**

    Nie wymuszamy limitu częstotliwości zmiany warstwy dostępu, ale należy pamiętać, że zmiana warstwy dostępu z chłodnej na gorącą spowoduje naliczenie znaczących opłat. Nie zaleca się częstych zmian warstwy dostępu.

7. **Czy obiekty blob w warstwie dostępu do magazynu chłodnego będą działały inaczej niż obiekty blob w warstwie dostępu do magazynu gorącego?**

    Obiekty blob w warstwie dostępu do magazynu gorącego mają takie samo opóźnienie jak obiekty blob na kontach magazynu ogólnego przeznaczenia. Obiekty blob w warstwie dostępu do magazynu chłodnego mają podobne opóźnienie (w milisekundach) jak obiekty blob na kontach magazynu ogólnego przeznaczenia.

    Obiekty blob w warstwie dostępu do magazynu chłodnego będą miały nieco niższy poziom dostępności usług (umowa SLA) niż obiekty blob przechowywane w warstwie dostępu do magazynu gorącego. Aby uzyskać więcej szczegółów, zobacz [Magazyn — umowa SLA](https://azure.microsoft.com/support/legal/sla/storage).

8. **Czy mogę przechowywać stronicowe obiekty i dyski maszyny wirtualnej na kontach Magazynu obiektów blob?**

    Konta Magazynu obiektów blob obsługują tylko blokowe obiekty blob i uzupełnialne obiekty blob — stronicowe obiekty blob nie są obsługiwane. Dyski maszyny wirtualnej platformy Azure są oparte na stronicowych obiektach blob, w związku z czym konta Magazynu obiektów blob nie mogą być używane do przechowywania dysków maszyny wirtualnej. Istnieje jednak możliwość przechowywania kopii zapasowych dysków maszyny wirtualnej jako blokowych obiektów blob na kontach Magazynu obiektów blob.

9. **Czy muszę zmienić swoje istniejące aplikacje, aby umożliwić korzystanie z kont Magazynu obiektów blob?**

    Konta Magazynu obiektów blob są w pełni spójne pod względem interfejsu API z kontami magazynu ogólnego przeznaczenia dla blokowych obiektów blob i uzupełnialnych obiektów blob. Twoja aplikacja powinna działać, dopóki korzysta z blokowych obiektów blob lub uzupełnialnych obiektów blob i używasz wersji 2014-02-14 lub nowszej [interfejsu API REST usług Storage](https://msdn.microsoft.com/library/azure/dd894041.aspx). Jeśli używasz starszej wersji protokołu, musisz zaktualizować aplikację do nowej wersji, tak aby bezproblemowo współpracowała z oboma typami kont magazynu. Ogólnie zawsze zalecamy używanie najnowszej wersji niezależnie od używanego typu konta magazynu.

10. **Czy w środowisku użytkownika zostaną wprowadzone zmiany?**

    Konta usługi Blob Storage są bardzo podobne do kont magazynu ogólnego przeznaczenia służących do przechowywania blokowych obiektów blob oraz uzupełnialnych obiektów blob i obsługują wszystkie kluczowe funkcje usługi Azure Storage, w tym funkcje związane z wysoką trwałością i dostępnością, skalowalnością, wydajnością i zabezpieczeniami. Wszystkie inne elementy niż opisane powyżej funkcje oraz ograniczenia dotyczące kont Magazynu obiektów blob i ich warstw dostępu pozostają takie same.

## Następne kroki

### Ocena konta Magazynu obiektów blob

[Sprawdzanie dostępności kont Magazynu obiektów blob według regionu](https://azure.microsoft.com/regions/#services)

[Ocena użycia bieżących kont magazynu przez włączenie metryk usługi Azure Storage](storage-enable-and-view-metrics.md)

[Sprawdzanie cen Magazynu obiektów blob według regionu](https://azure.microsoft.com/pricing/details/storage/)

[Sprawdzanie ceny transferu danych](https://azure.microsoft.com/pricing/details/data-transfers/)

### Rozpoczynanie korzystania z kont Magazynu obiektów blob

[Rozpoczynanie pracy z Magazynem obiektów blob Azure](storage-dotnet-how-to-use-blobs.md)

[Przenoszenie danych do i z usługi Azure Storage](storage-moving-data.md)

[Transfer danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md)



<!--HONumber=Jun16_HO2-->


