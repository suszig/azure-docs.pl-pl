---
title: "Wykonywania zapytań między obszarami roboczymi Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak można zbadać w wielu obszarach roboczych i specyficzne dla aplikacji usługi App Insights w ramach subskrypcji."
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
ms.date: 02/15/2018
ms.author: magoedte
ms.openlocfilehash: 403448995c28ff7172d2c3abbf3b9d67341017b4
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-perform-queries-across-multiple-log-analytics-workspaces"></a>Jak wykonywać zapytania w wielu obszarów roboczych analizy dzienników

Wcześniej z usługi Analiza dzienników Azure, użytkownik może tylko analizować dane w bieżącym obszarze roboczym i możliwość zapytania ono ograniczone przez wiele obszarów roboczych, zdefiniowane w ramach subskrypcji.  

Teraz można badać nie tylko przez wiele obszarów roboczych usługi Analiza dzienników, ale także dane z określonej aplikacji usługi Application Insights w tej samej grupie zasobów, w innej grupie zasobów lub w innej subskrypcji. Udostępnia systemowe widoku danych.  Można wykonać tylko tego typu zapytań w [portal zaawansowane](log-analytics-log-search-portals.md#advanced-analytics-portal), a nie w portalu Azure.  

## <a name="querying-across-log-analytics-workspaces-and-from-application-insights"></a>Wykonywanie zapytania między obszarami roboczymi analizy dzienników i z usługi Application Insights
Aby odwołać się do innego obszaru roboczego w kwerendzie użyć [ *obszaru roboczego* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/workspace()) identyfikator i dla aplikacji z usługi Application Insights, użyj [ *aplikacji* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/app())identyfikator.  

Na przykład pierwsza kwerenda zwraca podsumowanie liczby aktualizacje wymagane przez ich klasyfikacji z tabeli aktualizacji z bieżącym obszarze roboczym i innego obszaru roboczego o nazwie *contosoretail it*.  W drugim przykładzie zapytanie zwraca podsumowanie liczba żądań wysyłanych z aplikacji o nazwie *fabrikamapp* w usłudze Application Insights. 

### <a name="identifying-workspace-resources"></a>Identyfikowanie zasobów obszaru roboczego
Identyfikowanie obszaru roboczego można wykonać kilka sposobów:

* Nazwa zasobu — jest zrozumiałą nazwę obszaru roboczego, czasami określane jako *nazwa składnika*. 

    `workspace("contosoretail").Update | count`
 
    >[!NOTE]
    >Identyfikowanie obszaru roboczego według jego nazwy przyjęto założenie, że jest on unikatowy we wszystkich dostępnych subskrypcji. Jeśli masz wiele aplikacji o określonej nazwie, zapytanie kończy się niepowodzeniem z powodu niejednoznaczności. W takim przypadku należy użyć jednego z innych identyfikatorów.

* Nazwa kwalifikowana — jest "Pełna nazwa" obszaru roboczego, zawierający nazwę subskrypcji, grupy zasobów i nazwy składnika w następującym formacie: *NazwaSkładnika-Nazwa subskrypcji/resourceGroup*. 

    `workspace('contoso/contosoretail/development').requests | count `

    >[!NOTE]
    >Ponieważ nazwy subskrypcji platformy Azure nie są unikatowe, ten identyfikator może być niejednoznaczna. 
    >

* Identyfikator obszaru roboczego - identyfikator obszaru roboczego jest unikatowy, modyfikować, identyfikator przypisany do każdego obszaru roboczego reprezentowane jako unikatowy identyfikator globalny (GUID).

    `workspace("b438b4f6-912a-46d5-9cb1-b44069212ab4").Update | count`

* Azure Resource ID — definicja Azure unikatową tożsamość obszaru roboczego. Identyfikator zasobu jest używany, gdy nazwa zasobu jest niejednoznaczna.  Obszary robocze, jest format: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. OperationalInsights/workspaces/NazwaSkładnika*.  

    Na przykład:
    ``` 
    workspace("/subscriptions/e427267-5645-4c4e-9c67-3b84b59a6982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
    ```

### <a name="identifying-an-application"></a>Identyfikowanie aplikacji

Identyfikowanie aplikacji w usłudze Application Insights może odbywać się przy użyciu *app(Identifier)* wyrażenia.  *Identyfikator* argument określa aplikacji przy użyciu jednej z następujących czynności:

* Nazwa zasobu — jest człowieka dla użytkownika nazwę aplikacji, czasami określane jako *nazwa składnika*.  

    `app("fabrikamapp")`

* Nazwa kwalifikowana — jest "Pełna nazwa" aplikacja składa się z nazwy subskrypcji, grupy zasobów i nazwy składnika w następującym formacie: *NazwaSkładnika-Nazwa subskrypcji/resourceGroup*. 

    `app("AI-Prototype/Fabrikam/fabrikamapp").requests | count`

     >[!NOTE]
    >Ponieważ nazwy subskrypcji platformy Azure nie są unikatowe, ten identyfikator może być niejednoznaczna. 
    >

* ID — identyfikator GUID aplikacji aplikacji.

    `app("b438b4f6-912a-46d5-9cb1-b44069212ab4").requests | count`

* Azure identyfikator zasobu - zdefiniowane Azure unikatową tożsamość aplikacji. Identyfikator zasobu jest używany, gdy nazwa zasobu jest niejednoznaczna. Format: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. NazwaSkładnika OperationalInsights/składniki*.  

    Na przykład:
    ```
    app("/subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
    ```

## <a name="next-steps"></a>Kolejne kroki

Przegląd [analizy dzienników dziennika odwołanie wyszukiwania](https://docs.loganalytics.io/docs/Language-Reference) Aby wyświetlić wszystkie dostępne w analizy dzienników Opcje składni zapytania.    