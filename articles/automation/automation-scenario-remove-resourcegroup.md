---
title: "Automatyczne usuwanie grup zasobów | Microsoft Docs"
description: "Wersja scenariusza użycia usługi Azure Automation obejmująca wykorzystanie przepływu pracy programu PowerShell i elementów Runbook umożliwiających usunięcie wszystkich grupy zasobów w subskrypcji."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: 
ms.assetid: b848e345-fd5d-4b9d-bc57-3fe41d2ddb5c
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: magoedte
ms.openlocfilehash: cb7183cbec1c3efafe58f4508042d329be5dcecf
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="azure-automation-scenario---automate-removal-of-resource-groups"></a>Scenariusz użycia usługi Azure Automation — automatyczne usuwanie grup zasobów
Wielu klientów tworzy więcej niż jedną grupę zasobów. Niektóre grupy mogą służyć do zarządzania aplikacjami produkcyjnymi, a inne mogą być używane jako środowiska rozwojowe, testowe lub przejściowe. Automatyzacja wdrażania tych zasobów to jedno, ale wymagane jest również zapewnienie możliwości łatwej likwidacji grupy zasobów. To typowe zadanie zarządzania można uprościć przy użyciu usługi Azure Automation. Przydaje się to w przypadku korzystania z subskrypcji platformy Azure z limitem wydatków w ramach takich ofert jak MSDN lub program Microsoft Partner Network Cloud Essentials.

Ten scenariusz jest oparty na elemencie Runbook programu PowerShell i jest przeznaczony do usuwania określonych grup zasobów z subskrypcji. Domyślne ustawienie elementu Runbook powoduje testowanie przed wykonaniem. Daje to gwarancję, że nie można przypadkowo usunąć grupy zasobów przed przygotowaniem wszystkiego do wykonania tej procedury.   

## <a name="getting-the-scenario"></a>Uzyskiwanie scenariusza
Ten scenariusz składa się z elementu Runbook programu PowerShell, który można pobrać z [Galerii programu PowerShell](https://www.powershellgallery.com/packages/Remove-ResourceGroup/1.0/DisplayScript). Można również zaimportować go bezpośrednio z [Galerii elementów Runbook](automation-runbook-gallery.md) w witrynie Azure Portal.<br><br>

| Element Runbook | Opis |
| --- | --- |
| Remove-ResourceGroup |Umożliwia usunięcie z subskrypcji grup zasobów (jednej lub wielu) platformy Azure wraz z powiązanymi zasobami. |

<br>
Dla tego elementu Runbook zdefiniowano następujące parametry wejściowe:

| Parametr | Opis |
| --- | --- |
| NameFilter (Filtr nazw) (wymagany) |Określa filtr nazw w celu ograniczenia grup zasobów, które mają zostać usunięte. Można przekazać wiele wartości za pomocą listy rozdzielanej przecinkami.<br>Filtr nie jest rozróżniana wielkość liter i dopasowuje dowolną grupę zasobów, który zawiera ciąg. |
| PreviewMode (Tryb podglądu) (opcjonalny) |Umożliwia uruchomienie elementu Runbook w celu sprawdzenia, które grupy zasobów zostaną usunięte, ale bez podejmowania żadnych działań.<br>Wartość domyślna równa **true** (prawda) pozwala zapobiec przypadkowemu usunięciu jednej lub większej liczby grup zasobów przekazanych do elementu Runbook. |

## <a name="install-and-configure-this-scenario"></a>Instalowanie i konfigurowanie tego scenariusza
### <a name="prerequisites"></a>Wymagania wstępne
Uwierzytelnianie elementu Runbook odbywa się przy użyciu [konta Uruchom jako platformy Azure](automation-sec-configure-azure-runas-account.md).    

### <a name="install-and-publish-the-runbooks"></a>Instalowanie i publikowanie elementów Runbook
Po pobraniu elementu Runbook można go zaimportować za pomocą [procedury importowania elementów Runbook](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation). Po pomyślnym zaimportowaniu elementu Runbook do konta usługi Automation należy go opublikować.

## <a name="using-the-runbook"></a>Używanie elementu Runbook
W poniższych krokach objaśniono przez wykonanie tego elementu runbook i pomocy należy zapoznać się z jej działania. Podczas testowania elementu runbook w tym przykładzie rzeczywiście usunięcie grupy zasobów.  

1. W witrynie Azure Portal otwórz konto usługi Automation i kliknij kafelek **Elementy Runbook**.
2. Wybierz element Runbook **Remove-ResourceGroup** i kliknij pozycję **Uruchom**.
3. Po uruchomieniu elementu runbook, **Uruchom element Runbook** zostanie otwarta strona i można skonfigurować parametrów. Wprowadź nazwy grup zasobów w ramach subskrypcji, można użyć do testowania, powoduje żadnych problemów, jeśli przypadkowo usunięte.

   > [!NOTE]
   > Aby uniknąć usunięcia wybranych grup zasobów, upewnij się, że parametr **Previewmode** (Tryb podglądu) ma wartość **true** (prawda). Ten element runbook nie powoduje usunięcia grupy zasobów, która zawiera konta automatyzacji, który jest uruchomiony ten element runbook.  
   >
   >
1. Po skonfigurowaniu wszystkich wartości parametrów kliknij przycisk **OK**. Element Runbook zostanie umieszczony w kolejce do wykonania.  

Aby wyświetlić szczegóły **ResourceGroup Usuń** runbook zadania w portalu Azure w obszarze **zasobów**, wybierz pozycję **zadania** w elemencie runbook. Wybierz zadania, które chcesz wyświetlić. W podsumowaniu zadania są wyświetlane parametry wejściowe i dane ze strumienia wyjściowego oraz ogólne informacje o zadaniu wraz z wygenerowanymi wyjątkami.<br> ![Stan zadania elementu Runbook Remove-ResourceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-status.png).

W obszarze **Podsumowanie zadania** są widoczne ostrzeżenia i komunikaty o błędach oraz dane ze strumienia wyjściowego. Kliknij kafelek **Dane wyjściowe**, aby wyświetlić szczegółowe wyniki wykonania elementu Runbook.<br> ![Dane wyjściowe elementu Runbook Remove-ResourceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-output.png)

## <a name="next-steps"></a>Kolejne kroki
* Aby rozpocząć tworzenie własnego elementu Runbook, zobacz artykuł [Creating or importing a runbook in Azure Automation](automation-creating-importing-runbook.md) (Tworzenie lub importowanie elementu Runbook w usłudze Azure Automation).
* Aby rozpocząć pracę z elementami Runbook przepływu pracy programu PowerShell, zobacz artykuł [Mój pierwszy element Runbook przepływu pracy programu PowerShell](automation-first-runbook-textual.md).
