---
title: "Wywoływanie elementu runbook usługi Azure Automation z alertu usługi Log Analytics | Microsoft Docs"
description: "Ten artykuł zawiera omówienie sposobu wywoływania elementu runbook usługi Automation z alertu usługi Microsoft OMS Log Analytics."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/31/2017
ms.author: magoedte
ms.openlocfilehash: 0c0b15f33a177afc70a3662c5bd008eb236ed0d6
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="calling-an-azure-automation-runbook-from-an-oms-log-analytics-alert"></a>Wywoływanie elementu runbook usługi Azure Automation z alertu usługi OMS Log Analytics

W przypadku skonfigurowania w usłudze Log Analytics alertu do tworzenia rekordu alertu, gdy wyniki spełniają kryteria (takie jak długotrwały wzrost użycia procesora lub jeśli proces aplikacji o znaczeniu krytycznym dla funkcjonalności aplikacji biznesowej zakończy się niepowodzeniem i zapisze odpowiadające zdarzenie w dzienniku zdarzeń systemu Windows), alert ten może automatycznie uruchomić element runbook usługi Automation w celu podjęcia próby automatycznego rozwiązania problemu.  

Podczas konfigurowania alertu dostępne są dwie opcje wywołania elementu runbook, takie jak:

1. Skorzystanie z elementu webhook.
   * To jedyna dostępna opcja w przypadku, gdy obszar roboczy pakietu OMS nie został połączony z kontem usługi Automation.
   * Jeśli masz już konto usługi Automation połączone z obszarem roboczym pakietu OMS, ta opcja będzie nadal dostępna.  

2. Bezpośrednie wybranie elementu runbook.
   * Ta opcja jest dostępna tylko po połączeniu obszaru roboczego pakietu OMS z kontem usługi Automation.

## <a name="calling-a-runbook-using-a-webhook"></a>Wywoływanie elementu runbook przy użyciu elementu webhook

Element webhook umożliwia uruchomienie określonego elementu runbook w usłudze Azure Automation za pośrednictwem pojedynczego żądania HTTP. Zanim skonfigurujesz [alert usługi Log Analytics](../log-analytics/log-analytics-alerts.md#alert-rules) do wywoływania elementu runbook za pomocą elementu webhook jako akcji alertu, musisz najpierw utworzyć element webhook dla elementu runbook, który jest wywoływany przy użyciu tej metody. Wykonaj kroki podane w artykule dotyczącym [tworzenia elementu webhook](automation-webhooks.md#creating-a-webhook). Pamiętaj o zapisaniu adresu URL elementu webhook, aby podać go podczas konfigurowania reguły alertu.   

## <a name="calling-a-runbook-directly"></a>Bezpośrednie uruchamianie elementu runbook

Jeśli w obszarze roboczym pakietu OMS masz zainstalowaną i skonfigurowaną usługę Automation & Control, podczas konfigurowania opcji akcji elementu runbook dla alertu możesz wyświetlić wszystkie elementy runbook na liście rozwijanej **Wybierz element runbook** i wybrać konkretny element runbook, który ma być uruchamiany w odpowiedzi na alert. Wybrany element runbook można uruchomić w przestrzeni roboczej w chmurze platformy Azure lub w hybrydowym procesie roboczym elementu runbook. Po utworzeniu alertu przy użyciu opcji elementu runbook dla elementu runbook jest tworzony element webhook. Element webhook można wyświetlić po przejściu do konta usługi Automation i do okienka elementu webhook wybranego elementu runbook. W przypadku usunięcia alertu element webhook nie zostanie usunięty, jednak użytkownik może usunąć element webhook ręcznie. Nieusunięcie elementu webhook nie stanowi problemu — będzie on po prostu elementem oddzielonym, który ostatecznie trzeba będzie usunąć w celu utrzymania uporządkowanego konta usługi Automation.  

## <a name="characteristics-of-a-runbook-for-both-options"></a>Właściwości elementu runbook (w przypadku obu opcji)

Przed skonfigurowaniem reguł alertu należy zrozumieć cechy obu metod wywoływania elementu runbook z poziomu alertu usługi Log Analytics. Dane alertu są zapisane w formacie json w pojedynczej właściwości o nazwie **SearchResult**. Ten format jest używany w przypadku akcji elementów runbook i webhook ze standardowym ładunkiem. W przypadku akcji elementów webhook z niestandardowymi ładunkami, zawierających ciąg **IncludeSearchResults:True** w elemencie **RequestBody**, jest to właściwość **SearchResults**.

* Musisz mieć parametr wejściowy elementu runbook o nazwie **WebhookData**, którego typ to **Obiekt**. Może on być wymagany lub opcjonalny. Przy użyciu tego parametru wejściowego alert przekazuje wyniki wyszukiwania do elementu runbook.

    ```powershell
    param  
        (  
        [Parameter (Mandatory=$true)]  
        [object] $WebhookData  
        )
    ```
*  Musisz mieć kod konwertujący parametr WebhookData na obiekt programu PowerShell.

    ```powershell
    $SearchResult = (ConvertFrom-Json $WebhookData.RequestBody).SearchResult.value
    ```

    Element *$SearchResult* jest tablicą obiektów; każdy obiekt zawiera pola z wartościami z jednego wyniku wyszukiwania


## <a name="example-walkthrough"></a>Przykładowy przewodnik

Pokażemy, jak działa ten proces, korzystając z następującego przykładowego graficznego elementu runbook, który uruchamia usługę systemu Windows.<br><br> ![Graficzny element runbook uruchamiający usługę systemu Windows](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice.png)<br>

Ten element runbook ma jeden parametr wejściowy typu **Obiekt** o nazwie **WebhookData** i zawiera dane elementu webhook przekazane z alertu zawierającego element \*.SearchResult\*.<br><br> ![Parametry wejściowe elementu runbook](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice-inputparameter.png)<br>

Na potrzeby tego przykładu w usłudze Log Analytics utworzyliśmy dwa pola niestandardowe, \*SvcDisplayName\_CF\* i \*SvcState\_CF\*, w celu wyodrębnienia nazwy wyświetlanej usługi i stanu usługi (uruchomiona lub zatrzymana) ze zdarzenia zapisanego w dzienniku zdarzeń systemu. Następnie tworzymy regułę alertu z następującym zapytaniem wyszukiwania: `Type=Event SvcDisplayName_CF="Print Spooler" SvcState_CF="stopped"`, dzięki czemu możemy wykryć, kiedy usługa Print Spooler zostanie zatrzymana w systemie Windows. Może być to dowolna interesująca nas usługa, ale w tym przykładzie odwołujemy się do jednej ze wstępnie istniejących usług zawartych w systemie operacyjnym Windows. Akcja alertu jest skonfigurowana do wykonywania naszego elementu runbook użytego w tym przykładzie i do działania w hybrydowym procesie roboczym elementu runbook, który jest włączany w systemie docelowym.   

Działanie kodu elementu runbook **Get Service Name from LA** konwertuje ciąg w formacie JSON na typ obiektu i stosuje filtr do elementu \*SvcDisplayName\_CF\* w celu wyodrębnienia nazwy wyświetlanej usługi systemu Windows, a następnie przekazuje tę wartość do kolejnego działania, które przed podjęciem próby ponownego uruchomienia usługi sprawdza, czy została zatrzymana. *SvcDisplayName_CF* to [pole niestandardowe](../log-analytics/log-analytics-custom-fields.md) utworzone w usłudze Log Analytics na potrzeby tego przykładu.

```powershell
$SearchResult = (ConvertFrom-Json $WebhookData.RequestBody).SearchResult.value
$SearchResult.SvcDisplayName_CF  
```

Po zatrzymaniu usługi reguła alertu w usłudze Log Analytics wykrywa dopasowanie, po czym wyzwala element runbook i wysyła kontekst alertu do tego elementu. Element runbook podejmuje działanie w celu sprawdzenia, czy usługa została zatrzymana. Jeśli tak, nastąpi próba ponownego uruchomienia usługi. Działanie sprawdzi, czy usługa została uruchomiona prawidłowo, i wyprowadzi wyniki.     

Jeśli nie masz konta usługi Automation połączonego z przestrzenią roboczą pakietu OMS, możesz też skonfigurować regułę alertu z akcją elementu webhook wyzwalającą element runbook i skonfigurować element runbook do konwertowania ciągu w formacie JSON oraz filtrowania elementu \*.SearchResult\* zgodnie z podanymi wcześniej wskazówkami.    

>[!NOTE]
> Jeśli Twój obszar roboczy został uaktualniony do [nowego języka zapytań usługi Log Analytics](../log-analytics/log-analytics-log-search-upgrade.md), ładunek elementu webhook uległ zmianie.  Szczegółowe informacje na temat tego formatu zawiera artykuł [Azure Log Analytics REST API (Interfejs API REST usługi Azure Log Analytics)](https://aka.ms/loganalyticsapiresponse).

## <a name="next-steps"></a>Następne kroki

* Aby dowiedzieć się więcej o alertach w usłudze Log Analytics i o sposobie ich tworzenia, zobacz temat [Alerts in Log Analytics](../log-analytics/log-analytics-alerts.md) (Alerty w usłudze Log Analytics).

* Aby zrozumieć, jak wyzwalać elementy runbook przy użyciu elementu webhook, zobacz artykuł [Elementy webhook w usłudze Azure Automation](automation-webhooks.md).
