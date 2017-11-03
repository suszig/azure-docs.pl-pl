---
title: Zbieranie danych z lokalnego komputery z systemem Linux z Azure Log Analytics | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak wdrożyć agenta analizy dzienników dla systemu Linux i Włącz zbieranie danych z tego systemu operacyjnego z analizy dzienników."
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/13/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: d22fe6456c3bd886f8f8863d362c0084fbe03da3
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="collect-data-from-linux-computers-hosted-in-your-environment"></a>Zbieranie danych z komputerów z systemem Linux hostowanych w danym środowisku
[Analiza dzienników Azure](log-analytics-overview.md) może zbierać dane bezpośrednio z komputerów fizycznych lub wirtualnych systemu Linux i innych zasobów w danym środowisku, w jednym repozytorium szczegółowej analizy i korelacji.  Ta opcja szybkiego startu przedstawia sposób konfigurowania i zbieranie danych z komputera z systemem Linux z kilku prostych krokach.  Dla maszyn wirtualnych systemu Linux platformy Azure, zobacz następujący temat [zbierać dane o maszynach wirtualnych platformy Azure](log-analytics-quick-collect-azurevm.md).  
 
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="log-in-to-azure-portal"></a>Zaloguj się do portalu Azure
Zaloguj się do portalu Azure pod adresem [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Tworzenie obszaru roboczego
1. W portalu Azure kliknij **więcej usług** znaleziono w lewym dolnym rogu. Na liście zasobów wpisz **Log Analytics**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Wybierz **dziennika analizy**.<br><br> ![Witryna Azure Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. Kliknij przycisk **Utwórz**, a następnie wybierz opcje dla następujących elementów:

  * Podaj nazwę dla nowego **obszarem roboczym pakietu OMS**, takich jak *DefaultLAWorkspace*. 
  * Wybierz **Subskrypcję** do połączenia poprzez wybór subskrypcji z listy rozwijanej, jeśli domyślnie wybrana subskrypcja jest niewłaściwa.
  * Aby uzyskać **grupy zasobów**, wybierz istniejącą grupę zasobów, która zawiera co najmniej jednej maszyny wirtualnej Azure.  
  * Wybierz **lokalizacji** maszyny wirtualne są wdrażane.  Aby uzyskać dodatkowe informacje, sprawdź, która [Log Analytics jest dostępna w regionach](https://azure.microsoft.com/regions/services/).
  * Możesz wybrać spośród trzech różnych **warstw cenowych** w analizy dzienników, ale dla tego przewodnika Szybki Start zamierzasz wybierz **wolnego** warstwy.  Aby uzyskać dodatkowe informacje dotyczące konkretnego warstw, zobacz [szczegóły cennika usługi Analiza dzienników](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](./media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-01.png)<br>  
3. Po podaniu wymaganych informacji w **obszarem roboczym pakietu OMS** okienku, kliknij przycisk **OK**.  

Podczas weryfikowania informacji i tworzenia obszaru roboczego możesz śledzić postęp w sekcji **Powiadomienia** z poziomu menu. 

## <a name="obtain-workspace-id-and-key"></a>Uzyskaj identyfikator i klucz
Przed zainstalowaniem agenta pakietu OMS dla systemu Linux, potrzebne identyfikator i klucz obszaru roboczego analizy dzienników.  Te informacje są wymagane przez skrypt otoki agenta, aby poprawnie skonfigurować agenta i upewnij się, że mogą się komunikować z analizy dzienników.  

1. W portalu Azure kliknij **więcej usług** znaleziono w lewym dolnym rogu. Na liście zasobów wpisz **Log Analytics**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Wybierz **dziennika analizy**.
2. Na liście obszarów roboczych usługi Analiza dzienników, wybierz *DefaultLAWorkspace* utworzony wcześniej.
3. Wybierz **Zaawansowane ustawienia**.<br><br> ![Ustawienia zaawansowane analizy dzienników](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. Wybierz **połączonych źródeł**, a następnie wybierz **serwerów z systemem Linux**.   
5. Wartość z prawej strony **identyfikator obszaru roboczego** i **klucz podstawowy**. Skopiuj i Wklej zarówno do Twojego ulubionego edytora.   

## <a name="install-the-agent-for-linux"></a>Zainstaluj agenta dla systemu Linux
Następujące kroki konfiguracji Instalatora agenta dla analizy dzienników w chmurze Azure oraz Azure dla instytucji rządowych.  

1. Aby skonfigurować komputerze z systemem Linux do nawiązania połączenia analizy dzienników, uruchom następujące polecenie, podając identyfikator i klucz podstawowy, wcześniej skopiowane.  To polecenie pobiera agenta, sprawdza poprawność jej sumy kontrolnej i instaluje je. 
    
    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

2. Aby skonfigurować komputerze z systemem Linux do nawiązania połączenia analizy dzienników w chmurze Azure dla instytucji rządowych, uruchom następujące polecenie, podając identyfikator i klucz podstawowy, wcześniej skopiowane.  To polecenie pobiera agenta, sprawdza poprawność jej sumy kontrolnej i instaluje je. 

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ``` 

## <a name="configure-agent-to-communicate-with-a-proxy-server"></a>Konfigurowanie agenta do komunikowania się z serwerem proxy

Jeśli komputery Linux muszą komunikować się za pośrednictwem serwera proxy do analizy dzienników, należy wykonać następujące czynności.  Wartość konfiguracji serwera proxy ma następującą składnię `[protocol://][user:password@]proxyhost[:port]`.

1. Edytuj plik `/etc/opt/microsoft/omsagent/proxy.conf` , uruchamiając następujące polecenia i zmień wartości podanych ustawień.

    ```
    proxyconf="https://proxyuser:proxypassword@proxyserver01:30443"
    sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
    sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf 
    ```

2. Uruchom ponownie agenta, uruchamiając następujące polecenie: 

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ``` 

## <a name="collect-event-and-performance-data"></a>Zbieranie danych zdarzeń i wydajności
Analizy dzienników można zbierać zdarzenia z dziennika systemu Linux i liczniki wydajności, które określisz dłuższy okres analizy i raportowania i podejmij akcję po wykryciu określonego warunku.  Wykonaj następujące kroki, aby rozpocząć konfigurowanie zbierania zdarzeń z dziennika systemu Linux i kilka typowych liczników wydajności.  

1. Wybierz **Syslog**.  
2. Możesz dodać dziennika zdarzeń, wpisując nazwę dziennika.  Typ **Syslog** , a następnie kliknij znak plus  **+** .  
3. W tabeli, usuń zaznaczenie pola wyboru wag **informacji**, **powiadomienia** i **debugowania**. 
4. Kliknij przycisk **zapisać** w górnej części strony, aby zapisać konfigurację.
5. Wybierz **dane wydajności systemu Linux** można włączyć kolekcji liczników wydajności na komputerze z systemem Windows. 
6. Podczas pierwszej konfiguracji liczników wydajności systemu Linux dla nowego obszaru roboczego analizy dzienników, podano opcję, aby szybko utworzyć kilka typowych liczników. Są one wyświetlane pole wyboru obok każdego.<br><br> ![Liczniki wydajności systemu Windows domyślne wybrane](media/log-analytics-quick-collect-azurevm/linux-perfcounters-default.png).<br><br> Kliknij przycisk **Dodaj wybrane liczniki wydajności**.  Są one dodawane i ustawienia wstępnego z interwału próbkowania sekundę dziesięć kolekcji.  
7. Kliknij przycisk **zapisać** w górnej części strony, aby zapisać konfigurację.

## <a name="view-data-collected"></a>Wyświetlanie danych zebranych
Teraz, gdy jest włączone zbieranie danych, pozwala uruchomić przykład wyszukiwania prostego dziennika, aby wyświetlić dane z komputera docelowego.  

1. W portalu Azure przejdź do analizy dzienników i wybierz obszar roboczy utworzony wcześniej.
2. Kliknij przycisk **wyszukiwania dziennika** Kafelek i w okienku wyszukiwania dziennika, w polu wpisz kwerendę `Perf` , a następnie naciśnij klawisz wprowadź lub kliknij przycisk wyszukiwania z prawej strony pola zapytania.<br><br> ![Przykład zapytania wyszukiwania dziennika analizy dzienników](media/log-analytics-quick-collect-linux-computer/log-analytics-portal-queryexample.png)<br><br> Na przykład 735 rekordów wydajności zwrócić zapytania na poniższej ilustracji.<br><br> ![Wynik wyszukiwania dziennika analizy dzienników](media/log-analytics-quick-collect-linux-computer/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów
Gdy nie są już potrzebne, można usunąć agenta z komputera z systemem Linux i usuwanie obszaru roboczego analizy dzienników.  

Aby usunąć agenta, wykonaj następujące kroki.

1. Pobierz agenta systemu Linux [uniwersalnego skryptu](https://github.com/Microsoft/OMS-Agent-for-Linux/releases) do komputera.
2. Uruchom w pliku SH pakietu *--przeczyścić* argumentu na komputerze, który powoduje całkowite usunięcie agenta i jego konfiguracja.

    `sudo sh ./omsagent-<version>.universal.x64.sh --purge`

Aby usunąć obszar roboczy, wybierz obszar roboczy analizy dzienników utworzono wcześniej i kliknij stronę zasobów **usunąć**.<br><br> ![Usuń zasób analizy dzienników](media/log-analytics-quick-collect-azurevm/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Następne kroki
Teraz, kiedy są zbierane operacyjne i dane dotyczące wydajności z komputera lokalnego systemu Linux, można łatwo rozpocząć eksplorowanie, analizowanie i podjęcie działań na danych zbieranych dla *wolnego*.  

Aby dowiedzieć się, jak przeglądać i analizować dane, nadal samouczka.   

> [!div class="nextstepaction"]
> [Wyświetlanie i analizowanie danych analizy dzienników](log-analytics-tutorial-viewdata.md)
