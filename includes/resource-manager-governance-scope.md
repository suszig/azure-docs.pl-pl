---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 7a0e7e19d53d8566b859004c363ec5bbc71cb733
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
Przed utworzeniem jakichkolwiek elementów, umożliwia przegląd koncepcji zakresu. Platforma Azure udostępnia cztery poziomy zarządzanie: grup zarządzania, subskrypcji, grupy zasobów i zasobów. [Grupy zarządzania](../articles/billing/billing-enterprise-mgmt-group-overview.md) znajdują się w wersji zapoznawczej. Na poniższej ilustracji przedstawiono przykład tych warstw.

![Zakres](./media/resource-manager-governance-scope/scope-levels.png)

Zastosowanie ustawień zarządzania na każdym z tych poziomów zakresu. Wybrany poziom określa, jak bardzo ustawienie jest stosowane. Niższe poziomy dziedziczyć ustawień z wyższego poziomu. Po zastosowaniu do subskrypcji, to ustawienie jest stosowane do wszystkich grup zasobów i zasobów w ramach subskrypcji. Zastosuj ustawienia w grupie zasobów, że ustawienie stosowane w przypadku grupy zasobów i wszystkie jego zasoby. Jednak innej grupie zasobów nie ma tego ustawienia.

Zwykle warto stosować ustawienia krytyczne na wyższych poziomach i wymagań specyficznych dla projektu na niższych poziomach. Można na przykład upewnij się, że wszystkie zasoby w Twojej organizacji są wdrażane w pewnych regionach. To wymaganie można spełnić, należy zastosować zasady do subskrypcji, która określa dozwolonych lokalizacji. Inni użytkownicy w organizacji dodawania nowej grupy zasobów i zasoby, automatycznie są wymuszane dozwolonych lokalizacji. Z określonego projektu można upewnij się, że wszystkie zasoby są oznaczone za pomocą tagów, które identyfikuje projektu. Zastosuj zasady do grupy zasobów dla projektu, który wymusza stosowanie tego tagu. Użytkownicy dodają nowe zasoby do grupy zasobów, automatycznie są dodawane tagu. Jednak użytkownicy, można dodać grupy zasobów, które nie są związane z projektu i tag nie jest wymuszana.
