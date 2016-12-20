---
title: "Konfigurowanie ustawień serwera proxy i zapory w usłudze Log Analytics | Microsoft Docs"
description: "Ustawienia serwera proxy i zapory należy skonfigurować w sytuacji, gdy Twoi agenci lub usługi OMS muszą używać określonych portów."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: jwhit
editor: 
ms.assetid: b55ebd80-efd4-4220-971b-c18aea1b1ab2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/11/2016
ms.author: banders;magoedte
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 16af0b801cd6955a8edc88aae5dd5199f4f2a713


---
# <a name="configure-proxy-and-firewall-settings-in-log-analytics"></a>Konfigurowanie ustawień serwera proxy i zapory w usłudze Log Analytics
Działania wymagane do skonfigurowania ustawień serwera proxy i zapory dla usługi Log Analytics w usłudze OMS różnią się w zależności od tego, czy korzystasz z programu Operations Manager i jego agentów czy programu Microsoft Monitoring Agent i jego agentów, którzy łączą się bezpośrednio z serwerami. Przejrzyj poniższe sekcji, aby dowiedzieć się, jakiego typu agenta używasz.

## <a name="configure-proxy-and-firewall-settings-with-the-microsoft-monitoring-agent"></a>Konfigurowanie ustawień serwera proxy i zapory za pomocą programu Microsoft Monitoring Agent
Aby program Microsoft Monitoring Agent mógł nawiązać połączenie z usługą OMS i zarejestrować się za jej pomocą, musi mieć dostęp do numeru portu Twoich domen i adresów URL. Jeśli na potrzeby komunikacji między agentem i usługą OMS używasz serwera proxy, konieczne będzie zapewnienie dostępności odpowiednich zasobów. W przypadku ograniczania dostępu do Internetu za pomocą zapory należy skonfigurować w zaporze zezwalanie na dostęp do usługi OMS. W poniższych tabelach podano porty, których potrzebuje usługa OMS.

| **Zasób agenta** | **Porty** | **Obejście inspekcji HTTPS** |
| --- | --- | --- |
| \*.ods.opinsights.azure.com |443 |Tak |
| \*.oms.opinsights.azure.com |443 |Tak |
| \*.blob.core.windows.net |443 |Tak |
| ods.systemcenteradvisor.com |443 | |

Poniższa procedura pozwala skonfigurować ustawienia serwera proxy dla programu Microsoft Monitoring Agent za pomocą Panelu sterowania. Należy użyć tej procedury dla każdego serwera. Jeśli masz wiele serwerów, które trzeba skonfigurować, łatwiejszym rozwiązaniem może być użycie skryptu automatyzującego ten proces. W takim przypadku zobacz następną procedurę [Aby skonfigurować ustawienia serwera proxy dla programu Microsoft Monitoring Agent za pomocą skryptu](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-control-panel"></a>Aby skonfigurować ustawienia serwera proxy dla programu Microsoft Monitoring Agent za pomocą Panelu sterowania
1. Otwórz **Panel sterowania**.
2. Otwórz program **Microsoft Monitoring Agent**.
3. Kliknij kartę **Ustawienia serwera proxy**.<br>  
   ![karta Ustawienia serwera proxy](./media/log-analytics-proxy-firewall/proxy-direct-agent-proxy.png)
4. Zaznacz pozycję **Użyj serwera proxy**, a następnie wpisz adres URL i numer portu, jeśli jest potrzebny, podobnie jak pokazano na przykładzie. Jeśli Twój serwer proxy wymaga uwierzytelniania, wpisz nazwę użytkownika i hasło, aby uzyskać dostęp do serwera proxy.

Skorzystaj z poniższej procedury, aby utworzyć skrypt programu PowerShell umożliwiający skonfigurowanie ustawień serwera proxy dla każdego agenta, który łączy się bezpośrednio z serwerami.

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script"></a>Aby skonfigurować ustawienia serwera proxy dla programu Microsoft Monitoring Agent za pomocą skryptu
Skopiuj poniższy przykład, zaktualizuj go informacjami specyficznymi dla Twojego środowiska, zapisz go z rozszerzeniem nazwy pliku PS1, a następnie uruchom skrypt na każdym komputerze, który łączy się bezpośrednio z usługą OMS.

    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get the Health Service configuration object.  We need to determine if we
    #have the right update rollup with the API we need.  If not, no need to run the rest of the script.
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

    $proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

    if (!$proxyMethod)
    {
         Write-Output 'Health Service proxy API not present, will not update settings.'
         return
    }

    Write-Output "Clearing proxy settings."
    $healthServiceSettings.SetProxyInfo('', '', '')

    $ProxyUserName = $cred.username

    Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)


## <a name="configure-proxy-and-firewall-settings-with-operations-manager"></a>Konfigurowanie ustawień serwera proxy i zapory za pomocą programu Operations Manager
Aby grupa zarządzania programu Operations Manager mogła nawiązać połączenie z usługą OMS i zarejestrować się za jej pomocą, musi mieć dostęp do numerów portów Twoich domen i adresów URL. Jeśli na potrzeby komunikacji między serwerem zarządzania programu Operations Manager i usługą OMS używasz serwera proxy, konieczne będzie zapewnienie dostępności odpowiednich zasobów. W przypadku ograniczania dostępu do Internetu za pomocą zapory należy skonfigurować w zaporze zezwalanie na dostęp do usługi OMS. Nawet jeśli serwer zarządzania programu Operations Manager nie znajduje się za serwerem proxy, jego agenci mogą się znajdować za serwerem proxy. W takim przypadku serwer proxy należy skonfigurować w taki sam sposób, jak agentów, aby włączyć wysyłanie danych rozwiązania Security and Log Management do usługi sieci Web OMS i zezwolić na to wysyłanie.

Aby agenci programu Operations Manager mogli komunikować się z usługą OMS, Twoja infrastruktura programu Operations Manager (w tym agenci) musi mieć prawidłowe ustawienia serwera proxy oraz wersję. Ustawienia serwera proxy dla agentów są określone w konsoli programu Operations Manager. Twoja wersja powinna być jedną z następujących wersji:

* Operations Manager 2012 SP1 z pakietem zbiorczym aktualizacji 7 lub nowszym
* Operations Manager 2012 R2 z pakietem zbiorczym aktualizacji 3 lub nowszym

W poniższych tabelach podano porty związane z tymi zadaniami.

> [!NOTE]
> W przypadku niektórych z poniższych zasobów wymieniono usługi Advisor i Operational Insights. Obie te usługi to wcześniejszej wersje usługi OMS. Podane zasoby ulegną jednak zmianie w przyszłości.
>
>

Oto lista zasobów i portów agentów:<br>

| **Zasób agenta** | **Porty** |
| --- | --- |
| \*.ods.opinsights.azure.com |443 |
| \*.oms.opinsights.azure.com |443 |
| \*.blob.core.windows.net/\* |443 |
| ods.systemcenteradvisor.com |443 |

<br>
Oto lista zasobów i portów serwera zarządzania:<br>

| **Zasób serwera zarządzania** | **Porty** | **Obejście inspekcji HTTPS** |
| --- | --- | --- |
| service.systemcenteradvisor.com |443 | |
| \*.service.opinsights.azure.com |443 | |
| \*.blob.core.windows.net |443 |Tak |
| data.systemcenteradvisor.com |443 | |
| ods.systemcenteradvisor.com |443 | |
| \*.ods.opinsights.azure.com |443 |Tak |

<br>
Oto lista zasobów i portów usługi OMS i konsoli programu Operations Manager.<br>

| **Zasób pakietu OMS i konsoli programu Operations Manager** | **Porty** |
| --- | --- |
| service.systemcenteradvisor.com |443 |
| \*.service.opinsights.azure.com |443 |
| \*.live.com |Port 80 i 443 |
| \*.microsoft.com |Port 80 i 443 |
| \*.microsoftonline.com |Port 80 i 443 |
| \*.mms.microsoft.com |Port 80 i 443 |
| login.windows.net |Port 80 i 443 |

<br>

Skorzystaj z poniższych procedur, aby zarejestrować grupę zarządzania programu Operations Manager za pomocą usługi OMS. Jeśli występują problemy z komunikacją między grupą zarządzania i usługą OMS, należy skorzystać z procedur weryfikacji, aby rozwiązać problemy z transmisją danych do usługi OMS.

### <a name="to-request-exceptions-for-the-oms-service-endpoints"></a>Aby zażądać wyjątków od punktów końcowych usługi OMS
1. Użyj informacji z przedstawionej wcześniej pierwszej tabeli, aby zagwarantować, że zasoby wymagane dla serwera zarządzania programu Operations Manager są dostępne za pośrednictwem wszystkich zapór, które są skonfigurowane.
2. Użyj informacji z przedstawionej wcześniej drugiej tabeli, aby zagwarantować, że zasoby wymagane dla konsoli Operacje w programie Operations Manager oraz dla usługi OMS są dostępne za pośrednictwem wszystkich zapór, które są skonfigurowane.
3. Jeśli korzystasz z serwera proxy za pomocą programu Internet Explorer, upewnij się, że jest prawidłowo skonfigurowany i działa poprawnie. Aby to zweryfikować, możesz otworzyć bezpieczne połączenie internetowe (HTTPS), na przykład [https://bing.com](https://bing.com). Jeśli bezpieczne połączenie internetowe nie działa w przeglądarce, prawdopodobnie nie będzie ono działać w konsoli zarządzania programu Operations Manager dla usług sieci Web w chmurze.

### <a name="to-configure-the-proxy-server-in-the-operations-manager-console"></a>Aby skonfigurować serwer proxy w konsoli programu Operations Manager
1. Otwórz konsolę programu Operations Manager i wybierz obszar roboczy **Administracja**.
2. Rozwiń węzeł **Operational Insights**, a następnie wybierz pozycję **Połączenie z usługą Operational Insights**.<br>  
    ![Połączenie programu Operations Manager z pakietem OMS](./media/log-analytics-proxy-firewall/proxy-om01.png)
3. W widoku Połączenie OMS kliknij pozycję **Konfiguruj serwer proxy**.<br>  
    ![Pozycja Konfiguruj serwer proxy w widoku Połączenie OMS programu Operations Manager](./media/log-analytics-proxy-firewall/proxy-om02.png)
4. Na ekranie Serwer proxy w kreatorze ustawień usługi Operational Insights wybierz pozycję **Użyj serwera proxy, aby uzyskać dostęp do usługi sieci Web Operational Insights**, a następnie wpisz adres URL z numerem portu, na przykład: **http://moj_serwer_proxy:80**.<br>  
    ![Adres serwera proxy pakietu OMS w programie Operations Manager](./media/log-analytics-proxy-firewall/proxy-om03.png)

### <a name="to-specify-credentials-if-the-proxy-server-requires-authentication"></a>Aby podać poświadczenia, jeśli serwer proxy wymaga uwierzytelniania
 Musi zostać przeprowadzona propagacja poświadczeń i ustawień serwera proxy do zarządzanych komputerów, które będą raportować usłudze OMS. Te serwery powinny się znajdować w *grupie serwerów monitorowania usługi Microsoft System Center Advisor*. Poświadczenia są zaszyfrowane w rejestrach poszczególnych serwerów w tej grupie.

1. Otwórz konsolę programu Operations Manager i wybierz obszar roboczy **Administracja**.
2. W obszarze **Konfiguracja Uruchom jako** wybierz pozycję **Profile**.
3. Otwórz profil **Serwer proxy profilu Uruchom jako usługi System Center Advisor**.  
    ![obraz przedstawiający profil Uruchom jako serwer proxy usługi System Center Advisor](./media/log-analytics-proxy-firewall/proxy-proxyacct1.png)
4. W kreatorze profilu Uruchom jako kliknij przycisk **Dodaj**, aby użyć konta Uruchom jako. Możesz utworzyć nowe konto Uruchom jako lub użyć istniejącego konta. Konto to musi mieć wystarczające uprawnienia do komunikacji za pośrednictwem serwera proxy.  
    ![obraz przedstawiający kreatora profilu Uruchom jako](./media/log-analytics-proxy-firewall/proxy-proxyacct2.png)
5. Aby ustawić konto do zarządzania, wybierz pozycję **Wybrana klasa, grupa lub obiekt** w celu otwarcia okna Wyszukiwanie obiektów.  
    ![obraz przedstawiający kreatora profilu Uruchom jako](./media/log-analytics-proxy-firewall/proxy-proxyacct2-1.png)
6. Wyszukaj **grupę serwerów monitorowania usługi Microsoft System Center Advisor**, a następnie ją wybierz.  
    ![obraz przedstawiający okno Wyszukiwanie obiektów](./media/log-analytics-proxy-firewall/proxy-proxyacct3.png)
7. Kliknij przycisk **OK**, aby zamknąć okno Dodawanie konta Uruchom jako.  
    ![obraz przedstawiający kreatora profilu Uruchom jako](./media/log-analytics-proxy-firewall/proxy-proxyacct4.png)
8. Zakończ działanie kreatora i zapisz zmiany.  
    ![obraz kreatora profilu Uruchom jako](./media/log-analytics-proxy-firewall/proxy-proxyacct5.png)

### <a name="to-validate-that-oms-management-packs-are-downloaded"></a>Aby sprawdzić, czy pakiety zarządzania usługi OMS zostały pobrane
Jeśli dodano rozwiązania do usługi OMS, możesz je wyświetlić w konsoli programu Operations Manager jako pakiety zarządzania w obszarze **Administracja**. Wyszukaj usługę *System Center Advisor*, aby je szybko znaleźć.  
    ![pobrane pakiety zarządzania](./media/log-analytics-proxy-firewall/proxy-mpdownloaded.png) Możesz również sprawdzić pakiety zarządzania pakietu OMS, używając następującego polecenia programu Windows PowerShell na serwerze zarządzania programu Operations Manager:

    ```
    Get-ScomManagementPack | where {$_.DisplayName -match 'Advisor'} | select Name,DisplayName,Version,KeyToken
    ```

### <a name="to-validate-that-operations-manager-is-sending-data-to-the-oms-service"></a>Aby zweryfikować, czy program Operations Manager wysyła dane do usługi OMS
1. Na serwerze zarządzania programu Operations Manager otwórz Monitor wydajności (perfmon.exe), a następnie wybierz pozycję **Monitor wydajności**.
2. Kliknij przycisk **Dodaj**, a następnie wybierz pozycję **Grupy zarządzania usługi kondycji**.
3. Dodaj wszystkie liczniki rozpoczynające się ciągiem **HTTP**.  
    ![dodawanie liczników](./media/log-analytics-proxy-firewall/proxy-sendingdata1.png)
4. Jeśli Twoja konfiguracja programu Operations Manager jest poprawna, zobaczysz aktywność liczników zarządzania usługi kondycji dla zdarzeń i innych elementów danych na podstawie pakietów zarządzania dodanych w usłudze OMS oraz skonfigurowanych zasad gromadzenia dzienników.  
    ![Aktywność wyświetlana w Monitorze wydajności](./media/log-analytics-proxy-firewall/proxy-sendingdata2.png)

## <a name="next-steps"></a>Następne kroki
* [Dodaj rozwiązania Log Analytics z galerii rozwiązań](log-analytics-add-solutions.md), aby dodać funkcje i zebrać dane.
* Zapoznaj się z [wyszukiwaniem w dziennikach](log-analytics-log-searches.md), aby wyświetlić szczegółowe informacje zebrane przez rozwiązania.



<!--HONumber=Dec16_HO1-->


