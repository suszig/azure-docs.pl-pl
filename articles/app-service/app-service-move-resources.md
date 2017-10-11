---
title: "Przenoszenie zasobów aplikacji sieci Web w innej grupie zasobów"
description: "W tym artykule opisano scenariusze, w których można przenosić aplikacje sieci Web i aplikacji usługi z jednej grupy zasobów do innego."
services: app-service
documentationcenter: 
author: ZainRizvi
manager: erikre
editor: 
ms.assetid: 22f97607-072e-4d1f-a46f-8d500420c33c
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: zarizvi
ms.openlocfilehash: 1b5059dc052005b6079f70ecf6771a3771df8d87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="supported-move-configurations"></a>Przenieś obsługiwane konfiguracje
Można przenieść zasobów aplikacji sieci Web platformy Azure przy użyciu [API zasobów Przenieś Menedżera zasobów](../azure-resource-manager/resource-group-move-resources.md).

Aplikacje sieci Web platformy Azure obsługuje obecnie następujące scenariusze przenoszenia:

* Przenieś całą zawartość grupy zasobów (aplikacje sieci web, planów usługi aplikacji i certyfikatów) do innej grupy zasobów. 
   > [!Note]
   > Docelowej grupy zasobów nie może zawierać żadnych zasobów Microsoft.Web w tym scenariuszu.

* Przenieś aplikacje sieci web poszczególnych do innej grupie zasobów, podczas nadal hosting je w ich bieżący plan usługi app service (plan usługi aplikacji pozostaje w starym grupy zasobów).


