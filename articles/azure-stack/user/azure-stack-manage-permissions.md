---
title: "Zarządzanie uprawnieniami do zasobów dla każdego użytkownika w stosie Azure | Dokumentacja firmy Microsoft"
description: "Jako administrator usługi lub dzierżawy informacje o sposobie zarządzania uprawnieniami RBAC."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: cccac19a-e1bf-4e36-8ac8-2228e8487646
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: dfec5648ff383fd98965c1918cdab6472bb3c17f
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/22/2018
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

## <a name="next-steps"></a>Kolejne kroki


