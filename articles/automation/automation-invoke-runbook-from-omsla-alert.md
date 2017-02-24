---
title: "Wywoływanie elementu runbook usługi Azure Automation z alertu usługi Log Analytics | Microsoft Docs"
description: "Ten artykuł zawiera omówienie sposobu wywoływania elementu runbook usługi Automation z alertu usługi Microsoft OMS Log Analytics."
services: automation
documentationcenter: 
author: mgoedtel
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
translationtype: Human Translation
ms.sourcegitcommit: 4ce5ad30d79e92a11231313fe13dd42b94fc2aa4
ms.openlocfilehash: 50969591267ca74e5c4d4aa5c1efe5b673498309

---

# <a name="calling-an-azure-automation-runbook-from-an-oms-log-analytics-alert"></a>Wywoływanie elementu runbook usługi Azure Automation z alertu usługi OMS Log Analytics

W przypadku skonfigurowania w usłudze Log Analytics alertu do tworzenia rekordu alertu, gdy wyniki spełniają określone kryteria (takie jak długotrwały wzrost użycia procesora lub jeśli konkretny proces aplikacji o znaczeniu krytycznym dla funkcjonalności aplikacji biznesowej zakończy się niepowodzeniem i zapisze odpowiadające zdarzenie w dzienniku zdarzeń systemu Windows), alert ten może automatycznie uruchomić element runbook usługi Automation w celu podjęcia próby automatycznego rozwiązania problemu.  

Podczas konfigurowania alertu dostępne są dwie opcje wywołania elementu runbook.  Są to:

1. Skorzystanie z elementu webhook.
   * To jedyna dostępna opcja w przypadku, gdy obszar roboczy pakietu OMS nie jest połączony z kontem usługi Automation.
   * Jeśli masz już konto usługi Automation połączone z obszarem roboczym pakietu OMS, ta opcja będzie nadal dostępna.  

2. Bezpośrednie wybranie elementu runbook.
   * Ta opcja jest dostępna tylko po połączeniu obszaru roboczego pakietu OMS z kontem usługi Automation.  

## <a name="calling-a-runbook-using-a-webhook"></a>Wywoływanie elementu runbook przy użyciu elementu webhook

Element webhook umożliwia uruchomienie określonego elementu runbook w usłudze Azure Automation za pośrednictwem pojedynczego żądania HTTP.  Zanim skonfigurujesz [alert usługi Log Analytics](../log-analytics/log-analytics-alerts.md#creating-an-alert-rule) do wywoływania elementu runbook za pomocą elementu webhook jako akcji alertu, musisz najpierw utworzyć element webhook dla elementu runbook, który będzie wywoływany przy użyciu tej metody.  Przejrzyj i wykonaj kroki podane w artykule dotyczącym [tworzenia elementu webhook](automation-webhooks.md#creating-a-webhook). Pamiętaj o zapisaniu adresu URL elementu webhook, aby podać go podczas konfigurowania reguły alertu.   

## <a name="calling-a-runbook-directly"></a>Bezpośrednie uruchamianie elementu runbook

Jeśli w obszarze roboczym pakietu OMS masz zainstalowaną i skonfigurowaną usługę Automation & Control, podczas konfigurowania opcji akcji elementu runbook dla alertu możesz wyświetlić wszystkie elementy runbook na liście rozwijanej **Wybierz element runbook** i wybrać konkretny element runbook, który ma być uruchamiany w odpowiedzi na alert.  Wybrany element runbook można uruchomić w przestrzeni roboczej w chmurze platformy Azure lub w hybrydowym procesie roboczym elementu runbook.  Po utworzeniu alertu przy użyciu opcji elementu runbook dla elementu runbook zostanie utworzony element webhook.  Element webhook można wyświetlić po przejściu do konta usługi Automation i do bloku elementu webhook wybranego elementu runbook.  W przypadku usunięcia alertu element webhook nie zostanie usunięty, jednak użytkownik może usunąć element webhook ręcznie.  Nieusunięcie elementu webhook nie stanowi problemu — będzie on po prostu elementem oddzielonym, który ostatecznie trzeba będzie usunąć w celu utrzymania uporządkowanego konta usługi Automation.  

## <a name="characteristics-of-a-runbook-for-both-options"></a>Właściwości elementu runbook (w przypadku obu opcji)

Obie metody wywoływania elementu runbook z poziomu alertu usługi Log Analytics cechują się innym zachowaniem, które należy zrozumieć przed skonfigurowaniem reguł alertu.  

* Musisz mieć parametr wejściowy elementu runbook o nazwie **WebhookData**, którego typ to **Obiekt**.  Może on być wymagany lub opcjonalny.  Przy użyciu tego parametru wejściowego alert przekazuje wyniki wyszukiwania do elementu runbook.

        param  
         (  
          [Parameter (Mandatory=$true)]  
          [object] $WebhookData  
         )
  
*  Musisz mieć kod konwertujący parametr WebhookData na obiekt programu PowerShell.

    `$SearchResults = (ConvertFrom-Json $WebhookData.RequestBody).SearchResults.value`

    Element *$SearchResults* będzie tablicą obiektów; każdy obiekt zawiera pola z wartościami z jednego wyniku wyszukiwania

### <a name="webhookdata-inconsistencies-between-the-webhook-option-and-runbook-option"></a>Niespójności parametru WebhookData między opcją elementu webhook a opcją elementu runbook 

* Podczas konfigurowania alertu do wywoływania elementu webhook wprowadź adres URL elementu webhook utworzonego dla elementu runbook i kliknij przycisk **Przetestuj element webhook**.  Wynikowy parametr WebhookData przesłany do elementu runbook nie zawiera elementu *.SearchResult* ani *.SearchResults*.

*  W przypadku zapisania tego alertu, gdy alert zostanie wyzwolony i wywoła element webhook, przesłany do elementu runbook parametr WebhookData będzie zawierać element *.SearchResult*.
* W przypadku utworzenia alertu i skonfigurowania go do wywoływania elementu runbook (który również tworzy element webhook) wyzwolony alert wysyła parametr WebhookData do elementu runbook, który zawiera element *.SearchResults*.

W związku z tym w powyższym przykładzie kodu należy pobrać element *.SearchResult*, jeśli alert wywołuje element webhook, bądź element *.SearchResults*, jeśli alert wywołuje element runbook bezpośrednio.

## <a name="example-walkthrough"></a>Przykładowy przewodnik 

Pokażemy, jak to działa, korzystając z następującego przykładowego graficznego elementu runbook, który uruchamia usługę systemu Windows.<br><br> ![Graficzny element runbook uruchamiający usługę systemu Windows](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice.png)<br>

Ten element runbook ma jeden parametr wejściowy typu **Obiekt** o nazwie **WebhookData** i zawiera dane elementu webhook przekazane z alertu zawierającego element *.SearchResults*.<br><br> ![Parametry wejściowe elementu runbook](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice-inputparameter.png)<br>

Na potrzeby tego przykładu w usłudze Log Analytics utworzyliśmy dwa pola niestandardowe, *SvcDisplayName_CF* i *SvcState_CF*, w celu wyodrębnienia nazwy wyświetlanej usługi i stanu usługi (uruchomiona lub zatrzymana) ze zdarzenia zapisanego w dzienniku zdarzeń systemu.  Następnie tworzymy regułę alertu z następującym zapytaniem wyszukiwania: `Type=Event SvcDisplayName_CF="Print Spooler" SvcState_CF="stopped"`, dzięki czemu możemy wykryć, kiedy usługa Print Spooler zostanie zatrzymana w systemie Windows.  Może być to dowolna interesująca nas usługa, ale w tym przykładzie odwołujemy się do jednej ze wstępnie istniejących usług zawartych w systemie operacyjnym Windows.  Akcja alertu jest skonfigurowana do wykonywania naszego elementu runbook użytego w tym przykładzie i do działania w hybrydowym procesie roboczym elementu runbook, który jest włączony w systemach docelowych.   

Działanie kodu elementu runbook **Get Service Name from LA** przekonwertuje ciąg w formacie JSON na typ obiektu i zastosuje filtr do elementu *SvcDisplayName_CF* w celu wyodrębnienia nazwy wyświetlanej usługi systemu Windows, a następnie przekaże ją do kolejnego działania, które przed podjęciem próby ponownego uruchomienia usługi sprawdzi, czy została zatrzymana.  *SvcDisplayName_CF* to [pole niestandardowe](../log-analytics/log-analytics-custom-fields.md) utworzone w usłudze Log Analytics na potrzeby tego przykładu.

    $SearchResults = (ConvertFrom-Json $WebhookData.RequestBody).SearchResults.value
    $SearchResults.SvcDisplayName_CF  

Po zatrzymaniu usługi reguła alertu w usłudze Log Analytics wykryje dopasowanie, po czym wyzwoli element runbook i wyśle kontekst alertu do tego elementu. Element runbook podejmie działanie w celu sprawdzenia, czy usługa została zatrzymana. Jeśli tak, nastąpi próba ponownego uruchomienia usługi. Działanie sprawdzi, czy usługa została uruchomiona prawidłowo, i wyprowadzi wyniki.     

Jeśli nie masz konta usługi Automation połączonego z przestrzenią roboczą pakietu OMS, możesz też skonfigurować regułę alertu z akcją elementu webhook wyzwalającą element runbook i skonfigurować element runbook do konwertowania ciągu w formacie JSON oraz filtrowania elementu *.SearchResult* zgodnie z podanymi wcześniej wskazówkami.    

## <a name="next-steps"></a>Następne kroki

* Aby dowiedzieć się więcej o alertach w usłudze Log Analytics i o sposobie ich tworzenia, zobacz temat [Alerts in Log Analytics](../log-analytics/log-analytics-alerts.md) (Alerty w usłudze Log Analytics).

* Aby zrozumieć, jak wyzwalać elementy runbook przy użyciu elementu webhook, zobacz artykuł [Elementy webhook w usłudze Azure Automation](automation-webhooks.md).



<!--HONumber=Feb17_HO2-->


