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
ms.openlocfilehash: 001bdf20f1d8756e63f15c68141aa415c000070e
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
Podczas wdrażania zasobów na platformie Azure, masz bardzo elastyczne, podczas podejmowania decyzji o jakich typów zasobów, aby wdrożyć, gdzie znajdują się oraz jak je skonfigurować. Jednak tego elastyczność może otworzyć więcej opcji niż chcesz zezwolić w firmie. Podczas planowania, wdrażania zasobów na platformie Azure, może być zastanawiasz się:

* Sposób spełnia wymogi prawne dla suwerenności danych w niektórych krajach?
* Metody kontrolowania kosztów?
* Jak upewnij, że ktoś nie mogą przypadkowo zmieniają krytyczne systemu
* Jak śledzenie kosztów zasobów i dokładnie naliczać opłaty?

W tym artykule opisano te pytania. W szczególności należy:

* Przypisywanie użytkowników do ról i przypisz role do zakresu, dlatego użytkownicy mają uprawnienia do wykonywania akcji oczekiwany, ale nie więcej akcji.
* Tag zasobów, co pozwala na śledzenie ich przez wartości odpowiednich dla danej organizacji.
* Zastosuj zasady, które określają konwencje dotyczące zasobów w ramach subskrypcji.
* Blokowanie zasobów, które są niezbędne do systemu.

Ten artykuł skupia się na zadaniach, które należy wykonać, aby wdrożyć ładu. Szersze omówienie pojęć, zobacz [ładu na platformie Azure](../articles/security/governance-in-azure.md). 
