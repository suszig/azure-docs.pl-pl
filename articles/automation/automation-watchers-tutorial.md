---
title: "Tworzenie zadania obserwatora w ramach konta usługi Automatyzacja Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć zadanie obserwatora w ramach konta usługi Automatyzacja Azure, aby obejrzeć dla nowych plików utworzonych w folderze."
services: automation
documentationcenter: 
author: eamonoreilly
manager: 
editor: 
ms.assetid: 0dd95270-761f-448e-af48-c8b1e82cd821
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/15/2017
ms.author: eamono
ms.openlocfilehash: 0ddd31f7ce2217c1136eccd391bb30bd4461c3e5
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/22/2017
---
# <a name="azure-automation-watcher-tasks-enable-you-to-respond-to-events-happening-in-your-local-datacenter"></a>Zadania obserwatora usługi Automatyzacja Azure pozwala na odpowiadanie na zdarzenia wykonywane w lokalnym centrum danych

Z tego samouczka dowiesz się sposób tworzenia nowego zadania obserwatora:

> [!div class="checklist"]
> * Utworzenie elementu runbook obserwatora, który wyszukuje nowe pliki w katalogu.
> * Utwórz zmienną automatyzacji do zachowania podczas ostatniego przetwarzania plików przez obserwatora.
> * Utwórz element runbook akcji, które jest wywoływane, gdy element runbook obserwatora znajdzie nowy plik.
> * Tworzenie zadania obserwatora wybiera obserwatora runbook i działania elementu runbook.
> * Wyzwalanie obserwatora przez dodanie nowego pliku do katalogu.
> * Sprawdź, czy dane wyjściowe z elementu runbook akcji, który zawiera informacje na nowym pliku.  

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka wymagane są następujące elementy.
+ Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
+ [Konto automatyzacji](automation-offering-get-started.md) do przechowywania obserwatora i działań elementów runbook i zadania obserwatora.
+ A [hybrydowy proces roboczy elementu runbook](automation-hybrid-runbook-worker.md) gdzie uruchomieniu zadania obserwatora.

## <a name="create-a-watcher-runbook-that-looks-for-new-files"></a>Tworzenie elementu runbook obserwatora, który wyszukuje nowe pliki
1.  Otwórz konta automatyzacji i kliknij na stronie elementy Runbook.
2.  Kliknij przycisk "Przeglądaj Galeria".
![Lista elementów Runbook z interfejsu użytkownika](media/automation-watchers-tutorial/WatcherTasksRunbookList.png)
3.  Wyszukaj "Watch-NewFile" i zaimportuj element runbook do konta automatyzacji.
![Publikowanie elementu runbook z interfejsu użytkownika](media/automation-watchers-tutorial/Watch-NewFileRunbook.png)
4.  Kliknij "Edytuj", aby wyświetlić źródło elementu Runbook, a następnie kliknij przycisk "Publikuj".

## <a name="create-an-automation-variable-to-keep-the-last-time-a-file-was-processed-by-the-watcher"></a>Utwórz zmienną automatyzacji do zachowania podczas ostatniego przetwarzania plików przez obserwatora
1.  Otwórz stronę zmienne w obszarze udostępnionych zasobów, a następnie kliknij przycisk "Dodaj zmiennej" ![listy zmiennych z poziomu interfejsu użytkownika](media/automation-watchers-tutorial/WatcherVariableList.png)
2.  Wprowadź nazwę "Watch-NewFileTimestamp"
3.  Wybierz typ jako element DateTime, a następnie kliknij przycisk "Utwórz".
![Utwórz zmienną znak wodny z poziomu interfejsu użytkownika](media/automation-watchers-tutorial/WatcherWatermarkVariable.png)

## <a name="create-an-action-runbook-that-is-called-when-the-watcher-runbook-finds-a-new-file"></a>Utwórz element runbook akcji, które jest wywoływane, gdy element runbook obserwatora znajdzie nowy plik
1.  Kliknij na stronie elementy Runbook w kategorii "AUTOMATYZACJI procesu".
2.  Kliknij przycisk "Przeglądaj Galeria".
3.  Wyszukaj "Proces NewFile" i zaimportuj element runbook do konta automatyzacji.
4.  Kliknij "Edytuj", aby wyświetlić źródło elementu Runbook, a następnie kliknij przycisk "Publikuj".
![Obserwator proces z poziomu interfejsu użytkownika](media/automation-watchers-tutorial/Watch-ProcessNewFile.png)


## <a name="create-a-watcher-task-that-selects-the-watcher-runbook-and-action-runbook"></a>Tworzenie zadania obserwatora wybiera obserwatora runbook i działania elementu runbook
1.  Otwórz stronę zadania obserwatora, a następnie kliknij przycisk "Dodaj zadania obserwatora".
![Lista obserwatora z poziomu interfejsu użytkownika](media/automation-watchers-tutorial/WatchersList.png)
2.  Wprowadź "WatchMyFolder" jako nazwy.
3.  Wybierz opcję "Konfiguruj obserwatora" i wybierz element runbook "Watch-NewFile".
![Konfigurowanie monitora z poziomu interfejsu użytkownika](media/automation-watchers-tutorial/ConfigureWatcher.png)
4.  Wpisz następujące wartości parametrów:
    *   FOLDERPATH. Folder na hybrydowy proces roboczy, w której tworzone nowe pliki
    *   ROZSZERZENIE. Pozostaw pole puste, aby przetworzyć wszystkie rozszerzenia pliku.
    *   RECURSE. Pozostaw wartość domyślną.
    *   USTAWIENIA URUCHOMIENIA. Wybierz hybrydowy proces roboczy.
5.  Kliknij przycisk OK, a następnie wybierz, aby powrócić do strony obserwatora.
6.  Wybierz opcję "Konfiguruj Akcja" i wybierz element runbook "Proces NewFile".
![Konfigurowanie akcji obserwatora z poziomu interfejsu użytkownika](media/automation-watchers-tutorial/ConfigureAction.png)
7.  Wpisz następujące wartości parametrów:
    *   EVENTDATA. Pozostaw to pole puste. Dane są przekazywane w z elementu runbook obserwatora.
    *   Ustawienia uruchomienia. Pozostaw Azure podczas działania tego elementu runbook usługi Automatyzacja.
8.  Kliknij przycisk OK, a następnie wybierz, aby powrócić do strony obserwatora.
9.  Kliknij przycisk OK, aby utworzyć zadanie obserwatora.

## <a name="trigger-a-watcher-by-adding-a-new-file-to-a-directory"></a>Wyzwalacz obserwatora przez dodanie nowego pliku do katalogu
1.  Zdalne w hybrydowego procesu roboczego
2.  Dodaj nowy plik tekstowy do folderu, który jest monitorowany przez zadania obserwatora.

## <a name="inspect-the-output-from-the-action-runbook-that-shows-information-on-the-new-file"></a>Sprawdź dane wyjściowe z elementu runbook akcji, który zawiera informacje na nowym pliku
1.  Kliknij zadanie obserwatora dla "WatchMyFolder"
2.  Wybierz polecenie "Wyświetl obserwatora strumieni" czy obserwatora znaleziono nowy plik i uruchomić akcję elementu runbook.
3.  Wybierz polecenie "Wyświetl obserwatora akcji zadania" Aby zobaczyć akcji zadania elementu runbook.
![Obserwator akcji zadania z poziomu interfejsu użytkownika](media/automation-watchers-tutorial/WatcherActionJobs.png)


## <a name="next-steps"></a>Następne kroki:

Aby uzyskać więcej informacji, zobacz [Moje pierwszego elementu runbook programu PowerShell](automation-first-runbook-textual-powershell.md).








