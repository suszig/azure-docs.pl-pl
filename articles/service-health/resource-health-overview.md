---
title: "Przegląd kondycji zasobów platformy Azure | Dokumentacja firmy Microsoft"
description: "Przegląd kondycji zasobów platformy Azure"
services: Resource health
documentationcenter: 
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/01/2017
ms.author: BernardoAMunoz
ms.openlocfilehash: 040d58a81a9b41fe660e4276d698bf884f90bb6c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-resource-health-overview"></a>Przegląd kondycji zasobów platformy Azure
 
Usługa Resource Health ułatwia diagnozowanie i uzyskanie pomocy technicznej, gdy problem z platformą Azure ma wpływ na zasoby. Informuje o bieżącej i wcześniejszej kondycji zasobów oraz pomaga uniknąć problemów. Usługa Resource Health oferuje pomoc techniczną w przypadku problemów z usługą platformy Azure.

Podczas gdy [stan Azure](https://status.azure.com) informuje o problemów dotyczących usługi, które mają wpływ na szerokiego zakresu klientów platformy Azure, kondycja zasobu zawiera spersonalizowane pulpit nawigacyjny kondycji zasobów. Kondycja zasobów zawiera cały czas, który zasoby były niedostępne w przeszłości z powodu problemów dotyczących usługi Azure. Upraszcza można łatwiej zrozumieć, jeśli zostało naruszone umowy dotyczącej poziomu usług. 

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-a-resource-is-healthy-or-not"></a>Co to jest uznawany za zasobu i jak kondycja zasobów decyduje o tym, jeśli zasób jest uszkodzony lub nie?
Zasób jest wystąpieniem typu zasobu oferowane przez usługę Azure za pomocą usługi Azure Resource Manager, na przykład: maszynę wirtualną, aplikacji sieci web lub bazy danych SQL.

Kondycja zasobów polega na sygnały emitowane przez różne usługi platformy Azure do oceny, czy zasób jest uszkodzony lub nie. Jeśli zasób jest zła, kondycja zasobu analizuje dodatkowych informacji w celu ustalenia źródła problemu. Identyfikuje również akcje Microsoft trwa do rozwiązania problemu lub jakie akcje można wykonać w celu rozwiązania przyczynę problemu. 

Zapoznaj się z pełną listą typów zasobów i kontroli kondycji w [kondycji zasobów platformy Azure](resource-health-checks-resource-types.md) dodatkowe szczegóły dotyczące sposób jest oceny kondycji.

## <a name="health-status-provided-by-resource-health"></a>Dostarczone przez kondycji zasobów stan kondycji
Kondycja zasobu jest jednym z następujących stanów:

### <a name="available"></a>Dostępna
Usługa wykryła nie wszystkie zdarzenia wpływające na kondycję zasobu. W przypadkach, gdzie zasobu odzyskał sprawność po nieplanowanych przestojów w ciągu ostatnich 24 godzin zostanie wyświetlona **ostatnio odzyskana** powiadomień.

![Maszyna wirtualna zasobów kondycji dostępności](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Niedostępne
Usługa wykryła trwającą platformy lub zdarzenia-platforma wpływające na kondycję zasobu.

#### <a name="platform-events"></a>Zdarzenia platformy
Te zdarzenia są wyzwalane przez kilka składników infrastruktury platformy Azure i obejmują zarówno zaplanowanych akcji, takich jak planowanej konserwacji i nieoczekiwane zdarzenia, takie jak ponowne uruchomienie komputera hosta nieplanowane.

Kondycja zasobów zawiera dodatkowe szczegóły dotyczące zdarzeń, proces odzyskiwania i umożliwia się z pomocą techniczną, nawet jeśli nie masz aktywnych pomocy technicznej umowy firmy Microsoft.

![Zasób kondycji niedostępny maszyny wirtualnej z powodu zdarzenia platformy](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a>Zdarzenia non-platformy
Te zdarzenia są wyzwalane przez akcje wykonywane przez użytkowników, na przykład zatrzymanie maszyny wirtualnej lub osiągnięciu maksymalnej liczby połączeń z pamięcią podręczną Redis.

![Zasób kondycji niedostępny maszyny wirtualnej z powodu zdarzenia z systemem innym niż platformy](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a>Nieznany
Ten stan kondycji wskazuje, że kondycja zasobu nie otrzymano informacji na temat tego zasobu na więcej niż 10 minut. Gdy ten stan nie jest oznaczenie ostateczne stanu zasobu, jest ważne dane w proces rozwiązywania problemów:
* Jeśli zasób działa zgodnie z oczekiwaniami stan zasobu zostaną zaktualizowane na dostępne po kilku minutach.
* Jeśli występują problemy z zasobem, stan kondycji nieznany może sugerują, że zasób jest wpływ zdarzenie w platformie.

![Nieznany maszyny wirtualnej kondycji zasobów](./media/resource-health-overview/Unknown.png)

## <a name="report-an-incorrect-status"></a>Niepoprawny stan raportu
Jeśli w dowolnym momencie uważasz bieżący stan kondycji jest niepoprawny, możesz można Daj nam znać, klikając **zgłoszenia stanu kondycji niepoprawne**. W przypadkach, w których ma wpływ Azure problem, firma Microsoft zachęca się z pomocą techniczną w bloku kondycji zasobów. 

![Kondycja zasobu raportu niepoprawny stan.](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a>Informacje historyczne
Dostęp do danych historycznych kondycji do 14 dni, klikając **wyświetlanie historii** w bloku kondycji zasobów. 

![Kondycja zasobów historii raportu](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a>Wprowadzenie
Aby otworzyć kondycja zasobów dla jednego zasobu
1.  Zaloguj się do portalu Azure.
2.  Przejdź do zasobu.
3.  W menu zasobów znajdujący się po lewej stronie, kliknij przycisk **kondycja zasobów**.

![Otwórz kondycja zasobów z bloku zasobów](./media/resource-health-overview/from-resource-blade.png)

Kondycja zasobów można również uzyskać, klikając **więcej usług**i wpisując **kondycja zasobów** w polu tekstowym filtru, aby otworzyć **Pomoc i obsługa techniczna** bloku. Na koniec kliknij [ **kondycja zasobów**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).

![Otwórz z usługi więcej kondycja zasobów](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a>Następne kroki

Wyewidencjonuj tych zasobów, aby dowiedzieć się więcej na temat kondycji zasobów:
-  [Typy zasobów i kondycji sprawdza w kondycji zasobów platformy Azure](resource-health-checks-resource-types.md)
-  [Często zadawane pytania dotyczące kondycji zasobów platformy Azure](resource-health-faq.md)




