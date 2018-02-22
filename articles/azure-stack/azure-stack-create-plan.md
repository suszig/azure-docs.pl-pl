---
title: Tworzenie planu w stosie Azure | Dokumentacja firmy Microsoft
description: "Jako administrator chmury Utwórz plan, który umożliwia maszynom wirtualnym udostępniania subskrybentów."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 7/10/2017
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 5eefca3541ae9f73514f80b0f8df9e5027600f87
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-plan-in-azure-stack"></a>Tworzenie planu w usłudze Azure Stack

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

[Plany](azure-stack-key-features.md) są grupami obejmującymi co najmniej jedną usługę. Jako dostawca można utworzyć plany oferować użytkownikom. Z kolei użytkownikom subskrybować oferty korzystać z planów i usług, które obejmują one. Ten przykład przedstawia sposób tworzenia planu, który zawiera compute, sieci i dostawcy zasobów magazynu. Ten plan umożliwia subskrybentów na umieszczanie maszyn wirtualnych.

1. Zaloguj się do portalu administratora platformy Azure stosu (https://adminportal.local.azurestack.external). Wprowadź poświadczenia dla konta, który został utworzony w kroku 5 [uruchomienia skryptu programu PowerShell](azure-stack-run-powershell-script.md) sekcji.

2. Aby utworzyć plan i oferty, które użytkownicy mogą subskrybować, kliknij przycisk **nowy** > **oferuje dzierżawy + planów** > **planu**.

   ![](media/azure-stack-create-plan/image01.png)
3. W **nowy Plan** bloku, wypełnij **Nazwa wyświetlana** i **Nazwa zasobu**. Nazwa wyświetlana jest przyjazna nazwa planu, dostępna dla użytkowników. Nazwa zasobu jest widoczna tylko dla administratora. Jest to nazwa, których administratorzy używać do pracy z planem jako zasób usługi Azure Resource Manager.

   ![](media/azure-stack-create-plan/image02.png)
4. Utwórz nową **grupy zasobów**, lub wybierz istniejący, jako kontener dla planu.

   ![](media/azure-stack-create-plan/image02a.png)
5. Kliknij przycisk **usług**, wybierz pozycję **Microsoft.Compute**, **Microsoft.Network**, i **Microsoft.Storage**, a następnie kliknij przycisk **Wybierz**.

   ![](media/azure-stack-create-plan/image03.png)
6. Kliknij przycisk **przydziały**, kliknij przycisk **Microsoft.Storage (lokalne)**, a następnie wybierz domyślnego przydziału lub kliknij przycisk **Utwórz nowy przydział** dostosować limit przydziału.

   ![](media/azure-stack-create-plan/image04.png)
7. Jeśli tworzysz nowy przydział, wprowadź nazwę dla limitu przydziału > Ustaw wartości przydziału > kliknij **OK** > kliknij nazwę nowego limitu przydziału.

   ![](media/azure-stack-create-plan/image06.png)
8. Kliknij przycisk **Microsoft.Network (lokalne)**, a następnie wybierz domyślnego przydziału lub kliknij przycisk **Utwórz nowy przydział** dostosować limit przydziału.

    ![](media/azure-stack-create-plan/image07.png)
9. Jeśli tworzysz nowy przydział, wpisz nazwę przydziału > Ustaw wartości przydziału > kliknij **OK** > kliknij nazwę nowego limitu przydziału.

    ![](media/azure-stack-create-plan/image08.png)
10. Kliknij przycisk **Microsoft.Compute (lokalne)**, a następnie wybierz domyślnego przydziału lub kliknij przycisk **Utwórz nowy przydział** dostosować limit przydziału.

    ![](media/azure-stack-create-plan/image09.png)
11. Jeśli tworzysz nowy przydział, wpisz nazwę przydziału > Ustaw wartości przydziału > kliknij **OK** > kliknij nazwę nowego limitu przydziału.

    ![](media/azure-stack-create-plan/image10.png)
12. W **przydziały** bloku, kliknij przycisk **OK**, a następnie w **nowy Plan** bloku, kliknij przycisk **Utwórz** Aby utworzyć plan.

    ![](media/azure-stack-create-plan/image11.png)
13. Aby wyświetlić nowy plan, kliknij przycisk **wszystkie zasoby**, następnie wyszukaj plan i kliknij jego nazwę.

    ![](media/azure-stack-create-plan/image12.png)

### <a name="next-steps"></a>Kolejne kroki
[Tworzenie oferty](azure-stack-create-offer.md)
