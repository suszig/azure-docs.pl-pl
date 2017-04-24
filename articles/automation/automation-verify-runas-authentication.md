---
title: "Sprawdzanie poprawności konfiguracji konta usługi Azure Automation | Microsoft Docs"
description: "W tym artykule został opisany sposób potwierdzania, że konfiguracja konta usługi Automation jest poprawna."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/14/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 5bed2616e15a2e5f52e79d0c28159b568e7a1de3
ms.lasthandoff: 04/15/2017

---

# <a name="test-azure-automation-run-as-account-authentication"></a>Sprawdzanie uwierzytelniania konta Uruchom jako usługi Azure Automation
Po pomyślnym utworzeniu konta usługi Automation możesz wykonać prosty test, aby potwierdzić, że możesz poprawnie przeprowadzić uwierzytelnienie w usłudze Azure Resource Manager lub klasycznym wdrożeniu platformy Azure przy użyciu nowo utworzonego lub zaktualizowanego konta Uruchom jako usługi Automation.    

## <a name="automation-run-as-authentication"></a>Uwierzytelnianie konta Uruchom jako usługi Automation

1. Otwórz utworzone wcześniej konto usługi Automation w witrynie Azure Portal.  
2. Kliknij kafelek **Elementy Runbook**, aby otworzyć listę elementów Runbook.
3. Wybierz element Runbook **AzureAutomationTutorialScript**, a następnie kliknij pozycję **Start**, aby go uruchomić.  Zostanie wyświetlony monit sprawdzający, czy chcesz uruchomić element Runbook.
4. Nastąpi utworzenie [zadania elementu Runbook](automation-runbook-execution.md), wyświetlenie bloku Zadanie oraz wyświetlenie stanu zadania w kafelku **Podsumowanie zadania**.  
5. Zadanie będzie miało początkowy stan *W kolejce*, wskazujący, że trwa oczekiwanie na udostępnienie procesu roboczego elementu Runbook w chmurze. Następnym stanem będzie *Uruchamianie*, gdy proces roboczy wywołuje zadanie, a następnie *Uruchomiono*, gdy element Runbook faktycznie zacznie działać.  
6. Po zakończeniu zadania elementu Runbook powinniśmy zobaczyć stan **Ukończono**.<br> ![Test elementu Runbook zabezpieczeń](media/automation-verify-runas-authentication/job-summary-automationtutorialscript.png)<br>
7. Aby wyświetlić szczegółowe wyniki elementu Runbook, kliknij kafelek **Dane wyjściowe**.
8. W bloku **Dane wyjściowe** powinna być wyświetlana informacja, że element został pomyślnie uwierzytelniony, oraz zwrócona lista wszystkich zasobów dostępnych w tej grupie zasobów.
9. Zamknij blok **Dane wyjściowe**, aby wrócić do bloku **Podsumowanie zadania**.
10. Zamknij blok **Podsumowanie zadania** i odpowiedni blok elementu Runbook **AzureAutomationTutorialScript**.

## <a name="classic-run-as-authentication"></a>Uwierzytelnianie klasycznego konta Uruchom jako
Jeśli będziesz zarządzać zasobami w klasycznym modelu wdrażania, aby potwierdzić, że możesz poprawnie przeprowadzić uwierzytelnienie za pomocą nowego klasycznego konta Uruchom jako, musisz wykonać następujące czynności.     

1. Otwórz utworzone wcześniej konto usługi Automation w witrynie Azure Portal.  
2. Kliknij kafelek **Elementy Runbook**, aby otworzyć listę elementów Runbook.
3. Wybierz element Runbook **AzureClassicAutomationTutorialScript**, a następnie kliknij pozycję **Start**, aby go uruchomić.  Zostanie wyświetlony monit sprawdzający, czy chcesz uruchomić element Runbook.
4. Nastąpi utworzenie [zadania elementu Runbook](automation-runbook-execution.md), wyświetlenie bloku Zadanie oraz wyświetlenie stanu zadania w kafelku **Podsumowanie zadania**.  
5. Zadanie będzie miało początkowy stan *W kolejce*, wskazujący, że trwa oczekiwanie na udostępnienie procesu roboczego elementu Runbook w chmurze. Następnym stanem będzie *Uruchamianie*, gdy proces roboczy wywołuje zadanie, a następnie *Uruchomiono*, gdy element Runbook faktycznie zacznie działać.  
6. Po zakończeniu zadania elementu Runbook powinniśmy zobaczyć stan **Ukończono**.<br><br> ![Test elementu Runbook zabezpieczeń](media/automation-verify-runas-authentication/job-summary-automationclassictutorialscript.png)<br>  
7. Aby wyświetlić szczegółowe wyniki elementu Runbook, kliknij kafelek **Dane wyjściowe**.
8. W bloku **Dane wyjściowe** powinna być wyświetlana informacja, że element został pomyślnie uwierzytelniony, oraz zwrócona lista wszystkich klasycznych maszyn wirtualnych w subskrypcji.
9. Zamknij blok **Dane wyjściowe**, aby wrócić do bloku **Podsumowanie zadania**.
10. Zamknij blok **Podsumowanie zadania** i odpowiedni blok elementu Runbook **AzureClassicAutomationTutorialScript**.

## <a name="next-steps"></a>Następne kroki
* Aby rozpocząć pracę z elementami Runbook programu PowerShell, zobacz artykuł [My first PowerShell runbook](automation-first-runbook-textual-powershell.md) (Mój pierwszy element Runbook programu PowerShell).
* Aby dowiedzieć się więcej na temat tworzenia elementów graficznych, zobacz [Graphical authoring in Azure Automation](automation-graphical-authoring-intro.md) (Tworzenie elementów graficznych w usłudze Azure Automation).

