---
title: "Usługa przydziały i limity dla partii zadań Azure | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat przydziałów domyślnych partii zadań Azure, ograniczenia i ograniczenia i zwiększa jak utworzyć żądanie przydziału"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f3f69ed8d3a985afe07e648e7512a88b25278ced
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="batch-service-quotas-and-limits"></a>Limity przydziału i limity usługi Batch

Zgodnie z innymi usługami Azure, istnieją ograniczenia na niektóre zasoby skojarzone z usługi partia zadań. Wiele tych limitów jest przydziałów domyślnych zastosowane przez platformę Azure na poziomie konta lub subskrypcji. W tym artykule omówiono te ustawienia domyślne, i jak mogą żądać przydziału zwiększa.

Pamiętaj o tych limitach przydziału podczas projektowania i skalowania obciążeń usługi Batch. Na przykład jeśli pulę nie jest osiągnięciu docelowej liczby węzłów obliczeniowych, określonych przez Ciebie, może osiągnięto limit przydziału rdzeni dla swojego konta usługi partia zadań lub regionalnych przydziału rdzeni maszyn wirtualnych dla Twojej subskrypcji.

Można uruchomić wiele obciążeń usługi Batch na jednym koncie usługi Batch lub rozdzielić obciążenia pomiędzy konta tej usługi znajdujące się w jednej subskrypcji, ale różnych regionach świadczenia usługi Azure.

Jeśli planujesz uruchamianie obciążeń produkcyjnych w partii, może być konieczne zwiększyć co najmniej jeden przydziały powyżej domyślny. Jeśli użytkownik chce podnieść limit przydziału, możesz otworzyć online [żądania obsługi klienta](#increase-a-quota) bez dodatkowych opłat.

> [!NOTE]
> Limit przydziału jest limit kredytu nie gwarancji pojemności. Jeśli wymagana pojemność na dużą skalę, skontaktuj się z pomocą techniczną platformy Azure.
> 
> 

## <a name="resource-quotas"></a>Limity przydziałów zasobów
[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="quotas-in-user-subscription-mode"></a>Przydziały w trybie użytkownika subskrypcji

Dla konta wsadowego z trybem przydziału puli ustawioną **subskrypcji użytkownika**, partii maszyny wirtualne i inne zasoby, takie jak kont magazynu, są tworzone bezpośrednio w ramach Twojej subskrypcji po utworzeniu puli. Limit przydziału rdzeni partii zadań Azure nie ma zastosowania do konto utworzone w tym trybie. Zamiast tego przydziały w subskrypcji dla regionalne obliczeniowe rdzeni i inne zasoby są stosowane. Dowiedz się więcej o te przydziały w [subskrypcji platformy Azure i usługi limity, przydziały i ograniczenia](../azure-subscription-service-limits.md).

Podczas planowania użycia zasobów dla konto utworzone w trybie użytkownika subskrypcji, należy pamiętać, że następujące zasoby partii (oprócz obliczeń rdzenie) są wymagane dla każdego 40 maszyn wirtualnych systemu Linux lub 20 maszyn wirtualnych systemu Windows:

| Zasób | Przydział | Dostawca |
| --- | ---| --- |
| Jedno konto magazynu | Konta magazynu | Microsoft.Storage |
| Jeden publiczny adres IP | Publiczne adresy IP | Microsoft.Network | 
| Jedną sieć wirtualną | Sieci wirtualne | Microsoft.Network | 
| Grupy zabezpieczeń sieci | Grupy zabezpieczeń sieci | Microsoft.Network | 
| Jeden zestaw skali maszyny wirtualnej | Zestawy skali maszyn wirtualnych | Microsoft.Compute | 
| Jedna usługa równoważenia obciążenia | Moduły równoważenia obciążenia | Microsoft.Network | 

Należy ustawić limit przydziału rdzeni na poziomie regionalnym lub na rodzinę maszyny Wirtualnej na podstawie rozmiaru maszyny Wirtualnej wymagana dla puli partii lub pule:

| Przydział | Dostawca |
| --- | ---- |
| Całkowita liczba rdzeni regionalne | Microsoft.Compute |
| … Rodziny rdzeni | Microsoft.Compute |



## <a name="other-limits"></a>Inne ograniczenia.
| **Zasób** | **Limit maksymalny** |
| --- | --- |
| [Równoczesnych zadań](batch-parallel-node-tasks.md) na węzeł obliczeń |4 x liczba rdzeni węzła |
| [Aplikacje](batch-application-packages.md) na konto usługi partia zadań |20 |
| Pakiety aplikacji na aplikację |40 |
| Rozmiar pakietu aplikacji (wszystkie) |Około 195GB<sup>1</sup> |
| Maksymalna początkowy rozmiar zadań | znaki 32768<sup>2</sup> |

<sup>1</sup> limit bloku maksymalny rozmiar obiektu blob magazynu azure<br />
<sup>2</sup> obejmuje plików zasobów i zmienne środowiskowe

## <a name="view-batch-quotas"></a>Przydziały partii widoku
Wyświetl przydziałami konta wsadowego w [portalu Azure][portal].

1. Wybierz **partii kont** w portalu, a następnie wybierz konto usługi partia zadań interesuje Cię.
2. Wybierz **właściwości** w bloku menu konta usługi partia zadań.
3. Wyświetla bloku właściwości **przydziały** zastosowanym do konta usługi partia zadań
   
    ![Przydziały konta usługi partia zadań][account_quotas]

Dla konta wsadowego, utworzony w trybie użytkownika subskrypcji należy wyświetlić przydziały pokrewne subskrypcji w portalu Azure.

1. Wybierz **subskrypcje**i wybierz subskrypcję, której używasz dla konta usługi partia zadań.

2. Na **subskrypcji** bloku, wybierz opcję **użycia + przydziały**.



## <a name="increase-a-quota"></a>Zwiększ limit przydziału
Wykonaj następujące kroki, aby zażądać przydział zwiększyć dla swojego konta usługi partia zadań lub subskrypcją za pomocą [portalu Azure][portal]. Typ zwiększenia limitu przydziału zależy od trybu alokacji puli konta partii zadań.

### <a name="increase-a-batch-cores-quota"></a>Zwiększ limit przydziału rdzeni partii 

Jeśli Twoje konto usługi partia zadań została utworzona w **partii usługi** tryb, wykonaj następujące kroki, aby zażądać przydziału rdzeni partii zwiększyć:

1. Wybierz **Pomoc i obsługa techniczna** kafelka na pulpicie nawigacyjnym portalu lub znak zapytania (**?**) w prawym górnym rogu portalu.
2. Wybierz **nowy obsługuje żądania** > **podstawy**.
3. Na **podstawy** bloku:
   
    a. **Wystawianie typu** > **przydziału**
   
    b. Wybierz subskrypcję.
   
    c. **Typ limitu przydziału** > **partii**
   
    d. **Plan pomocy technicznej** > **Obsługa przydziałów — włączone**
   
    Kliknij przycisk **Dalej**.
4. Na **Problem** bloku:
   
    a. Wybierz **ważność** zgodnie z [wpływ na prowadzoną działalność][support_sev].
   
    b. W **szczegóły**, określ każdego przydziału, aby zmienić nazwę konta wsadowego i nowego limitu.
   
    Kliknij przycisk **Dalej**.
5. Na **informacje kontaktowe** bloku:
   
    a. Wybierz **preferowana metoda kontaktu**.
   
    b. Sprawdź i wprowadź wymagane szczegóły dotyczące kontaktu.
   
    Kliknij przycisk **Utwórz**, aby przesłać żądanie pomocy technicznej.

Po przesłaniu żądania obsługi pomocy technicznej platformy Azure skontaktuje się z Tobą. Należy pamiętać, że wykonywania żądania może potrwać maksymalnie 2 dni roboczych.

### <a name="increase-a-subscription-cores-quota"></a>Zwiększ limit przydziału rdzeni subskrypcji

Jeśli Twoje konto usługi partia zadań została utworzona w **subskrypcji użytkownika** tryb i potrzebujesz dodatkowych regionalnych lub wirtualna rodziny rdzeni żądania, a limit przydziału Zwiększ w ramach subskrypcji. Aby uzyskać instrukcje, zobacz [żądań Zwiększ limit przydziału rdzeni Resource Manager](../azure-supportability/resource-manager-core-quotas-request.md).



## <a name="related-topics"></a>Powiązane tematy
* [Tworzenie konta usługi partia zadań Azure za pomocą portalu Azure](batch-account-create-portal.md)
* [Omówienie funkcji usługi partia zadań Azure](batch-api-basics.md)
* [Subskrypcji platformy Azure i usługi limity, przydziały i ograniczenia](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
