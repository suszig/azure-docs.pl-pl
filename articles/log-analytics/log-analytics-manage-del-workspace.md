---
title: "Usuń z obszarem roboczym usługi Analiza dzienników Azure | Dokumentacja firmy Microsoft"
description: "Usuwanie obszaru roboczego analizy dzienników, jeśli został on utworzony w osobistym subskrypcji lub restrukturyzacji modelu obszaru roboczego."
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 3d99f9869dfdfbe18f82e873bd367cc85f9414f1
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="delete-an-azure-log-analytics-workspace-with-the-azure-portal"></a>Usuń z obszarem roboczym usługi Analiza dzienników Azure przy użyciu portalu Azure
W tym temacie pokazano, jak usunąć workpace analizy dzienników, które mogą nie wymagać za pomocą portalu Azure. 

## <a name="to-delete-a-workspace"></a>Aby usunąć obszar roboczy 
Po usunięciu obszaru roboczego analizy dzienników wszystkie dane związane z obszaru roboczego jest usuwane z usługi w ciągu 30 dni.  Należy zachować ostrożność podczas usuwania obszaru roboczego, ponieważ może być ważnych danych i konfiguracji, które może niekorzystnie wpłynąć na operacje usługi.  
 
Jeśli jesteś administratorem i z tym obszarem roboczym jest skojarzonych wielu użytkowników, skojarzenie między tymi użytkownikami i obszarem roboczym zostanie przerwane. Jeśli użytkownicy są skojarzone z innych obszarów roboczych, następnie można przejść za pomocą analizy dzienników z tych obszarów roboczych. Jednak jeśli nie są one powiązane z innych obszarów roboczych następnie muszą utworzyć obszaru roboczego, aby użyć analizy dzienników. 

1. Zaloguj się do [Azure Portal](http://portal.azure.com). 
2. W portalu Azure kliknij **więcej usług** znaleziono w lewym dolnym rogu. Na liście zasobów wpisz **Log Analytics**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Wybierz **dziennika analizy**.
3. W okienku subskrypcje analizy dzienników, wybierz obszar roboczy, a następnie kliknij przycisk **usunąć** od góry w środkowym okienku.<br><br> ![Usuń opcję w okienku właściwości obszaru roboczego](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace.png)<br>  
4. Po wyświetleniu okna komunikatu potwierdzenia prośbą o potwierdzenie usunięcia obszaru roboczego, kliknij przycisk **tak**.<br><br> ![Potwierdzanie usunięcia obszaru roboczego](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace-confirm.png)

