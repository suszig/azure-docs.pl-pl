---
title: "Utwórz ofertę w stosie Azure | Dokumentacja firmy Microsoft"
description: "Jako administrator chmury Dowiedz się, jak utworzyć ofertę dla użytkowników w stosie Azure."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 96b080a4-a9a5-407c-ba54-111de2413d59
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: erikje
ms.openlocfilehash: 269a6106f657536ba74be366f842b2f9cd86c5dc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-offer-in-azure-stack"></a>Tworzenie oferty w usłudze Azure Stack

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

[Oferuje](azure-stack-key-features.md) są grupami co najmniej jeden plan udostępniające dostawców do użytkowników w celu zakupu lub subskrybować. Ten dokument przedstawia sposób tworzenia oferty, która obejmuje [utworzony plan](azure-stack-create-plan.md) w ostatnim kroku. Ta oferta umożliwia subskrybentów na umieszczanie maszyn wirtualnych.

1. Zaloguj się do portalu administratora platformy Azure stosu (https://adminportal.local.azurestack.external) > kliknij **nowy** > **oferuje dzierżawy + planów**  >   **Oferują**.

   ![](media/azure-stack-create-offer/image01.png)
2. W **oferują nowe** bloku, wypełnij **Nazwa wyświetlana** i **Nazwa zasobu**, a następnie wybierz nowy lub istniejący **grupy zasobów**. Nazwa wyświetlana jest przyjazna nazwa oferty i to tylko informacje o ofercie, którą użytkownicy zobaczą, gdy subskrypcja. W związku z tym należy użyć nazwy intuicyjny, która ułatwia użytkownikom zrozumienie, co jest dostarczany z oferty. Nazwa zasobu jest widoczna tylko dla administratora. Jest to nazwa używana przez administratorów do pracy z ofertą jako zasobem usługi Azure Resource Manager.

   ![](media/azure-stack-create-offer/image01a.png)
3. Kliknij pozycję **Plany bazowe** i w bloku **Plan** wybierz plany, które chcesz uwzględnić w ofercie, a następnie kliknij przycisk **Wybierz**. Kliknij pozycję **Utwórz**, aby utworzyć ofertę.

   ![](media/azure-stack-create-offer/image02.png)
4. Kliknij przycisk **wszystkie zasoby**wyszukać nowe ofertę, kliknij na nowej oferty, kliknij przycisk **zmiany stanu**, a następnie kliknij przycisk **publicznego**.

   ![](media/azure-stack-create-offer/image03.png)

Oferty muszą być wprowadzane publiczne dla użytkowników w celu pełnego widoku subskrypcji. Oferty można:

* **Publiczny**: widoczne dla użytkowników.
* **Prywatne**: widoczne tylko dla administratorów chmury. Przydatne podczas opracowywania planu lub oferty, lub jeśli administrator chmury chce zatwierdzenia każdej subskrypcji.
* **Zlikwidowane**: zamknięte dla nowych subskrybentów. Administrator chmury można użyć wycofany z eksploatacji uniknąć przyszłych subskrypcji, pozostawiając jednak bieżący subskrybentów bez zmian.

Zmiany do oferty, nie są bezpośrednio widoczne dla użytkowników. Aby wyświetlić zmiany, może być konieczne wylogowanie/logowania, aby wyświetlić nową subskrypcję w selektorze"subskrypcji" podczas tworzenia zasobów lub grupy zasobów.

> [!NOTE]
>Można również utworzyć za pomocą programu PowerShell, zgodnie z objaśnieniem w oferty domyślne, planów i przydziały [readme administratora usługi Azure stosu](https://github.com/Azure/AzureStack-Tools/tree/master/ServiceAdmin).
>


### <a name="next-steps"></a>Następne kroki
[Subskrypcja do oferty, a następnie zainicjujesz maszyny Wirtualnej](azure-stack-subscribe-plan-provision-vm.md)
