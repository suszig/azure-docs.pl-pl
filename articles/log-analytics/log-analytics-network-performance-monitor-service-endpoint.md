---
title: "Sieci monitora wydajności rozwiązania Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Możliwości usługi Menedżer punktu końcowego w Monitorze wydajności sieci umożliwia monitorowanie łączność sieciową z dowolnego punktu końcowego, który został otwarty port TCP."
services: log-analytics
documentationcenter: 
author: abshamsft
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: 7eb31b91480b6e57135581cfa2f5503de3189e10
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/23/2018
---
# <a name="service-endpoint-manager"></a>Service Endpoint Manager

Możliwości usługi Menedżer punktu końcowego w [monitora wydajności sieci](log-analytics-network-performance-monitor.md) umożliwia monitorowanie łączność sieciową z dowolnego punktu końcowego, który został otwarty port TCP. Takie punkty końcowe obejmują witryn sieci Web, aplikacji SaaS PaaS aplikacji i baz danych. 

Można wykonywać następujące funkcje związane z **Monitor punktu końcowego usługi**: 

- Monitoruje łączność sieciową do aplikacji i usług sieciowych (takich jak Office 365, Dynamics CRM, wiersz wewnętrznych aplikacji biznesowych, bazy danych SQL, itp.) z wielu gałęzi oddziałów lokalizacji 
- Testy wbudowane monitorować łączność sieciową z punktów końcowych usługi Office 365 i Dynamics365 
- Określają czas odpowiedzi, opóźnienia sieci, utraty pakietów wystąpił podczas nawiązywania połączenia z punktem końcowym 
- Określ, czy niską wydajnością znajduje się z powodu sieci lub z powodu problemu z niektórych na końcu dostawcy aplikacji 
- Zidentyfikuj punkty aktywne w sieci, który może powodować niską wydajnością wyświetlając opóźnienia przekazanych przez każdego przeskoku w formie mapy topologii. 


![Monitor punktu końcowego usługi](media/log-analytics-network-performance-monitor/service-endpoint-intro.png)


## <a name="configuration"></a>Konfigurowanie 
Aby otworzyć konfigurację monitora wydajności w sieci, otwórz [rozwiązania monitora wydajności sieci](log-analytics-network-performance-monitor.md) i kliknij przycisk **Konfiguruj** przycisku.

![Konfigurowanie monitora wydajności sieci](media/log-analytics-network-performance-monitor/npm-configure-button.png)


### <a name="configure-oms-agents-for-the-monitoring"></a>Konfigurowanie agentów OMS monitorowania.  
Włącz następujące reguły zapory w węzłach służący do monitorowania, aby rozwiązanie umożliwia odnalezienie topologii z węzłów z punktem końcowym usługi: 

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action 
```

### <a name="create-service-endpoint-monitor-tests"></a>Tworzenie testów Monitor punktu końcowego usługi 

Rozpocznij tworzenie testów monitorowania łączność sieciową z punktów końcowych usługi 

1. Polecenie **Monitor punktu końcowego usługi** kartę.
2. Kliknij przycisk **Dodaj Test** , a następnie wprowadź nazwę testu i opis. 
3. Wybierz typ testu:<br>Wybierz **testu sieci Web** Jeśli monitorowanych połączenia z usługą, która odpowiada na żądania HTTP/S, takich jak outlook.office365.com, bing.com.<br>Wybierz **test sieci** Jeśli monitorowanych połączenia z usługą, która odpowiada na żądanie protokołu TCP, ale nie odpowiada na żądania HTTP/S, takich jak SQL server, serwer FTP, SSH portu itp. 
4. Jeśli nie chcesz, aby wykonać pomiarów sieci (opóźnienia sieci, utraty pakietów, odnajdowanie topologii), a następnie wyczyść pole tekstowe. Firma Microsoft zaleca, aby zachować sprawdzany w celu pobrania maksymalnych korzyści z możliwości. 
5. Wprowadź adres IP-adresu URL/nazwa FQDN docelowego, do którego chcesz monitorować łączność sieciową.  
6. Wprowadź numer portu z usług docelowych. 
7. Wprowadź częstotliwość ma testów do uruchomienia. 
8. Wybierz węzeł, z których chcesz monitorować łączność sieciową usługi. 

    >[!NOTE]
    > Dla węzłów na serwerze z systemem Windows możliwość używa żądań TCP do przeprowadzania pomiarów sieci. Dla węzłów opartą na kliencie systemu Windows możliwość używa żądań protokołu ICMP do przeprowadzania pomiarów sieci. W niektórych przypadkach w aplikacji docelowej blokuje przychodzące żądanie protokołu ICMP na podstawie z powodu którego gdy węzły są oparte na kliencie systemu Windows rozwiązanie jest w stanie wykonać pomiarów sieci. W związku z tym zaleca się, że używasz węzły na serwerze z systemem Windows w takich przypadkach. 

9. Jeśli nie chcesz utworzyć zdarzenia kondycji dla elementów jest zaznaczony, następnie wyczyść **Włącz monitorowanie kondycji dla elementów docelowych, jest objęty przez ten test**. 
10. Wybierz warunki monitorowania. Można ustawić progami niestandardowymi kondycji zdarzenie generacji, wpisując wartości progowe. Zawsze, gdy wartość warunku wykraczają poza jego próg dla pary wybranego sieć/podsieć, jest generowane zdarzenie kondycji. 
11. Kliknij przycisk **zapisać** Aby zapisać konfigurację. 

 ![Konfiguracja Monitora punktu końcowego usługi](media/log-analytics-network-performance-monitor/service-endpoint-configuration.png)



## <a name="walkthrough"></a>Przewodnik 

Przejdź do widoku pulpitu nawigacyjnego monitorowania wydajności sieci i obserwować **Monitor punktu końcowego usługi** stronę, aby uzyskać podsumowanie informacji o kondycji inne testy, które zostały utworzone.  

![Strona Monitor punktu końcowego usługi](media/log-analytics-network-performance-monitor/service-endpoint-blade.png)

Kliknij Kafelek, aby przejść i wyświetlania szczegółów testy na **testy** strony. W tabeli LHS można wyświetlić kondycji w momencie i wartość czas odpowiedzi usługi, opóźnienie sieci i utraty pakietów, do wszystkich testów. Formant Rejestrator stanu sieci umożliwia wyświetlanie migawki sieci w innej czas w przeszłości. Polecenie testu w tabeli, które chcesz zbadać. W okienku RHS można wyświetlić trendów historycznych utraty, opóźnienia i wartości czas odpowiedzi z wykresy. Kliknij łącze Szczegóły testu, aby wyświetlić dane wydajności z każdego węzła. 

![Testy Monitor punktu końcowego usługi](media/log-analytics-network-performance-monitor/service-endpoint-tests.png)

Na **węzły testowego** widoku można obserwować łączność sieciową z każdego węzła. Kliknij węzeł o spadek wydajności.  To jest węzeł, z którym zaobserwowano aplikacja działa wolno. 

Określić, czy niską wydajnością jest obserwując korelacji między czas odpowiedzi aplikacji i opóźnienie sieci — z powodu sieci lub z powodu problemu z niektórych na końcu dostawcy aplikacji 

**Problem aplikacji:** Jeśli brak kolekcji w czasie odpowiedzi, ale opóźnienie w sieci jest spójne, a następnie sugeruje to, że sieć działa prawidłowo i jest problem z powodu problemu z końcem aplikacji.  

![Problem dotyczący aplikacji Monitor punktu końcowego usługi](media/log-analytics-network-performance-monitor/service-endpoint-application-issue.png)

**Sieci problem:** kolekcji czas odpowiedzi jest powiązany z odpowiedniej kolekcji opóźnienia sieci, a następnie sugeruje to, że czas odpowiedzi jest z powodu wzrost opóźnienia sieci.  

![Problem dotyczący usługi punktu końcowego Monitor sieci](media/log-analytics-network-performance-monitor/service-endpoint-network-issue.png)

Po określeniu, że problem dotyczy z powodu sieci, możesz kliknąć **topologii** link do wyświetlenia do identyfikowania powodującymi przeskoku na mapy topologii. Na przykład obraz poniżej przedstawiono poza opóźnienie całkowite 105 ms między węzłem a punkt końcowy aplikacji 96 ms jest z powodu przeskoku oznaczone na czerwono. Po wyłaniają powodującymi przeskoku, można podjąć działania naprawcze.  

![Testy Monitor punktu końcowego usługi](media/log-analytics-network-performance-monitor/service-endpoint-topology.png)

## <a name="diagnostics"></a>Diagnostyka 

Jeśli zauważysz nieprawidłowości, wykonaj następujące czynności:

Jeśli czasu odpowiedzi usługi, utratę połączenia sieciowego i opóźnienia są pokazywane jako NA, a następnie można go z powodu co najmniej jednego z następujących powodów:
- Aplikacja jest wyłączony.
- Węzeł używany do sprawdzania łączności z usługą jest wyłączony.
- Element docelowy w konfiguracji testu jest niepoprawny.
- Węzeł nie ma łączności sieciowej.

Jeśli jest wyświetlany czas odpowiedzi usługi prawidłowy, ale utraty sieci, a także opóźnienia są wyświetlane jako NA, może być z powodu co najmniej jednego z następujących powodów:
- Jeśli węzeł używany do sprawdzania łączności z usługą jest komputer kliencki z systemem Windows, Usługa docelowa blokuje żądania protokołu ICMP lub sieci, Zapora blokuje żądania protokołu ICMP pochodzące z węzła.
- Pole wyboru **wykonać pomiarów sieci** została wyłączona w konfiguracji testu. 

Jeśli NA to czasu odpowiedzi usługi, ale utraty sieci, a także opóźnienia są prawidłowe, następnie go sugeruje, że Usługa docelowa nie jest aplikacją sieci web. Zmień konfigurację testu i wybierz typ testu jako test sieci zamiast testu sieci Web. 

Jeśli aplikacja działa wolno, należy określić, czy niską wydajnością z powodu sieci lub z powodu problemu z niektórych na końcu dostawcy aplikacji.


## <a name="next-steps"></a>Kolejne kroki
* [Wyszukaj dzienniki](log-analytics-log-searches.md) do wyświetlania rekordów danych wydajności szczegółowe sieci.
