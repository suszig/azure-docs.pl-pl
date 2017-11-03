---
title: "Zarządzanie uprawnieniami do zasobów dla każdego użytkownika w stosie Azure (administrator usługi i dzierżawcy) | Dokumentacja firmy Microsoft"
description: "Jako administrator usługi lub dzierżawy informacje o sposobie zarządzania uprawnieniami RBAC."
services: azure-stack
documentationcenter: 
author: Heathl17
manager: byronr
editor: 
ms.assetid: cccac19a-e1bf-4e36-8ac8-2228e8487646
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: e558f9de9bc3182bbe20ceb9d8f3f96e47fa542c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-role-based-access-control"></a>Zarządzanie kontrolą dostępu opartą na rolach

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Użytkownik w stosie Azure może być czytnika, właściciela lub współautora dla każdego wystąpienia subskrypcji, grupy zasobów lub usługi. Na przykład użytkownik A może czytnika uprawnień do subskrypcji 1, ale uprawnień właściciela do 7 maszyny wirtualnej.

* Czytnik: Użytkownika mogą przeglądać wszystko, ale nie można wprowadzać żadnych zmian.
* Współautor: Użytkownik może zarządzać wszystkim poza dostępem do zasobów.
* Właściciel: Użytkownik może zarządzać wszystkim łącznie z dostępem do zasobów.

## <a name="set-access-permissions-for-a-user"></a>Ustaw uprawnienia dostępu dla użytkownika
1. Zaloguj się przy użyciu konta mającego uprawnienia właściciela zasobu, z którym chcesz zarządzać.
2. W bloku zasobu, kliknij **dostępu** ikona ![](media/azure-stack-manage-permissions/image1.png).
3. W **użytkowników** bloku, kliknij przycisk **ról**.
4. W **ról** bloku, kliknij przycisk **Dodaj** się dodanie zezwoleń użytkownika.

## <a name="next-steps"></a>Następne kroki
[Dodawanie dzierżawy usługi Azure Stack](azure-stack-add-new-user-aad.md)

