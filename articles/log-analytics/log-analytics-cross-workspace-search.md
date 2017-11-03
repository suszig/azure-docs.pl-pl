---
title: "Wykonywania zapytań między obszarami roboczymi Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak można zbadać między wiele obszarów roboczych w ramach subskrypcji wraz z przykładami, które należy wykonać."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: magoedte
ms.openlocfilehash: 6b6dc6f472a87178c006301f4f692aeff15e257e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="perform-queries-across-multiple-log-analytics-workspaces"></a>Wykonywania zapytań przez wiele obszarów roboczych analizy dzienników

Wcześniej z usługi Analiza dzienników Azure, możesz można tylko analizować dane w bieżącym obszarze roboczym i to ograniczona możliwość zapytania przez wiele obszarów roboczych, zdefiniowane w ramach subskrypcji.  

Teraz można badać między wiele obszarów roboczych, zapewniając systemowe widoku danych.  Można wykonać tylko tego typu zapytań w [portal zaawansowane](log-analytics-log-search-portals.md#advanced-analytics-portal), a nie w portalu Azure.  

## <a name="querying-across-log-analytics-workspaces"></a>Wykonywanie zapytań w obszary robocze analizy dzienników
Aby odwołać się do innego obszaru roboczego w kwerendzie użyć *obszaru roboczego* identyfikator.  Na przykład następujące zapytanie zwraca podsumowanie liczby aktualizacje wymagane przez ich klasyfikacji z tabeli aktualizacji z bieżącym obszarze roboczym i innego obszaru roboczego o nazwie *contosoretail it*.  


## <a name="identifying-resources"></a>Identyfikowanie zasobów
Identyfikowanie obszaru roboczego można wykonać kilka sposobów:

* Nazwa zasobu — jest zrozumiałą nazwę obszaru roboczego, czasami określane jako *nazwa składnika*. 

    `workspace("contosoretail").Update | count`
 
    >[!NOTE]
    >Identyfikowanie obszaru roboczego według jego nazwy przyjęto założenie, że jest on unikatowy we wszystkich dostępnych subskrypcji. Jeśli masz wiele aplikacji o określonej nazwie, zapytanie kończy się niepowodzeniem z powodu niejednoznaczności. W takim przypadku należy użyć jednego z innych identyfikatorów.

* Nazwa kwalifikowana — jest "Pełna nazwa" obszaru roboczego, zawierający nazwę subskrypcji, grupy zasobów i składnik nazwę w następującym formacie: *NazwaSkładnika-Nazwa subskrypcji/resourceGroup*. 

    `workspace('contoso/contosoretail/development').requests | count `

    >[!NOTE]
    >Ponieważ nazwy subskrypcji platformy Azure nie są unikatowe, ten identyfikator może być niejednoznaczna. 
    >

* Identyfikator obszaru roboczego - identyfikator obszaru roboczego jest unikatowy, modyfikować, identyfikator przypisany do każdego obszaru roboczego reprezentowane jako unikatowy identyfikator globalny (GUID).

    `workspace("b438b4f6-912a-46d5-9cb1-b44069212ab4").Update | count`

* Azure Resource ID — definicja Azure unikatową tożsamość obszaru roboczego. Za pomocą to nazwa zasobu jest niejednoznaczna.  Obszary robocze, jest format: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. OperationalInsights/workspaces/NazwaSkładnika*.  

    Na przykład:
    ``` 
    `workspace("/subscriptions/e427267-5645-4c4e-9c67-3b84b59a6982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
    ```

## <a name="next-steps"></a>Następne kroki

Przegląd [analizy dzienników dziennika odwołanie wyszukiwania](https://docs.loganalytics.io/docs/Language-Reference) Aby wyświetlić wszystkie dostępne w analizy dzienników Opcje składni zapytania.    