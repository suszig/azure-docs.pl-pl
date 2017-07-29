---
title: "Łączenie produktów dotyczących zabezpieczeń z rozwiązaniem Zabezpieczenia i inspekcja w pakiecie Operations Management Suite (OMS) | Microsoft Docs"
description: "Ten dokument ułatwia łączenie produktów dotyczących zabezpieczeń z rozwiązaniem Zabezpieczenia i inspekcja w pakiecie Operations Management Suite przy użyciu formatu CEF (Common Event Format)."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 46eee484-e078-4bad-8c89-c88a3508f6aa
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: f0a512fb0684b1db25353bf4bbd35bdf2a07e1b2
ms.openlocfilehash: 5cb5c6b6cc047974013df59d615d4f094d8d60a4
ms.contentlocale: pl-pl
ms.lasthandoff: 12/06/2016

---
# <a name="connecting-your-security-products-to-the-operations-management-suite-oms-security-and-audit-solution"></a>Łączenie produktów dotyczących zabezpieczeń z rozwiązaniem Zabezpieczenia i inspekcja w pakiecie Operations Management Suite (OMS) 
Ten dokument ułatwia łączenie produktów dotyczących zabezpieczeń z rozwiązaniem Zabezpieczenia i inspekcja w pakiecie OMS. Obsługiwane są następujące źródła:

- Zdarzenia w formacie Common Event Format (CEF)
- Zdarzenia Cisco ASA


## <a name="what-is-cef"></a>Co to jest format CEF?
Common Event Format (CEF) to standardowy format branżowy komunikatów dzienników systemu używany przez wielu dostawców zabezpieczeń w celu umożliwienia współdziałania zdarzeń między różnymi platformami. Rozwiązanie Zabezpieczenia i inspekcja w pakiecie OMS obsługuje pozyskiwanie danych w formacie CEF, który umożliwia łączenie produktów dotyczących zabezpieczeń z zabezpieczeniami pakietu OMS. 

Połączenie źródła danych z pakietem OMS umożliwia korzystanie z następujących funkcji należących do tej platformy:

- Wyszukiwanie i korelacja
- Inspekcja
- Alerty
- Analiza zagrożeń
- Problemy godne uwagi

## <a name="collection-of-security-solution-logs"></a>Zbieranie dzienników rozwiązań dotyczących zabezpieczeń

Zabezpieczenia pakietu OMS obsługują zbieranie dzienników w formacie CEF przez dzienniki systemu i dzienniki rozwiązania [Cisco ASA](https://blogs.technet.microsoft.com/msoms/2016/08/25/add-your-cisco-asa-logs-to-oms-security/). W tym przykładzie źródłem (komputerem generującym dzienniki) jest komputer z systemem Linux z uruchomionym demonem syslog-ng, a obiektem docelowym jest funkcja zabezpieczeń w pakiecie OMS. Aby przygotować komputer z systemem Linux, wykonaj następujące zadania:

- Pobierz agenta pakietu OMS dla systemu Linux w wersji 1.2.0-25 lub nowszej.
- Postępuj zgodnie z instrukcjami przedstawionymi w sekcji **krótkiego przewodnika instalacji** w [tym artykule](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux), aby zainstalować agenta i dołączyć go do obszaru roboczego.

Zwykle agent jest zainstalowany na innym komputerze niż ten, na którym są generowane dzienniki. Przekazanie dzienników do maszyny agenta zwykle wymaga wykonania następujących czynności:

- Skonfiguruj produkt/maszynę rejestrowania w celu przekazywania wymaganych zdarzeń do demona dziennika systemu (rsyslog lub syslog-ng) na maszynie agenta.
- Włącz demona dziennika systemu na maszynie agenta, aby odbierać komunikaty z systemu zdalnego.

Na maszynie agenta zdarzenia muszą być wysyłane z demona dziennika systemu do lokalnego portu UDP 25226. Agent nasłuchuje na tym porcie pod kątem zdarzeń przychodzących. Poniżej przedstawiono przykładową konfigurację wysyłania wszystkich zdarzeń z systemu lokalnego do agenta (możesz zmodyfikować konfigurację tak, aby dopasować ją do ustawień lokalnych):

1. Otwórz okno terminalu i przejdź do katalogu */etc/syslog-ng/* 
2. Utwórz nowy plik *security-config-omsagent.conf* i dodaj następującą zawartość: OMS_facility = local4
    
    filter f_local4_oms { facility(local4); };

    destination security_oms { tcp("127.0.0.1" port(25226)); };

    log { source(src); filter(f_local4_oms); destination(security_oms); };
    
3. Pobierz plik *security_events.conf* i umieść go w katalogu */etc/opt/microsoft/omsagent/conf/omsagent.d/* na komputerze agenta pakietu OMS.
4. Wpisz poniższe polecenie, aby ponownie uruchomić demona dziennika systemu:  *Uruchamianie demona syslog-ng:*
    
    ```
    sudo service rsyslog restart
    ```

    *Uruchamianie demona rsyslog:*
    
    ```
    /etc/init.d/syslog-ng restart
    ```
5. Wpisz poniższe polecenie, aby ponownie uruchomić agenta pakietu OMS:

    *Uruchamianie demona syslog-ng:*
    
    ```
    sudo service omsagent restart
    ```

    *Uruchamianie demona rsyslog:*
    
    ```
    systemctl restart omsagent
    ```
6. Wpisz poniższe polecenie i przejrzyj wynik, aby upewnić się, że w dzienniku agenta pakietu OMS nie ma żadnych błędów:

    ``` 
    tail /var/opt/microsoft/omsagent/log/omsagent.log
    ```

## <a name="reviewing-collected-security-events"></a>Przeglądanie zebranych zdarzeń zabezpieczeń

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

Po zakończeniu konfiguracji funkcja zabezpieczeń w pakiecie OMS zacznie pozyskiwać zdarzenie zabezpieczeń. Aby zwizualizować te zdarzenia, otwórz wyszukiwanie w dzienniku, wpisz polecenie *Type=CommonSecurityLog* w polu wyszukiwania i naciśnij klawisz ENTER. W poniższym przykładzie przedstawiono wynik tego polecenia. W tym przypadku funkcja zabezpieczeń pakietu OMS już pozyskała dzienniki zabezpieczeń od wielu dostawców:
   
![Ocena linii bazowej w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie OMS](./media/oms-security-connect-products/oms-security-connect-products-fig1.png)

Możesz zawęzić kryteria wyszukiwania do jednego dostawcy. Aby na przykład zwizualizować dzienniki Cisco online, wpisz: *Type=CommonSecurityLog DeviceVendor=Cisco*. Element „CommonSecurityLog” ma wstępnie zdefiniowane pola dla każdego nagłówka CEF, łącznie z podstawowymi rozszerzeniami, natomiast pozostałe rozszerzenia (niezależnie od tego, czy są to rozszerzenia niestandardowe) zostaną wstawione w polu „AdditionalExtensions”. Funkcja pól niestandardowych umożliwia uzyskanie dedykowanych pól. 

### <a name="accessing-computers-missing-baseline-assessment"></a>Dostęp do komputerów, dla których brak oceny linii bazowej
Pakiet OMS obsługuje profil linii bazowej członka domeny w systemie Windows Server od wersji 2008 R2 do wersji 2012 R2. Ostateczna wersja linii bazowej dla systemu Windows Server 2016 nie jest jeszcze opracowana i zostanie dodana natychmiast po jej opublikowaniu. Wszystkie inne systemy operacyjne skanowane za pomocą oceny linii bazowej w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie OMS są widoczne w sekcji **Komputery, dla których brak oceny linii bazowej**.

## <a name="see-also"></a>Zobacz też
W tym dokumencie pokazano, jak połączyć rozwiązanie CEF z pakietem OMS. Więcej informacji na temat zabezpieczeń w pakiecie OMS zawierają następujące artykuły:

* [Omówienie pakietu Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Monitorowanie alertów zabezpieczeń i reagowanie na nie w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite](oms-security-responding-alerts.md)
* [Monitorowanie zasobów w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite](oms-security-monitoring-resources.md)


