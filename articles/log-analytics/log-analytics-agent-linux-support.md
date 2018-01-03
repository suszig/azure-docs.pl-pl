---
title: "Rozwiązywanie problemów z agenta systemu Linux Analiza dzienników Azure | Dokumentacja firmy Microsoft"
description: "Opisz objawy, przyczyny i rozwiązania typowych problemów z agentem Linux analizy dziennika."
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
ms.date: 12/17/2017
ms.author: magoedte
ms.openlocfilehash: 5f598da9b82b4425ca509a26a2e6e366ba4c3394
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/19/2017
---
# <a name="how-to-troubleshoot-issues-with-the-linux-agent-for-log-analytics"></a>Jak rozwiązywać problemy z agentem systemu Linux dla analizy dzienników

Ten artykuł zawiera pomoc w rozwiązywaniu błędów mogą wystąpić z agentem systemu Linux w celu wykonania analizy dziennika, a także sugeruje możliwe rozwiązania, aby je rozwiązać.

## <a name="issue-unable-to-connect-through-proxy-to-log-analytics"></a>Problem: Nie można nawiązać połączenia za pośrednictwem serwera proxy do analizy dzienników

### <a name="probable-causes"></a>Prawdopodobne przyczyny
* Określone podczas dołączania serwera proxy jest niepoprawne
* Analiza dzienników i punktów końcowych usługi Automatyzacja Azure nie są białej w centrum danych. 

### <a name="resolutions"></a>Rozwiązania
1. Reonboard z usługą analizy dzienników z agentem pakietu OMS dla systemu Linux przy użyciu następującego polecenia z opcją `-v` włączone. Dzięki temu pełne dane wyjściowe agenta łączących się za pośrednictwem serwera proxy z usługą OMS. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. Zapoznaj się z sekcją [Konfigurowanie agenta do użycia przy użyciu serwera proxy lub bramy OMS](#configuring the-agent-for-use-with-a-proxy-server-or-oms-gateway) można sprawdzić poprawności konfiguracji agenta do komunikowania się za pośrednictwem serwera proxy.    
* Sprawdź, czy następujące punkty końcowe usługi analizy dzienników białej:

    |Zasób agenta| Porty |  
    |------|---------|  
    |*.ods.opinsights.azure.com | Port 443|   
    |*.oms.opinsights.azure.com | Port 443|   
    |ods.systemcenteradvisor.com | Port 443|   
    |*.blob.Core.Windows.NET/ | Port 443|   

## <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Problem: Komunikat o błędzie 403 podczas próby dołączenia

### <a name="probable-causes"></a>Prawdopodobne przyczyny
* Data i godzina jest nieprawidłowa na serwerze z systemem Linux 
* Identyfikator i klucz obszaru roboczego, używane są nieprawidłowe

### <a name="resolution"></a>Rozwiązanie

1. Sprawdź czas na serwerze Linux przy użyciu polecenia danych. Jeśli godzina +/-15 minut od bieżącego czasu dołączania kończy się niepowodzeniem. Aby poprawne to zaktualizuj datę i/lub strefy czasowej serwera z systemem Linux. 
2. Sprawdź, czy zainstalowano najnowszą wersję agenta pakietu OMS dla systemu Linux.  Najnowsza wersja teraz powiadamia się, że możesz Jeśli przesunięcia czasowego powoduje błąd przy dołączaniu.
3. Reonboard przy użyciu poprawny identyfikator i klucz obszaru roboczego z instrukcjami instalacji wcześniej w tym temacie.

## <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Problem: Widzisz 500 i 404 błąd w pliku dziennika bezpośrednio po dołączania
Jest to znany problem występujący podczas pierwszego przekazywania danych Linux w obszarze roboczym analizy dzienników. Nie dotyczy to danych wysłanego lub usługa obsługi.

## <a name="issue-you-are-not-seeing-any-data-in-the-azure-portal"></a>Problem: Nie widać żadnych danych w portalu Azure

### <a name="probable-causes"></a>Prawdopodobne przyczyny

- Dołączenie do usługi analizy dzienników nie powiodło się
- Połączenie z usługą analizy dzienników jest zablokowane
- Agent pakietu OMS dla systemu Linux danych jest wykonywana kopia zapasowa

### <a name="resolutions"></a>Rozwiązania
1. Sprawdź, czy przechodzenia do usługi analizy dzienników powiodła się przez sprawdzenie, czy istnieje następującego pliku:`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Przy użyciu Reonboard `omsadmin.sh` instrukcje wiersza polecenia
3. Jeśli przy użyciu serwera proxy, odwołują się do serwera proxy kroki rozwiązania, znajdujących się we wcześniejszej.
4. W niektórych przypadkach gdy Agent pakietu OMS dla systemu Linux nie mogą komunikować się z usługą, dane na agencie jest w kolejce do rozmiaru buforu pełnej, czyli 50 MB. Agent pakietu OMS Linux powinien zostać uruchomiony ponownie, uruchamiając następujące polecenie: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Tego problemu w 1.1.0-28 wersję agenta i nowszych.

