---
title: "Skalowanie w poziomie roli proces roboczy w usługach App - stosu Azure | Dokumentacja firmy Microsoft"
description: "Szczegółowe wskazówki dotyczące skalowania usługi aplikacji Azure stosu"
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 3cbe87bd-8ae2-47dc-a367-51e67ed4b3c0
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2018
ms.author: brenduns
ms.reviewer: anwestg
ms.openlocfilehash: ddd9820715e964218db8b88fb5211b3725c808b9
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="app-service-on-azure-stack-add-more-infrastructure-or-worker-roles"></a>Usługi aplikacji Azure stosu: Dodaj więcej ról infrastruktury lub procesu roboczego
*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*  

Ten dokument zawiera instrukcje dotyczące sposobu skalowania usługi aplikacji Azure stosu ról infrastruktury i proces roboczy. Zawiera ona procedury służące do tworzenia ról dodatkowych procesów roboczych do obsługi aplikacji o dowolnym rozmiarze.

> [!NOTE]
> Jeśli środowiska Azure stos nie zawiera więcej niż 96 GB pamięci RAM mogą mieć trudności dodanie dodatkowej pojemności.

Usługi aplikacji Azure stosu, domyślnie obsługuje warstw bezpłatne i udostępniane procesu roboczego. Aby dodać inne warstw procesu roboczego, należy dodać jedną rolę procesu roboczego.

Jeśli nie masz pewności, jaka została wdrożona przy użyciu domyślnego usługi aplikacji Azure stosu instalacji, możesz przejrzeć dodatkowe informacje w [usługi aplikacji — omówienie stosu Azure](azure-stack-app-service-overview.md).

Usługa aplikacji Azure na stosie Azure wdraża wszystkich ról przy użyciu zestawy skalowania maszyny wirtualnej i jako takie korzysta z możliwości skalowania danego obciążenia. W związku z tym wszystkie skalowania warstwy procesu roboczego odbywa się za pośrednictwem administratora aplikacji usługi.

Dodawanie dodatkowych pracowników bezpośrednio z poziomu administratora dostawcy zasobów usługi aplikacji.

1. Zaloguj się do portalu administracyjnego platformy Azure stosu jako administratora usługi.

2. Przejdź do **usługi aplikacji**.

    ![](media/azure-stack-app-service-add-worker-roles/image01.png)

3. Kliknij przycisk **ról**. W tym miejscu wyświetlić podział wszystkich ról usług aplikacji wdrożone.

4. Kliknij prawym przyciskiem myszy w wierszu typu, aby skalować, a następnie kliknij przycisk **ScaleSet**.

    ![](media/azure-stack-app-service-add-worker-roles/image02.png)

5. Kliknij przycisk **skalowanie**, wybierz liczbę wystąpień chcesz skalować, a następnie kliknij przycisk **zapisać**.

    ![](media/azure-stack-app-service-add-worker-roles/image03.png)

6. Usługi aplikacji Azure stosu będzie teraz Dodawanie dodatkowych maszyn wirtualnych, skonfigurować ich zainstalowania wymaganego oprogramowania i oznaczyć je jako gotowy, po zakończeniu tego procesu. Ten proces może potrwać około minuty 80.

7. Postęp gotowość nowe role można monitorować, wyświetlając pracowników w **ról** bloku.

Po umieszczeniu ich w pełni wdrożone i gotowe, pracownicy stają się dostępne dla użytkowników wdrożyć swoje obciążeń na nich. Poniżej pokazano przykład wielu dostępne warstwy cenowe domyślnie. Jeśli nie ma żadnych dostępnych pracowników dla warstwy określonego procesu roboczego, może wybrać odpowiedniego warstwa cenowa jest niedostępna.

![](media/azure-stack-app-service-add-worker-roles/image04.png)

>[!NOTE]
> Do skalowania w poziomie zarządzania, role frontonu lub wydawcy dodać musi skalowanie w poziomie odpowiedni zestaw skalowania maszyny Wirtualnej. Dodamy możliwość skalowania tych ról za pośrednictwem funkcji zarządzania usługi App Service w przyszłej wersji.

Do skalowania w poziomie zarządzania, frontonu lub role wydawcy, wykonaj te same czynności, wybierając typ odpowiednią rolę. Kontrolery nie są wdrażane jako zestawy skalowania i w związku z tym dwóch powinny zostać wdrożone w czasie instalacji dla wszystkich wdrożeń produkcyjnych.

### <a name="next-steps"></a>Kolejne kroki

[Konfigurowanie źródeł wdrożenia](azure-stack-app-service-configure-deployment-sources.md)
