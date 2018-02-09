---
title: "Utwórz ofertę w stosie Azure | Dokumentacja firmy Microsoft"
description: "Jako administrator chmury Dowiedz się, jak utworzyć ofertę dla użytkowników w stosie Azure."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 96b080a4-a9a5-407c-ba54-111de2413d59
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/06/2018
ms.author: brenduns
ms.openlocfilehash: 2666aacbbaf44ae9b3bcf2df480e835457e6f449
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="create-an-offer-in-azure-stack"></a>Tworzenie oferty w usłudze Azure Stack

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

[Oferuje](azure-stack-key-features.md) są grupami co najmniej jeden plan udostępniające dostawców do użytkowników w celu zakupu lub subskrybować. Ten dokument przedstawia sposób tworzenia oferty, która obejmuje [utworzony plan](azure-stack-create-plan.md) w ostatnim kroku. Ta oferta umożliwia subskrybentów na umieszczanie maszyn wirtualnych.

1. Zaloguj się do portalu administratora platformy Azure stosu (https://adminportal.local.azurestack.external) > kliknij **nowy** > **oferuje dzierżawy + planów**  >   **Oferują**.

   ![](media/azure-stack-create-offer/image01.png)
2. W **oferują nowe** wypełnienia w okienku **Nazwa wyświetlana** i **Nazwa zasobu**, a następnie wybierz nowy lub istniejący **grupy zasobów**. Nazwa wyświetlana jest przyjazna nazwa dla oferty. Przyjaznej nazwy, to tylko informacje o ofercie, który jest wyświetlany podczas subskrybowania. W związku z tym należy użyć nazwy intuicyjny, która ułatwia użytkownikom zrozumienie, co jest dostarczany z oferty. Nazwa zasobu jest widoczna tylko dla administratora. Jest to nazwa używana przez administratorów do pracy z ofertą jako zasobem usługi Azure Resource Manager.

   ![](media/azure-stack-create-offer/image01a.png)
3. Kliknij przycisk **podstawowa planów** otworzyć **Planowanie** okienku wybierz planów chcesz dołączyć do oferty, a następnie kliknij przycisk **wybierz**. Kliknij pozycję **Utwórz**, aby utworzyć ofertę.

   ![](media/azure-stack-create-offer/image02.png)
4. Kliknij przycisk **wszystkie zasoby**wyszukać nowe ofertę, kliknij na nowej oferty, kliknij przycisk **zmiany stanu**, a następnie kliknij przycisk **publicznego**.

   ![](media/azure-stack-create-offer/image03.png)

Oferty muszą być wprowadzane publiczne dla użytkowników w celu pełnego widoku subskrypcji. Oferty można:

* **Publiczny**: widoczne dla użytkowników.
* **Prywatne**: widoczne tylko dla administratorów chmury. Przydatne podczas opracowywania planu lub oferty, lub jeśli administrator chmury chce [utworzyć każdej subskrypcji dla użytkowników](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator).
* **Zlikwidowane**: zamknięte dla nowych subskrybentów. Administrator chmury można użyć wycofany z eksploatacji uniknąć przyszłych subskrypcji, pozostawiając jednak bieżący subskrybentów bez zmian.

Zmiany do oferty, nie są bezpośrednio widoczne dla użytkowników. Aby wyświetlić zmiany, może być konieczne wylogowanie/logowania, aby wyświetlić nową subskrypcję w selektorze"subskrypcji" podczas tworzenia zasobów lub grupy zasobów.

> [!NOTE]
>Można również utworzyć za pomocą programu PowerShell, zgodnie z objaśnieniem w oferty domyślne, planów i przydziały [readme administratora usługi Azure stosu](https://github.com/Azure/AzureStack-Tools/tree/master/ServiceAdmin).
>


### <a name="next-steps"></a>Kolejne kroki
[Tworzenie subskrypcji](azure-stack-subscribe-plan-provision-vm.md)      
[Inicjowanie obsługi administracyjnej maszyny wirtualnej](azure-stack-provision-vm.md)
