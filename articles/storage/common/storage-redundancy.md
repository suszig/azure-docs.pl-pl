---
title: "Replikacja danych w usłudze Azure Storage | Dokumentacja firmy Microsoft"
description: "Dane na koncie usługi Magazyn Microsoft Azure jest replikowana na potrzeby trwałości i wysokiej dostępności. Opcje replikacji obejmują magazyn lokalnie nadmiarowy (LRS), Magazyn strefowo nadmiarowy (ZRS) magazynu geograficznie nadmiarowego (GRS) i dostęp do odczytu magazynu geograficznie nadmiarowego (RA-GRS)."
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.workload: storage
ms.topic: article
ms.date: 01/21/2018
ms.author: tamram
ms.openlocfilehash: 18d0e8bc6cc1559f9ae1a1a4457aa85d2a206597
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/29/2018
---
# <a name="azure-storage-replication"></a>Replikacja usługi Azure Storage

Dane konta usługi Microsoft Azure Storage są zawsze replikowane w celu zapewnienia trwałości i wysokiej dostępności. Replikacja kopiuje dane tak, że plik jest chroniony z przejściowych awarii sprzętu, zachowując Twojej aplikacji czasu. 

Można replikować dane w tym samym centrum danych między centrami danych w tym samym regionie lub w regionach. Dane są replikowane w wielu centrach danych lub w regionach, są również chronione poważnej awarii w jednym miejscu.

Replikacja gwarantuje, że Twoje konto magazynu spełnia warunki [Umowy dotyczącej poziomu usług (SLA) dla Magazynu](https://azure.microsoft.com/support/legal/sla/storage/) nawet w przypadku wystąpienia błędów. Zobacz umowę SLA, aby uzyskać informacje o gwarancjach usługi Azure Storage dotyczących trwałości i dostępności.

Podczas tworzenia konta magazynu można wybrać jedną z następujących opcji replikacji:

* [Magazyn lokalnie nadmiarowy (LRS)](#locally-redundant-storage)
* [Magazyn strefowo nadmiarowy (ZRS)](#zone-redundant-storage)
* [Magazyn geograficznie nadmiarowy (GRS)](#geo-redundant-storage)
* [Magazyn geograficznie nadmiarowy dostępny do odczytu (RA-GRS)](#read-access-geo-redundant-storage)

Dostęp do odczytu magazynu geograficznie nadmiarowego (RA-GRS) jest domyślną opcją podczas tworzenia konta magazynu.

Poniższa tabela zapewnia szybki przegląd różnice między LRS, ZRS, GRS i RA-GRS. Kolejne sekcje w tym artykule dotyczą każdego typu replikacji bardziej szczegółowo.

| Strategia replikacji | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Dane są replikowane w wielu centrach danych. |Nie |Yes |Yes |Yes |
| Można odczytać danych z lokalizacji dodatkowej, a także lokalizacji głównej. |Nie |Nie |Nie |Yes |
| Zaprojektowana w celu zapewnienia trwałości ___ obiektów w danym roku. |co najmniej 99.999999999% (11 na 9)|co najmniej 99.9999999999% (12 na 9)|co najmniej 99.99999999999999% (16 na 9)|co najmniej 99.99999999999999% (16 na 9)|

Zobacz [cennik usługi Azure Storage](https://azure.microsoft.com/pricing/details/storage/) dla informacji o cenach dla opcji różnych nadmiarowości.

> [!NOTE]
> Magazyn w warstwie Premium obsługuje magazyn tylko lokalnie nadmiarowy (LRS). Aby uzyskać informacje o magazynie Premium, zobacz [magazyn w warstwie Premium: magazyn o wysokiej wydajności dla obciążeń maszyny wirtualnej Azure](../../virtual-machines/windows/premium-storage.md).
>

## <a name="locally-redundant-storage"></a>Magazyn lokalnie nadmiarowy
[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

## <a name="zone-redundant-storage"></a>Strefa magazynu geograficznie nadmiarowego
[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-ZRS.md)]

### <a name="zrs-classic-accounts"></a>Magazyn ZRS klasycznych kont

Istniejące możliwości ZRS jest teraz nazywany ZRS klasycznego. Magazyn ZRS klasycznych kont są dostępne tylko dla blokowych obiektów blob na kontach magazynu ogólnego przeznaczenia, V1. 

Magazyn ZRS klasycznego replikuje dane asynchronicznie centrów w ramach jednej do dwóch regionach. Replika nie mogą być dostępne, o ile nie pomocniczym w tryb failover inicjuje firmy Microsoft. 

Nie można przekonwertować ZRS klasycznych kont, do lub z LRS, GRS lub RA-GRS. Magazyn ZRS klasycznych kont nie obsługują również metryki lub rejestrowania.   

Po ZRS jest ogólnie dostępna w regionie, nie można utworzyć konto klasycznego ZRS z portalu w danym regionie, ale można go utworzyć za pomocą innych środków.  
Proces automatycznej migracji z klasycznym ZRS ZRS zapewnia się w przyszłości.

Dane konta ZRS można migrować ręcznie do lub z konta LRS, ZRS klasycznego, GRS lub RAGRS. Można wykonać tej ręcznej migracji za pomocą narzędzia AzCopy, Eksploratora usługi Storage platformy Azure, programu Azure PowerShell, interfejsu wiersza polecenia Azure lub jeden z biblioteki klienta magazynu Azure.

> [!NOTE]
> Amortyzacja i wymagane migracji na 31 marca 2021 planuje się kontami ZRS klasycznego. Firma Microsoft będzie wysyłać szczegółowe klientom ZRS klasycznego przed wycofywanie.

Dodatkowe pytania są opisane w [— często zadawane pytania](#frequently-asked-questions) sekcji. 

## <a name="geo-redundant-storage"></a>Magazyn geograficznie nadmiarowy
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-GRS.md)]

## <a name="read-access-geo-redundant-storage"></a>Dostęp do odczytu magazynu geograficznie nadmiarowego
Dostęp do odczytu magazynu geograficznie nadmiarowego (RA-GRS) maksymalizację dostępności dla konta magazynu. RA-GRS zapewnia dostęp tylko do odczytu do danych w dodatkowej lokalizacji, oprócz — replikacja geograficzna dwóch regionach.

Po włączeniu dostępu tylko do odczytu do danych w regionie pomocniczym, dane są dostępne na dodatkowej punktu końcowego, a także podstawowy punkt końcowy dla konta magazynu. Pomocniczy punkt końcowy jest podobny do podstawowego punktu końcowego, ale dołącza sufiks `–secondary` do nazwy konta. Na przykład, jeśli jest podstawowym punktu końcowego usługi Blob `myaccount.blob.core.windows.net`, to jest punkt końcowy dodatkowej `myaccount-secondary.blob.core.windows.net`. Klawisze dostępu dla konta magazynu są takie same dla obu głównych i dodatkowych punktów końcowych.

Niektóre kwestie należy wziąć pod uwagę w przypadku korzystania z RA-GRS:

* Aplikacja musi zarządzać którym punktem końcowym, używając RA-GRS prowadzi interakcję z.
* Ponieważ asynchroniczną replikację obejmuje opóźnienia, zmiany, które nie zostały jeszcze zreplikowane w regionie pomocniczym mogą zostać utracone, jeśli nie można odzyskać danych z regionu podstawowego, na przykład w przypadku regionalnej awarii.
* Jeśli Microsoft inicjuje trybu failover w regionie pomocniczym, będą mieć odczytu i zapisu do tych danych po przejściu w tryb failover została ukończona. Aby uzyskać więcej informacji, zobacz [wskazówki dotyczące odzyskiwania po awarii](../storage-disaster-recovery-guidance.md).
* RA-GRS jest przeznaczony dla celów wysokiej dostępności. Wskazówki dotyczące skalowalności, można przejrzeć [Lista kontrolna wydajności](../storage-performance-checklist.md).

## <a name="frequently-asked-questions"></a>Często zadawane pytania

<a id="howtochange"></a>
#### <a name="1-how-can-i-change-the-geo-replication-type-of-my-storage-account"></a>1. Jak zmienić typ — replikacja geograficzna moim koncie magazynu?

   Replikacja geograficzna typ konta magazynu można zmienić za pomocą [portalu Azure](https://portal.azure.com/), [programu Azure Powershell](storage-powershell-guide-full.md), lub jeden z biblioteki klienta magazynu Azure.

   > [!NOTE]
   > Kontami ZRS nie może być przekonwertowany LRS lub GRS. Podobnie istniejące konto LRS lub GRS nie można przekonwertować na konto ZRS.
    
<a id="changedowntime"></a>
#### <a name="2-does-changing-the-replication-type-of-my-storage-account-result-in-down-time"></a>2. Oznacza zmianę typu replikacji Moje magazynu konta wyniku w dół czas?

   Nie, zmiany typu replikacji konta magazynu nie powoduje czas przestoju.

<a id="changecost"></a>
#### <a name="3-are-there-additional-costs-to-changing-the-replication-type-of-my-storage-account"></a>3. Czy istnieją dodatkowe koszty na zmieniające się typ replikacji konta magazynu?

   Tak. W przypadku zmiany z replikacji LRS na GRS (lub RA-GRS) dla konta magazynu jest naliczane dodatkowe opłaty za wyjście objętego kopiowanie istniejących danych z lokalizacji podstawowej do dodatkowej lokalizacji. Po skopiowaniu danych początkowej nie istnieją żadne opłaty za dalsze dodatkowe wyjście za replikację geograficzną w podstawowej do dodatkowej lokalizacji. Aby uzyskać więcej informacji o opłatach przepustowości, zobacz [cennik usługi Azure Storage strony](https://azure.microsoft.com/pricing/details/storage/blobs/).

   Jeśli zmienisz z GRS LRS, nie ma żadnych dodatkowych kosztów, ale dane są usuwane z lokalizacji dodatkowej.

<a id="ragrsbenefits"></a>
#### <a name="4-how-can-ra-grs-help-me"></a>4. Jak RA-GRS może mi pomóc?

   Magazyn GRS zapewnia replikację danych z głównego w regionie pomocniczym będący setki odległości od regionu podstawowego. W wypadku magazynu GRS dane są trwałe nawet w przypadku pełnej regionalnej awarii lub awarii, w którym regionie podstawowym nie jest możliwe do odzyskania. RA-GRS magazynu zapewnia replikacji GRS i dodaje możliwości odczytu danych z lokacji dodatkowej. Masz sugestie dotyczące projektowania wysokiej dostępności, zobacz [Projektowanie wysoce dostępnych aplikacji przy użyciu magazynu RA-GRS](../storage-designing-ha-apps-with-ragrs.md).

<a id="lastsynctime"></a>
#### <a name="5-is-there-a-way-to-figure-out-how-long-it-takes-to-replicate-my-data-from-the-primary-to-the-secondary-region"></a>5. Czy istnieje sposób, aby dowiedzieć się, jak długo trwa replikowanie danych z serwera podstawowego w regionie pomocniczym?

   Jeśli używasz magazynu RA-GRS, można sprawdzić czas ostatniej synchronizacji z konta magazynu. Czas ostatniej synchronizacji jest wartość daty/godziny GMT. Wszystkie zapisy głównej przed czas ostatniej synchronizacji zostały pomyślnie zapisane w lokalizacji dodatkowej, co oznacza, że są one dostępne do odczytu z lokalizacji dodatkowej. Podstawowy zapisuje po czas ostatniej synchronizacji może lub mogą nie być dostępne dla odczytów jeszcze. Można badać przy użyciu tej wartości [portalu Azure](https://portal.azure.com/), [programu Azure PowerShell](storage-powershell-guide-full.md), albo z jednego z biblioteki klienta magazynu Azure.

<a id="outage"></a>
#### <a name="6-if-there-is-an-outage-in-the-primary-region-how-do-i-switch-to-the-secondary-region"></a>6. Jeśli występuje awaria w regionie podstawowym, jak przełączać w regionie pomocniczym?

   Aby uzyskać więcej informacji, zobacz [co robić w przypadku wystąpienia awarii usługi Azure Storage](../storage-disaster-recovery-guidance.md).

<a id="rpo-rto"></a>
#### <a name="7-what-is-the-rpo-and-rto-with-grs"></a>7. Co to jest RPO i RTO z GRS?

   **Cel punktu odzyskiwania (RPO):** w GRS i RA-GRS, magazyn usługi asynchronicznie geograficznie są replikowane dane z serwera podstawowego do lokalizacji dodatkowej. W przypadku poważnej awarii regionalnych w regionie podstawowym Microsoft wykonuje trybu failover w regionie pomocniczym. W przypadku trybu failover najnowszych zmian, które nie zostały jeszcze replikacją geograficzną mogą zostać utracone. Liczba minut potencjalnych utraconych danych jest określana jako cel punktu odzyskiwania i wskazuje punkt w czasie, do którego można odzyskać dane. Magazyn Azure zazwyczaj ma RPO mniej niż 15 minut, mimo że nie ma żadnych SLA na jak długo replikacja geograficzna przyjmuje.

   **Celu czasu odzyskiwania (RTO):** Docelowy jest miarą czas pracy w trybie failover i konto magazynu do trybu online. Czas pracy w trybie failover zawiera następujące akcje:

   * Czas firmy Microsoft wymaga, aby określić, czy dane można odzyskać w lokalizacji głównej, czy tryb failover jest konieczne.
   * Czas pracy w trybie failover konta magazynu, zmieniając głównej wpisy DNS, aby wskazać lokalizację dodatkowej.

   Microsoft przyjmuje odpowiedzialność poważnie zachowania danych. Jeśli jakakolwiek możliwość odzyskania danych w regionie podstawowym, firma Microsoft będzie opóźnienie pracy awaryjnej i skupić się na odzyskanie danych. Przyszłych wersji usługi pozwoli wyzwolić tryb failover na poziomie konta, aby użytkownik Docelowy można kontrolować.

#### <a name="8-what-azure-storage-objects-does-zrs-support"></a>8. Jakie obiekty usługi Azure Storage obsługuje ZRS? 
Blokowe obiekty BLOB, stronicowych obiektów blob (z wyjątkiem tych dysków maszyny Wirtualnej zapasowy) tabel, plików i kolejek. 

#### <a name="9-does-zrs-also-include-geo-replication"></a>9. Magazyn ZRS również zawiera — replikacja geograficzna? 
Magazyn ZRS nie obsługuje obecnie replikację geograficzną. Jeśli dany scenariusz wymaga replikacji między region na potrzeby odzyskiwania po awarii, należy użyć konta magazynu GRS lub RA-GRS.   

#### <a name="10-what-happens-when-one-or-more-zrs-zones-go-down"></a>10. Co się stanie, gdy co najmniej jedna strefa ZRS przestaną działać? 
Gdy pierwszą strefę ulegnie awarii, ZRS kontynuuje zapis repliki danych na dwóch pozostałych stref w regionie. Jeśli drugiej strefy ulegnie awarii, odczytu i zapisu jest niedostępny, dopóki ponownie co najmniej dwie strefy są dostępne. 

## <a name="next-steps"></a>Kolejne kroki
* [Projektowanie aplikacji wysokiej dostępności przy użyciu magazynu RA-GRS](../storage-designing-ha-apps-with-ragrs.md)
* [Cennik usługi Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Informacji o kontach magazynu Azure](../storage-create-storage-account.md)
* [Azure cele wydajności i skalowalności magazynu](storage-scalability-targets.md)
* [Microsoft Azure Storage nadmiarowość opcje i dostęp do odczytu z magazynu geograficznie nadmiarowego magazynu](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
* [SOSP Paper - Azure Storage: Wysoce dostępna usługa magazynu w chmurze z wysoki poziom spójności](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
