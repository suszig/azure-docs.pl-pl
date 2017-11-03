---
title: Tworzenie obszaru roboczego w Azure Log Analytics | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak utworzyć obszaru roboczego analizy dzienników, aby umożliwić zarządzanie rozwiązaniami i gromadzenia danych z środowiska chmury i lokalnych."
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.author: magoedte
ms.openlocfilehash: 8259a97d28effa7bfa9cfb9d7cd9cd2a14c9d906
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2017
---
# <a name="create-a-log-analytics-workspace-in-the-azure-portal"></a>Tworzenie obszaru roboczego analizy dzienników w portalu Azure
W portalu Azure, które można ustawić obszaru roboczego analizy dzienników, która jest unikatowym środowisku analizy dzienników z własnych danych repozytorium, źródła danych i rozwiązania.  Kroki opisane w tym artykule są wymagane, jeśli planujesz na zbieranie danych z następujących źródeł:

* Zasoby platformy Azure w ramach subskrypcji
* Lokalnych komputerów monitorowanych przez program System Center Operations Manager
* Kolekcje urządzeń z programu System Center Configuration Manager 
* Dane diagnostyczne lub dziennika z usługi Azure Storage

Dla innych źródeł, takich jak maszynach wirtualnych platformy Azure i komputery z systemem Windows lub Linux w środowisku zobacz następujące tematy:

*  [Zbierz dane o maszynach wirtualnych platformy Azure](log-analytics-quick-collect-azurevm.md) 
*  [Zbiera dane dotyczące komputerów z systemem Linux](log-analytics-quick-collect-linux-computer.md)
*  [Zbiera dane dotyczące komputerów z systemem Windows](log-analytics-quick-collect-windows-computer.md)

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="log-in-to-azure-portal"></a>Zaloguj się do portalu Azure
Zaloguj się do portalu Azure pod adresem [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Tworzenie obszaru roboczego
1. W portalu Azure kliknij **więcej usług** znaleziono w lewym dolnym rogu. Na liście zasobów wpisz **Log Analytics**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Wybierz **dziennika analizy**.<br><br> ![Witryna Azure Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. Kliknij przycisk **Utwórz**, a następnie wybierz opcje dla następujących elementów:

  * Podaj nazwę dla nowego **obszarem roboczym pakietu OMS**, takich jak *DefaultLAWorkspace*. 
  * Wybierz **Subskrypcję** do połączenia poprzez wybór subskrypcji z listy rozwijanej, jeśli domyślnie wybrana subskrypcja jest niewłaściwa.
  * Aby uzyskać **grupy zasobów**, wybierz opcję, aby użyć istniejącego zasobu już grupy konfiguracji lub Utwórz nową.  
  * Wybierz dostępny **lokalizacji**.  Aby uzyskać dodatkowe informacje, sprawdź, która [Log Analytics jest dostępna w regionach](https://azure.microsoft.com/regions/services/).
  * Możesz wybrać spośród trzech różnych **warstw cenowych** w analizy dzienników, ale dla tego przewodnika Szybki Start zamierzasz wybierz **wolnego** warstwy.  Aby uzyskać dodatkowe informacje dotyczące konkretnego warstw, zobacz [szczegóły cennika usługi Analiza dzienników](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-01.png)<br>  
3. Po podaniu wymaganych informacji w **obszarem roboczym pakietu OMS** okienku, kliknij przycisk **OK**.  

Podczas weryfikowania informacji i tworzenia obszaru roboczego możesz śledzić postęp w sekcji **Powiadomienia** z poziomu menu. 

## <a name="next-steps"></a>Następne kroki
Teraz, gdy masz obszaru roboczego, które są dostępne, Konfigurowanie zbierania monitorowania telemetrii, możliwe wyszukiwanie dziennika do analizowania danych i dodać rozwiązania do zarządzania, aby zapewnić dodatkowe dane i informacje analityczne. 

* Aby włączyć zbieranie danych z zasobów platformy Azure z diagnostyki Azure lub usługi Azure storage, zobacz [dzienniki usługi Azure zbieranie i metryk do użycia w analizy dzienników](log-analytics-azure-storage.md).  
* [Dodaj System Center Operations Manager jako źródła danych](log-analytics-om-agents.md) do zbierania danych z agentów raportowania grupę zarządzania programu Operations Manager i zapisz go w repozytorium obszaru roboczego analizy dzienników. 
* Połącz [programu Configuration Manager](log-analytics-sccm.md) do importowania komputery, które są członkami kolekcji w hierarchii.  
* Przegląd [rozwiązań do zarządzania](/log-analytics-add-solutions.md) dostępne i jak dodać lub usunąć rozwiązania z obszaru roboczego.

