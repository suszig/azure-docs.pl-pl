---
title: "Wprowadzenie do przepływu rejestrowania dla grup zabezpieczeń sieci z obserwatora sieciowego Azure | Dokumentacja firmy Microsoft"
description: "Ta strona opisano sposób korzystania NSG przepływu dzienniki funkcji Azure obserwatora sieciowego"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 47d91341-16f1-45ac-85a5-e5a640f5d59e
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 4eaffba08ccf601e440709d804891668340a376d
ms.sourcegitcommit: 234c397676d8d7ba3b5ab9fe4cb6724b60cb7d25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/20/2017
---
# <a name="introduction-to-flow-logging-for-network-security-groups"></a>Wprowadzenie do przepływu rejestrowania dla grup zabezpieczeń sieci

Dzienniki przepływu sieciowej grupy zabezpieczeń są funkcją obserwatora sieciowego, który służy do wyświetlania informacji na temat przychodzące i wychodzące ruchu IP za pośrednictwem grupy zabezpieczeń sieci. Te dzienniki przepływu są zapisywane w formacie json i Pokaż przepływów wychodzącego i przychodzącego na podstawie reguły w poszczególnych kart przepływ dotyczy 5-elementowej informacji o przepływie (źródłowego i docelowego adresu IP, portu źródłowego i docelowego Protocol), i jeśli ruch został dozwolony lub niedozwolony.

![Przegląd dzienników przepływu][1]

Podczas przepływu rejestruje grup zabezpieczeń sieci docelowej, nie są wyświetlane takie same jak inne dzienniki. Przepływ dzienniki są przechowywane tylko w ramach konta magazynu i po ścieżce rejestrowania, jak pokazano w poniższym przykładzie:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

Te same zasady przechowywania wyświetlanego na inne dzienniki dotyczą dzienniki przepływu. Dzienniki ma zasady przechowywania, które można ustawić od dnia 1 do 365 dni. Jeśli zasady przechowywania nie są ustawione, dzienniki będą obsługiwane przez czas nieokreślony.

## <a name="log-file"></a>Plik dziennika

Dzienniki przepływu ma wiele właściwości. Poniżej znajduje się lista właściwości, które są zwracane w dzienniku przepływu NSG:

* **czas** — jest to czas, gdy zdarzenie zostało zarejestrowane
* **systemId** — identyfikator zasobu grupy zabezpieczeń sieci.
* **Kategoria** -kategorię zdarzenia, to jest zawsze być NetworkSecurityGroupFlowEvent
* **RESOURCEID** — identyfikator grupy NSG zasobu
* **operationName** -zawsze NetworkSecurityGroupFlowEvents
* **właściwości** -zbiór właściwości przepływu
    * **Wersja** — numer wersji schematu przepływu dziennika zdarzeń
    * **przepływy** -kolekcji przepływów. Ta właściwość ma wiele pozycji dla różnych zasad
        * **Reguła** -reguły dla wymienionych strumienie
            * **przepływy** -kolekcji przepływów
                * **Mac** — adres MAC karty sieciowej dla maszyny Wirtualnej, w którym został zebrany przepływu
                * **flowTuples** — ciąg, który zawiera wiele właściwości krotki przepływu w formacie rozdzielone przecinkami
                    * **Sygnatury czasu** — ta wartość jest sygnaturę czasową przepływu problemu w formacie epoka systemu UNIX.
                    * **Źródłowy adres IP** -źródłowy adres IP
                    * **Docelowy IP** -docelowy adres IP
                    * **Port źródłowy** — port źródłowy
                    * **Port docelowy** — docelowy Port
                    * **Protokół** — protokół przepływu. Prawidłowe wartości to **T** dla protokołu TCP i **U** UDP
                    * **Przepływu ruchu** -kierunek przepływu ruchu. Prawidłowe wartości to **I** dla ruchu przychodzącego i **O** dla ruchu wychodzącego.
                    * **Ruch** — czy też odmówiono ruchu. Prawidłowe wartości to **A** dla dozwolone i **D** dla odmowa.


Poniżej przedstawiono przykładowy dziennik przepływu. Jak widać, że ma wiele rekordów, które należy wykonać na liście właściwości opisanych w poprzedniej sekcji. 

> [!NOTE]
> Wartości właściwości flowTuples to rozdzielana przecinkami lista.
 
```json
{
    "records": 
    [
        
        {
             "time": "2017-02-16T22:00:32.8950000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282421,42.119.146.95,10.1.0.4,51529,5358,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282370,163.28.66.17,10.1.0.4,61771,3389,T,I,A","1487282393,5.39.218.34,10.1.0.4,58596,3389,T,I,A","1487282393,91.224.160.154,10.1.0.4,61540,3389,T,I,A","1487282423,13.76.89.229,10.1.0.4,53163,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:01:32.8960000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282481,195.78.210.194,10.1.0.4,53,1732,U,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282435,61.129.251.68,10.1.0.4,57776,3389,T,I,A","1487282454,84.25.174.170,10.1.0.4,59085,3389,T,I,A","1487282477,77.68.9.50,10.1.0.4,65078,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:02:32.9040000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282492,175.182.69.29,10.1.0.4,28918,5358,T,I,D","1487282505,71.6.216.55,10.1.0.4,8080,8080,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282512,91.224.160.154,10.1.0.4,59046,3389,T,I,A"]}]}]}
        }
        ,
        ...
```

## <a name="next-steps"></a>Kolejne kroki

Dowiedz się, jak włączyć dzienniki przepływu odwiedzając [przepływ włączenie rejestrowania](network-watcher-nsg-flow-logging-portal.md).

Więcej informacji na temat rejestrowania NSG, odwiedzając [dziennika analizy grup zabezpieczeń sieci (NSG)](../virtual-network/virtual-network-nsg-manage-log.md).

Dowiedzieć się, jeśli ruch jest dozwolony lub zabroniony na maszynie Wirtualnej, odwiedzając [Sprawdź, sprawdź, czy ruch z przepływem IP](network-watcher-check-ip-flow-verify-portal.md)

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-overview/figure1.png

