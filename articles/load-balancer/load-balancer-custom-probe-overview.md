---
title: "Niestandardowe sondy modułu równoważenia obciążenia i monitorowanie stanu kondycji | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać niestandardowych sondy dla usługi równoważenia obciążenia Azure do monitorowania wystąpień za usługą równoważenia obciążenia"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 102c07ff0994b3b411f2a13d7a43c5398d5dfd42
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="understand-load-balancer-probes"></a>Opis sond modułu równoważenia obciążenia

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Moduł równoważenia obciążenia Azure oferuje możliwość monitorowania kondycji wystąpień serwera przy użyciu sondy. Podczas badania nie odpowiada, usługi równoważenia obciążenia zatrzymuje wysyłanie nowe połączenia z wystąpieniem złej kondycji. Nie ma wpływu na istniejące połączenia, a nowe połączenia są wysyłane do dobrej kondycji wystąpień.

Role usługi w chmurze (role procesów roboczych oraz role sieci web) skorzystać agenta gościa w celu monitorowania sondowania. TCP lub HTTP niestandardowego sondy musi być skonfigurowany, korzystając z maszyn wirtualnych za usługą równoważenia obciążenia.

## <a name="understand-probe-count-and-timeout"></a>Licznik sondy i limit czasu

Zachowanie sondowania zależy od:

* Liczba pomyślnych sond, umożliwiających wystąpienia oznaczone jako czas.
* Liczba nieudanych sond, powodujących wystąpienia oznaczone etykietą w dół.

Wartość limitu czasu i częstotliwość ustawiona w SuccessFailCount określić, czy wystąpienie jest potwierdzone uruchomiona lub nie działa. W portalu Azure limit czasu wynosi dwa razy wartości częstotliwości.

Badania konfiguracji wszystkich wystąpień równoważeniem obciążenia dla punktu końcowego (czyli zestaw z równoważeniem obciążenia) muszą być takie same. Oznacza to, że nie może mieć konfiguracji różnych sondowania dla każdego wystąpienia roli lub maszyny wirtualnej w tej samej usługi hostowanej kombinacji danego punktu końcowego. Na przykład każde wystąpienie musi mieć identyczne porty lokalne i przekroczeń limitu czasu.

> [!IMPORTANT]
> Sonda modułu równoważenia obciążenia używa adresu IP 168.63.129.16. Ten publiczny adres IP umożliwia komunikację do zasobów wewnętrznych platformy dla bring-your — właścicielem — adresu IP scenariusz sieci wirtualnej platformy Azure. Wirtualny adres IP publicznego 168.63.129.16 jest używany we wszystkich regionach i nie zmieni się. Firma Microsoft zaleca Zezwalaj ten adres IP w żadnych zasad zapory lokalnej. Go nie należy traktować jako zagrożenie bezpieczeństwa, ponieważ tylko wewnętrzny platformy Azure może pobierać wiadomości z tego adresu. Jeśli nie zrobisz, będzie nieoczekiwanego zachowania w różnych scenariuszy, takich jak konfigurowanie tego samego zakresu adresów IP 168.63.129.16 i mających zduplikowane adresy IP.

## <a name="learn-about-the-types-of-probes"></a>Informacje o typach sondy

### <a name="guest-agent-probe"></a>Sondowanie agenta gościa

To sondowanie jest tylko dostępne dla usługi w chmurze Azure. Moduł równoważenia obciążenia, korzysta z agenta gościa na maszynie wirtualnej i nasłuchuje i wysyła odpowiedź HTTP 200 OK tylko wtedy, gdy wystąpienie jest w stanie gotowe (to znaczy nie w innym stanu takich jak zajęty, odtwarzanie lub zatrzymywanie).

Aby uzyskać więcej informacji, zobacz [Konfigurowanie pliku definicji usługi (csdef) dla sondy kondycji](https://msdn.microsoft.com/library/azure/ee758710.aspx) lub [rozpocząć tworzenie internetowy równoważenia obciążenia dla usług w chmurze](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>Dzięki czemu sondowanie agenta gościa, Oznacz wystąpienia swój stan jako niezdrowy?

Jeśli agent gościa nie odpowiada HTTP 200 OK, usługi równoważenia obciążenia oznacza wystąpienia jako odpowiadać i zatrzymuje wysyłania ruchu do tego wystąpienia. Moduł równoważenia obciążenia w dalszym ciągu polecenie ping z wystąpieniem. Jeśli agent gościa odpowie 200 protokołu HTTP, usługi równoważenia obciążenia wysyła ruch do tego wystąpienia ponownie.

Korzystając z roli sieci web, kodu witryny sieci Web zwykle działa w w3wp.exe, który nie jest monitorowane przez usługę Azure agenta sieci szkieletowej lub gościa. To oznacza, że błędów w w3wp.exe (na przykład odpowiedzi HTTP 500) nie będą raportowane do agenta gościa, a moduł równoważenia obciążenia nie będą brane danego wystąpienia poza obrotu.

### <a name="http-custom-probe"></a>Niestandardowe badanie HTTP

Niestandardowe sondę modułu równoważenia obciążenia HTTP zastąpienia domyślnej sondy agenta gościa, co oznacza, możesz utworzyć własne niestandardowej logiki do ustalenia stanu wystąpienia roli. Moduł równoważenia obciążenia sondy punktu końcowego co 15 s domyślnie. Wystąpienie jest uważana rotacji usługi równoważenia obciążenia, gdy odpowiada z 200 protokołu HTTP przed upływem limitu czasu (w sekundach 31 domyślnie).

Może to być przydatne w przypadku implementacji logiki można usunąć wystąpienia z obrotu usługi równoważenia obciążenia. Na przykład można zdecydować, że usunięcia wystąpienia, jeśli jest ponad 90% zasobów Procesora i zwraca stan – 200. Jeśli masz role sieci web, które używają w3wp.exe, oznacza to też, możesz uzyskać automatyczne monitorowania witryny sieci Web, ponieważ błędów w kodzie witryny sieci Web zwróci stan – 200 sondę modułu równoważenia obciążenia.

> [!NOTE]
> Badanie niestandardowych HTTP obsługuje ścieżek względnych i tylko protokołu HTTP. Protokół HTTPS nie jest obsługiwane.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Co sprawia, że badanie niestandardowych HTTP oznaczyć swój stan jako niezdrowy wystąpienia?

* Aplikacja HTTP zwraca kod odpowiedzi HTTP innych niż 200 (na przykład 403, 404 lub 500). To jest dodatnią potwierdzenia, że wystąpienie aplikacji powinny być brane poza eksploatacją od razu.
* Serwer HTTP nie odpowiada na wszystkich po upływie limitu czasu. W zależności od tego, która jest ustawiona wartość limitu czasu może to oznaczać tej sondy wielu żądań bez odpowiedzi przed sondy pobiera oznaczona jako nie działa z rzeczywistym użyciem (czyli przed SuccessFailCount sond są wysyłane).
* Serwer zamyka połączenie za pośrednictwem resetowania TCP.

### <a name="tcp-custom-probe"></a>Niestandardowe sondowaniem TCP

Sondy protokołu TCP nawiązania połączenia, wykonując trójstopniowego z zdefiniowany port.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Dzięki czemu niestandardowy sondowaniem TCP oznaczyć swój stan jako niezdrowy wystąpienia?

* Serwer protokołu TCP nie odpowiada na wszystkich po upływie limitu czasu. Podczas sondowania jest oznaczony jako nieuruchomiona zależy od liczby żądań zakończonych niepowodzeniem sondowania, skonfigurowane w taki sposób, aby przejść bez odpowiedzi przed oznaczeniem sondowania jako nie działa.
* Sonda odbiera TCP zresetować z wystąpienia roli.

Aby uzyskać więcej informacji o konfigurowaniu sondy kondycji protokołu HTTP lub sondowaniem TCP, zobacz [rozpocząć tworzenie internetowy równoważenia obciążenia w Menedżerze zasobów przy użyciu programu PowerShell](load-balancer-get-started-internet-arm-ps.md).

## <a name="add-healthy-instances-back-into-load-balancer-rotation"></a>Dodaj dobrej kondycji wystąpień do obrotu usługi równoważenia obciążenia

TCP i HTTP są traktowane jako dobrej kondycji i Oznacz wystąpienia roli co w przypadku dobrej kondycji:

* Moduł równoważenia obciążenia pobiera sondę dodatnią rozruchu maszyny Wirtualnej po raz pierwszy.
* Liczba SuccessFailCount (opisanym wcześniej) definiuje wartość pomyślne sond, które są wymagane w celu oznaczenia wystąpienia roli w dobrej kondycji. Jeśli usunięto wystąpienia roli, liczbę sond pomyślne, kolejne musi równa lub przekracza wartość SuccessFailCount, aby oznaczyć wystąpienia roli co działa.

> [!NOTE]
> Jeśli kondycja wystąpienia roli jest zmienne, usługi równoważenia obciążenia już czeka przed wprowadzeniem wystąpienia roli w dobrej kondycji. Można to zrobić za pomocą zasady w celu ochrony infrastruktury i użytkownika.

## <a name="use-log-analytics-for-load-balancer"></a>Użyj analizy dzienników dla usługi równoważenia obciążenia

Można użyć [logowania dla usługi równoważenia obciążenia analytics](load-balancer-monitor-log.md) Aby sprawdzić stan kondycji sondowania i licznik sondy. Rejestrowanie umożliwia z usługi Power BI lub usługi Azure Operational Insights dostarczania statystyki dotyczące stanu kondycji modułu równoważenia obciążenia.
