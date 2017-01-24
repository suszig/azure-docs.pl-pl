---
title: "Obsługa błędów w graficznych elementach Runbook w usłudze Azure Automation | Microsoft Docs"
description: "W tym artykule został opisany sposób implementacji logiki obsługi błędów w graficznych elementach Runbook w usłudze Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/26/2016
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: f9b359691122da9e5d93e51f3085cad51e55d8f2
ms.openlocfilehash: 13c3e1693badcf4148738cb63666f34546d1696c

---

# <a name="error-handling-in-azure-automation-graphical-runbooks"></a>Obsługa błędów w graficznych elementach Runbook w usłudze Azure Automation

Kluczową zasadą projektu elementu Runbook, którą należy wziąć pod uwagę, jest ustalenie różnych problemów, które mogą wystąpić w elemencie Runbook, takich jak powodzenie, oczekiwane stany błędu i nieoczekiwane warunki błędu.  Elementy Runbook powinny zawierać odpowiednią obsługę wykrywania błędów.  W przypadku graficznych elementów Runbook, jeżeli chcesz sprawdzić poprawność danych wyjściowych działania lub obsłużyć błąd w odpowiedni sposób, prawdopodobnie będziesz postępować zgodnie z działaniami kodu programu PowerShell, definiować logikę warunkową łącza danych wyjściowych działania lub stosować inną metodę.          

Często podczas wykonywania elementów Runbook, jeśli istnieje niepowodujący zakończenia błąd występujący wraz z działaniem, wszelkie działania, które nastąpią po nim, zostaną niezależnie przetworzone.  Oczywiście prawdopodobnie zostanie wygenerowany wyjątek, ale chodzi o to, że będzie można wykonać następne działanie. Przyczyną takiego zachowania jest sposób zaprojektowania języka PowerShell do obsługi błędów.    

Typami błędów programu PowerShell, które mogą wystąpić podczas wykonywania, są błędy powodujące i niepowodujące zakończenia.  Różnice są następujące:

* Błąd powodujący zakończenie: poważny błąd podczas wykonywania, który całkowicie zatrzymuje polecenie (lub wykonywanie skryptu). Do przykładów mogą należeć nieistniejące polecenia cmdlet, błędy składniowe, które mogłyby uniemożliwiać wykonanie polecenia cmdlet, lub inne błędy krytyczne.

* Błąd niepowodujący zakończenia: niezbyt poważny błąd, który umożliwia kontynuowanie wykonywania pomimo awarii. Do przykładów należą błędy operacyjne, takie jak niemożność odnalezienia pliku, problemy z uprawnieniami itd.

Graficzne elementy Runbook usługi Azure Automation zostały ulepszone dzięki zdolności do uwzględnienia obsługi błędu. Dzięki temu można teraz przekształcić wyjątki w błędy niepowodujące zakończenia oraz tworzyć łącza błędów między działaniami. Umożliwia to autorowi elementu Runbook wychwytywanie błędów i określenie ścieżki do zarządzania zrealizowanym lub nieoczekiwanym warunkiem.  

## <a name="when-to-use"></a>Kiedy stosować

Ważne jest zapewnienie kontrolowania wykonywania przepływu pracy zawsze, gdy występuje krytyczne działanie, które zgłasza błąd lub wyjątek. Możesz wtedy zapobiec przejściu do następnego działania w elemencie Runbook i odpowiednio obsłużyć błąd.  Jest to konieczne zwłaszcza, gdy Twoje elementy Runbook obsługują biznes lub proces operacji usługi.

Do każdego działania, które może powodować błąd, autor elementu Runbook może dodać łącze błędu wskazujące na dowolne inne działanie.  Działanie docelowe może należeć do dowolnego typu: działanie kodu, wywoływanie polecenia cmdlet, wywoływanie innego elementu Runbook lub cokolwiek innego. 

Ponadto działanie docelowe może także zawierać łącza wychodzące, które mogą być łączami zwykłymi lub łączami błędu. Dzięki czemu autor elementu Runbook może zaimplementować kompleksową logikę obsługi błędu bez konieczności uciekania się do uwzględniania działania kodu.  Gdy zalecaną praktyką jest tworzenie elementu Runbook dedykowanego do obsługi błędów i mającego często spotykaną funkcjonalność, nie jest to obowiązkowe i logika obsługi błędów w programie PowerShell nie jest jedyną alternatywą.  

Na przykład weźmy pod uwagę element Runbook, który próbuje uruchomić maszynę wirtualną i zainstalować na niej aplikację, ale jeśli maszyna wirtualna nie zostanie uruchomiona poprawnie, wykona on dwie czynności: 

1. Wyśle powiadomienie o tym problemie.
2. Uruchomi inny element Runbook, który zamiast tego automatycznie zainicjuje nową maszynę wirtualną. 

Jedno rozwiązanie może polegać na utworzeniu łącza błędu do działania obsługującego krok 1, takiego jak polecenie cmdlet **Write-Warning**, połączonego z działaniem dla kroku 2, takiego jak polecenie cmdlet **Start AzureRmAutomationRunbook**. 

Możesz również uogólnić to zachowanie do zastosowania w wielu elementach Runbook i umieścić te dwa działania w oddzielnym elemencie Runbook obsługującym błędy zgodnie ze wskazówkami podanymi wcześniej w tym temacie.  Przed wywołaniem tego elementu Runbook obsługującego błędy możesz skonstruować niestandardowy komunikat na podstawie danych w oryginalnym elemencie Runbook, a następnie przekazać go jako parametr do elementu Runbook obsługującego błędy. 

## <a name="how-to-use"></a>Jak stosować

Każde działanie ma konfigurację umożliwiającą przekształcenie wyjątków w błędy niepowodujące zakończenia. Ta opcja jest domyślnie wyłączona.  Należy ją włączyć dla każdego działania, które ma obsługiwać błędy.  Przez włączenie tej konfiguracji zapewniasz, że zarówno błędy powodujące, jak i niepowodujące zakończenia w działaniu będą obsługiwane jako błędy niepowodujące zakończenia, a następnie mogą zostać obsłużone za pomocą łącza błędu.  Po skonfigurowaniu tego ustawienia możesz utworzyć działania, które będą obsługiwać błędy.  Jeśli działanie powoduje jakikolwiek błąd, to następne działanie będzie zgodne z wychodzącymi łączami błędu, a nie z normalnymi łączami nawet wtedy, gdy działanie utworzyło również normalne dane wyjściowe.<br><br> ![Przykład łącza błędu elementu Runbook automatyzacji](media/automation-runbook-graphical-error-handling/error-link-example.png)

W poniższym prostym przykładzie mamy element Runbook, który pobiera zmienną zawierającą nazwę komputera maszyny wirtualnej, a następnie próbuje uruchomić maszynę wirtualną w ramach następnego działania.<br><br> ![Przykład obsługi błędu elementu Runbook automatyzacji](media/automation-runbook-graphical-error-handling/runbook-example-error-handling.png)<br><br>      

Działanie **Get-AutomationVariable** i **Start AzureRmVm** są skonfigurowane do przekształcania wyjątków w błędy.  Jeśli wystąpią problemy z pobraniem zmiennej lub uruchomieniem maszyny wirtualnej, zostaną wygenerowane błędy.<br><br> ![Ustawienia działania obsługi błędu elementu Runbook automatyzacji](media/automation-runbook-graphical-error-handling/activity-blade-convertexception-option.png)

Łącza błędów przepływają z tych działań do pojedynczego działania **Zarządzanie błędami** (działania kodu), które jest skonfigurowane za pomocą prostego wyrażenia programu PowerShell przy użyciu słowa kluczowego *Throw*, aby zatrzymać przetwarzanie, oraz *$Error.Exception.Message*, aby uzyskać komunikat, który opisuje bieżący wyjątek.<br><br> ![Przykład kodu obsługi błędu elementu Runbook automatyzacji](media/automation-runbook-graphical-error-handling/runbook-example-error-handling-code.png)


## <a name="next-steps"></a>Następne kroki

* Aby dowiedzieć się więcej o łączach i zrozumieć typy łączy w graficznych elementach Runbook, zobacz [Graphical Authoring in Azure Automation](automation-graphical-authoring-intro.md#links-and-workflow) (Tworzenie graficzne w usłudze Azure Automation).

* Aby dowiedzieć się więcej o wykonywaniu elementów Runbook, sposobie monitorowania zadań elementów Runbook i innych szczegółach technicznych, zobacz [Track a runbook job](automation-runbook-execution.md) (Śledzenie zadania elementu Runbook) 


<!--HONumber=Jan17_HO1-->


