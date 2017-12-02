---
title: "Połączenie komputerów Linux Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób podłączania komputerów z systemem Linux hostowanych w Azure, innych chmurze lub lokalnie do analizy dzienników z agentem pakietu OMS dla systemu Linux."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2017
ms.author: magoedte
ms.openlocfilehash: 1f6e0f2a638d9ce9b06f19feba1c9e216812862c
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/01/2017
---
# <a name="connect-your-linux-computers-to-log-analytics"></a>Łączenie komputerów Linux do analizy dzienników 

Z usługi Analiza dzienników Azure można zbierać i korzystania z danych generowanych przez komputery z systemem Linux i kontener rozwiązań, takich jak Docker znajdującej się w lokalnym centrum danych jako serwerów fizycznych lub maszynach wirtualnych, maszyny wirtualne w usług hostowanych w chmurze, takich jak Amazon (AWS) usług sieci Web lub Microsoft Azure. Umożliwia także rozwiązania do zarządzania dostępnych w [usługi Automatyzacja Azure](../automation/automation-intro.md) takich jak śledzenia zmian, aby zidentyfikować zmian konfiguracji i zarządzania aktualizacjami w celu zarządzania aktualizacjami oprogramowania do aktywnego zarządzania cyklem życia z systemem Linux Maszyny wirtualne. 

Agent pakietu OMS Linux komunikuje się wychodzące z analizy dzienników i usługi Automatyzacja Azure za pośrednictwem protokołu TCP portu 443, a jeśli komputer łączy się z serwerem zapory lub serwera proxy do komunikacji za pośrednictwem Internetu, przejrzyj [konfigurowania agenta do użycia z programem Serwer proxy lub bramy OMS](#configuring-the-agent-for-use-with-a-proxy-server-or-oms-gateway) zrozumienie będzie zmian konfiguracji, należy zastosować.  Jeśli monitorowanych komputerów z System Center 2016 - Operations Manager lub programu Operations Manager 2012 R2, można go wieloadresowego z usługą analizy dzienników w celu zbierania danych i przekazywać je do usługi i nadal być monitorowane przez program Operations Manager.  Komputery z systemem Linux monitorowane przez grupę zarządzania programu Operations Manager, który jest zintegrowany z analizy dzienników (obecnie określone Operations Management Suite w konsoli operacje programu Operations Manager) nie pobranie konfiguracji dla źródła danych i do przodu dane zbierane przez grupę zarządzania.  Nie można skonfigurować agenta pakietu OMS do raportu, aby więcej niż jeden obszar roboczy analizy dzienników.  

Jeśli zasady zabezpieczeń IT nie zezwalają na komputerach w sieci, aby nawiązać połączenie z Internetem, aby połączyć się z bramą OMS do odbierania informacji o konfiguracji i wysyłania danych zebranych w zależności od rozwiązania, które aktywowano można skonfigurować agenta. Aby uzyskać więcej informacji i kroki dotyczące sposobu konfigurowania agenta systemu Linux OMS do komunikacji za pośrednictwem bramy OMS do usług, zobacz [łączenia komputerów przy użyciu bramy OMS OMS](log-analytics-oms-gateway.md).  

Poniższy diagram przedstawia połączenie między komputery zarządzane z wykorzystaniem agentów z systemem Linux i analizy dziennika, w tym kierunku i portów.

![Komunikacja agenta bezpośrednio z usług Azure diagram](./media/log-analytics-agent-linux/log-analytics-agent-linux-communication.png)

## <a name="system-requirements"></a>Wymagania systemowe
Przed rozpoczęciem należy przejrzeć następujące informacje, aby sprawdzić, czy zostały spełnione wymagania wstępne.

### <a name="supported-linux-operating-systems"></a>Obsługiwane systemy operacyjne Linux
Oficjalnie obsługiwane są następujące dystrybucje systemu Linux.  Jednak Agent pakietu OMS dla systemu Linux mogą również uruchomić na innych dystrybucje nie na liście.

* Linux Amazon 2012.09 do 2015.09 (x86/x64)
* CentOS Linux 5, 6 i 7 (x86/x64)
* Oracle Linux 5, 6 i 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 i 7 (x86/x64)
* Debian GNU/Linux 6, 7 i 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, LTS 16.04 (x86/x64)
* SUSE Linux Enterprise Server 11 i 12 (x86/x64)

### <a name="network"></a>Sieć
Informacje poniżej listy proxy i zapory konfiguracji wymaganych informacji dla agenta systemu Linux do komunikowania się z analizy dzienników i automatyzacji Azure. Ruch jest wychodzący z sieci do usługi. 

|Zasób agenta| Porty |  
|------|---------|  
|*.ods.opinsights.azure.com | Port 443|   
|*.oms.opinsights.azure.com | Port 443|   
|*.blob.Core.Windows.NET/ | Port 443|   
|*.azure-automation.net | Port 443|  

### <a name="package-requirements"></a>Wymagania pakietu

 **Wymagany pakiet**   | **Opis**   | **Minimalna wersja**
--------------------- | --------------------- | -------------------
Glibc | Biblioteka C GNU   | 2.5-12 
Biblioteki Openssl | Biblioteki OpenSSL | 0.9.8e lub 1.0
Narzędzie curl | cURL klienta sieci web | 7.15.5
Ctypes języka Python | | 
PAM | Uwierzytelnianie podłączane moduły | 

> [!NOTE]
>  Rsyslog lub syslog ng są wymagane do zbierania komunikaty dziennika systemowego. Demon syslog domyślne w wersji 5 (sysklog) w wersji Red Hat Enterprise Linux, CentOS i Oracle Linux nie jest obsługiwana dla zbierania zdarzeń usługi syslog. Aby zbierać dane syslog z tej wersji tych dystrybucji, demona rsyslog powinien zainstalowane i skonfigurowane do Zastąp sysklog, 

Agent zawiera wiele pakietów. Plik wersji zawiera następujących pakietów, dostępne za pomocą powłoki pakietu z `--extract`:

**Pakiet** | **Wersja** | **Opis**
----------- | ----------- | --------------
omsagent | 1.4.0 | Agent programu Operations Management Suite dla systemu Linux
omsconfig | 1.1.1 | Agent konfiguracji dla agenta pakietu OMS
OMI | 1.2.0 | Otwórz infrastruktury zarządzania (OMI) - lightweight serwer modelu wspólnych informacji
scx | 1.6.3 | OMI CIM dostawców dla metryki wydajności systemu operacyjnego
Apache cimprov | 1.0.1 | Monitorowanie dostawcę dla OMI wydajności Apache HTTP Server. Zainstalowany w przypadku wykrycia Apache HTTP Server.
MySQL cimprov | 1.0.1 | Monitorowanie dostawcę dla OMI wydajności serwera MySQL. Zainstalowany w przypadku wykrycia MySQL/MariaDB serwera.
docker cimprov | 1.0.0 | Dostawca docker OMI. Zainstalowany w przypadku wykrycia Docker.

### <a name="compatibility-with-system-center-operations-manager"></a>Zgodność z programem System Center Operations Manager
Agent pakietu OMS dla systemu Linux udostępnia pliki binarne agenta agenta programu System Center Operations Manager. Po zainstalowaniu Agent pakietu OMS dla systemu Linux w systemie, w obecnie zarządzany przez program Operations Manager, uaktualnia OMI i SCX pakietów na komputerze do nowszej wersji. W tej wersji pakietu OMS i System Center 2016 - agentów programu Operations Manager/Operations Manager 2012 R2 dla systemu Linux są zgodne. 

> [!NOTE]
> System Center 2012 SP1 i wcześniejsze wersje obecnie nie są zgodne lub nie jest obsługiwany z agentem pakietu OMS dla systemu Linux.<br>
> Jeśli Agent pakietu OMS dla systemu Linux jest zainstalowana na komputerze, który nie jest aktualnie monitorowane przez program Operations Manager, a następnie chcesz monitorować komputer z programem Operations Manager, należy zmodyfikować [konfiguracji OMI](#enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager) przed odnajdywania komputer. **Ten krok jest *nie* potrzebne, jeśli agent programu Operations Manager została zainstalowana przed Agent pakietu OMS dla systemu Linux.**

### <a name="system-configuration-changes"></a>Zmiany konfiguracji systemu
Po zainstalowaniu agenta pakietu OMS pakietów systemu Linux, są stosowane następujące zmiany w dodatkowych konfiguracji całego systemu. Te artefakty muszą zostać usunięte po odinstalowaniu pakietu omsagent.

* Użytkownik bez uprawnień o nazwie: `omsagent` jest tworzony. To konto, które demona omsagent działa jako.
* Plik "Dołącz" sudoers jest tworzony w /etc/sudoers.d/omsagent. To autoryzuje omsagent ponowne uruchomienie demonów syslog i omsagent. Jeśli dyrektywy "Dołącz" sudo nie są obsługiwane w zainstalowanej wersji programu sudo, te wpisy są zapisywane do /etc/sudoers.
* Zmodyfikowaniu konfiguracji syslog do przekazywania podzbiór zdarzeń na agencie. Aby uzyskać więcej informacji, zobacz **Konfigurowanie zbierania danych** poniższej sekcji

### <a name="upgrade-from-a-previous-release"></a>Uaktualnianie z poprzedniej wersji
Uaktualnienie z wersji starszej niż 1.0.0-47 jest obsługiwane w tej wersji. Przeprowadzanie instalacji z `--upgrade` polecenia uaktualnia wszystkie składniki agenta do najnowszej wersji.

## <a name="installing-the-agent"></a>Instalacja agenta

Ta sekcja zawiera opis sposobu instalowania agenta pakietu OMS Linux ręcznie przy użyciu bunndle, który zawiera pakiety Debian i obr. / min dla poszczególnych składników agenta.  Można go zainstalować bezpośrednio lub wyodrębniony do pobierania poszczególnych pakietów.  Jeśli planujesz zainstalować agenta na maszynie Wirtualnej systemu Linux platformy Azure, zobacz następujący temat [zbierać dane o maszynach wirtualnych platformy Azure](log-analytics-quick-collect-azurevm.md) więcej informacji na temat instalowania agenta przy użyciu rozszerzenia maszyny Wirtualnej analizy dziennika.  Wykonaj kroki opisane w sekcji *włączyć rozszerzenia maszyny Wirtualnej analizy dziennika*.  Na komputery z systemem Linux hostowanych w danym środowisku, można uprościć proces instalacji przy użyciu metody skryptową opisaną w artykule, [zbierania danych z komputerów z systemem Linux hostowanych w danym środowisku](log-analytics-quick-collect-linux-computer.md).  

> [!NOTE]
> Gdy dwa artykuły powyżej są przeznaczone do osoby, która jest nowa analizy dzienników i rozpocząć korzystanie z usługi szybko, kroki konfigurowania komputera są istotne.  Jeśli już masz obszaru roboczego i są chcą połączyć komputerze z systemem Linux, wybierz istniejący obszar roboczy maszyny Wirtualnej systemu Linux na platformie Azure, czy dla komputera hostowane poza Azure, kopia identyfikator obszaru roboczego, a klucz do przekazania do skryptu.  

Przed zainstalowaniem agenta pakietu OMS dla systemu Linux, potrzebne identyfikator i klucz obszaru roboczego analizy dzienników.  

1. Zaloguj się do portalu Azure pod adresem [https://portal.azure.com](https://portal.azure.com). 
2. W witrynie Azure Portal kliknij pozycję **Więcej usług** w lewym dolnym rogu. Na liście zasobów wpisz **Log Analytics**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Wybierz **dziennika analizy**.
3. Na liście obszarów roboczych usługi Analiza dzienników wybierz komputer zgłoszenia do obszaru roboczego.
3. Wybierz **Zaawansowane ustawienia**.<br><br> ![Ustawienia zaawansowane analizy dzienników](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. Wybierz **połączonych źródeł**, a następnie wybierz **serwerów z systemem Linux**.   
5. Wartość z prawej strony **identyfikator obszaru roboczego** i **klucz podstawowy**. Skopiuj i Wklej zarówno do Twojego ulubionego edytora.  
6. Pobierz najnowszą [Agent pakietu OMS Linux (x64)](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x64.sh) lub [Agent pakietu OMS Linux x86](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x86.sh) z usługi GitHub.  
7. Transfer odpowiedniego pakietu (x86 lub x64) na komputerze systemu Linux przy użyciu scp/sftp.
8. Instalowanie pakietu przy użyciu `--install` lub `--upgrade` argumentu. 

    > [!NOTE]
    > Jeśli wszystkie istniejące pakiety są instalowane, takich jak kiedy agent programu System Center Operations Manager dla systemu Linux jest już zainstalowana, użyj `--upgrade` argumentu. Aby nawiązać połączenie pakietu Operations Management Suite podczas instalacji, należy podać `-w <WorkspaceID>` i `-s <Shared Key>` parametrów.


#### <a name="to-install-and-onboard-directly"></a>Aby zainstalować i podłączone bezpośrednio
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -w <workspace id> -s <shared key>
```

#### <a name="to-upgrade-the-agent-package"></a>Aby uaktualnić pakiet agenta
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade
```

#### <a name="to-install-and-onboard-to-a-workspace-in-us-government-cloud"></a>Aby zainstalować i dołączyć do obszaru roboczego w chmurze firmy instytucji rządowych USA
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -w <workspace id> -s <shared key> -d opinsights.azure.us
```

## <a name="configuring-the-agent-for-use-with-a-proxy-server-or-oms-gateway"></a>Konfigurowanie agenta do użycia przy użyciu serwera proxy lub bramy OMS
Agent pakietu OMS Linux obsługuje komunikacji przy użyciu serwera proxy lub bramy OMS z usługą analizy dzienników przy użyciu protokołu HTTPS.  Zarówno w przypadku anonimowych, jak i podstawowe uwierzytelnianie (nazwy użytkownika i hasła) jest obsługiwane.  

### <a name="proxy-configuration"></a>Konfiguracja serwera proxy
Wartość konfiguracji serwera proxy ma następującą składnię:

`[protocol://][user:password@]proxyhost[:port]`

Właściwość|Opis
-|-
Protokół|https
użytkownik|Opcjonalna nazwa użytkownika dla uwierzytelniania serwera proxy
hasło|Opcjonalne hasło dla uwierzytelniania serwera proxy
proxyhost|Adres lub nazwę FQDN serwera proxy serwera/OMS bramy
port|Numer portu opcjonalne dla serwera proxy serwera/OMS bramy

Na przykład: `https://user01:password@proxy01.contoso.com:30443`

Podczas instalacji lub modyfikując plik konfiguracji proxy.conf po zakończeniu instalacji można określić serwer proxy.   

### <a name="specify-proxy-configuration-during-installation"></a>Określ konfigurację serwera proxy podczas instalacji
`-p` Lub `--proxy` argument dla pakietu instalacji omsagent Określa konfigurację serwera proxy do użycia. 

```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -p https://<proxy user>:<proxy password>@<proxy address>:<proxy port> -w <workspace id> -s <shared key>
```

### <a name="define-the-proxy-configuration-in-a-file"></a>Zdefiniuj w pliku konfiguracji serwera proxy
Konfiguracja serwera proxy można ustawić w plikach `/etc/opt/microsoft/omsagent/proxy.conf` i `/etc/opt/microsoft/omsagent/conf/proxy.conf `. Pliki można bezpośrednio utworzyć lub edytować, ale ich uprawnienia muszą zostać zaktualizowane uprawnienia użytkownika omiuser uprawnienie do odczytu w pliki. Na przykład:
```
proxyconf="https://proxyuser:proxypassword@proxyserver01:30443"
sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf
sudo chmod 600 /etc/opt/microsoft/omsagent/proxy.conf /etc/opt/microsoft/omsagent/conf/proxy.conf  
sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
```

### <a name="removing-the-proxy-configuration"></a>Usuwanie konfiguracji serwera proxy
Aby usunąć konfigurację serwera proxy w uprzednio zdefiniowanej i przywrócić bezpośrednie połączenie między, usuń plik proxy.conf:
```
sudo rm /etc/opt/microsoft/omsagent/proxy.conf /etc/opt/microsoft/omsagent/conf/proxy.conf
sudo /opt/microsoft/omsagent/bin/service_control restart 
```

## <a name="onboarding-with-log-analytics"></a>Dołączania z analizy dzienników
Jeśli klucz i identyfikator obszaru roboczego nie zostały podane podczas instalacji pakietu, agent musi być następnie zarejestrowany analizy dzienników.

### <a name="onboarding-using-the-command-line"></a>Dołączania przy użyciu wiersza polecenia
Uruchom polecenie omsadmin.sh, podając identyfikator obszaru roboczego i klucz obszaru roboczego. To polecenie musi uruchomić jako główny (z podniesienia uprawnień sudo):
```
cd /opt/microsoft/omsagent/bin
sudo ./omsadmin.sh -w <WorkspaceID> -s <Shared Key>
```

### <a name="register-using-a-file"></a>Zarejestruj za pomocą pliku
1.  Utwórz plik `/etc/omsagent-onboard.conf`. Plik musi być zdatny do odczytu i zapisu dla katalogu głównego.
`sudo vi /etc/omsagent-onboard.conf`
2.  Wstaw plik zawierający identyfikator i klucz wstępny następujące wiersze:

        WORKSPACE_ID=<WorkspaceID>  
        SHARED_KEY=<Shared Key>  
   
3.  Uruchom następujące polecenie, aby zarejestrować analizy dzienników:`sudo /opt/microsoft/omsagent/bin/omsadmin.sh`
4.  Plik zostanie usunięty na pomyślnego dołączenia.

## <a name="enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager"></a>Włącz agenta pakietu OMS Linux zgłoszenia do programu System Center Operations Manager
Wykonaj poniższe kroki, aby skonfigurować agenta pakietu OMS dla systemu Linux z grupy zarządzania programu System Center Operations Manager.  

1. Przeprowadź edycję pliku`/etc/opt/omi/conf/omiserver.conf`
2. Upewnij się, że zaczyna się od wiersza **httpsport =** definiuje portu 1270. Takie jak:`httpsport=1270`
3. Uruchom ponownie serwer OMI:`sudo /opt/omi/bin/service_control restart`

## <a name="agent-logs"></a>Dzienniki agentów
W dziennikach Agent pakietu OMS dla systemu Linux można znaleźć pod: `/var/opt/microsoft/omsagent/<workspace id>/log/` dzienniki programu omsconfig (konfiguracja agenta) można znaleźć w folderze: `/var/opt/microsoft/omsconfig/log/` dzienniki dla składników OMI i SCX (udostępniających dane metryk wydajności) można znaleźć w folderze:`/var/opt/omi/log/ and /var/opt/microsoft/scx/log`

### <a name="log-rotation-configuration"></a>Dziennik obrotu konfiguracji ##
Obróć konfiguracji omsagent dziennika można znaleźć w folderze:`/etc/logrotate.d/omsagent-<workspace id>`

Ustawienia domyślne są: 
```
/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log {
    rotate 5
    missingok
    notifempty
    compress
    size 50k
    copytruncate
}
```

## <a name="uninstalling-the-oms-agent-for-linux"></a>Odinstalowanie agenta pakietu OMS dla systemu Linux
Pakiety agent będzie mogła zostać usunięta przez uruchomienie pliku SH pakietu z `--purge` argumentu, który powoduje całkowite usunięcie agenta i jego konfiguracja z komputera.   

```
> sudo rpm -e omsconfig
> sudo rpm -e omsagent
> sudo /opt/microsoft/scx/bin/uninstall
```

## <a name="troubleshooting"></a>Rozwiązywanie problemów

### <a name="issue-unable-to-connect-through-proxy-to-log-analytics"></a>Problem: Nie można nawiązać połączenia za pośrednictwem serwera proxy do analizy dzienników

#### <a name="probable-causes"></a>Prawdopodobne przyczyny
* Określone podczas dołączania serwera proxy jest niepoprawne
* Analiza dzienników i punktów końcowych usługi Automatyzacja Azure nie są białej w centrum danych. 

#### <a name="resolutions"></a>Rozwiązania
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

### <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Problem: Komunikat o błędzie 403 podczas próby dołączenia

#### <a name="probable-causes"></a>Prawdopodobne przyczyny
* Data i godzina jest nieprawidłowa na serwerze z systemem Linux 
* Identyfikator i klucz obszaru roboczego, używane są nieprawidłowe

#### <a name="resolution"></a>Rozwiązanie

1. Sprawdź czas na serwerze Linux przy użyciu polecenia danych. Jeśli godzina +/-15 minut od bieżącego czasu dołączania kończy się niepowodzeniem. Aby poprawne to zaktualizuj datę i/lub strefy czasowej serwera z systemem Linux. 
2. Sprawdź, czy zainstalowano najnowszą wersję agenta pakietu OMS dla systemu Linux.  Najnowsza wersja teraz powiadamia się, że możesz Jeśli przesunięcia czasowego powoduje błąd przy dołączaniu.
3. Reonboard przy użyciu poprawny identyfikator i klucz obszaru roboczego z instrukcjami instalacji wcześniej w tym temacie.

### <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Problem: Widzisz 500 i 404 błąd w pliku dziennika bezpośrednio po dołączania
Jest to znany problem występujący podczas pierwszego przekazywania danych Linux w obszarze roboczym analizy dzienników. Nie dotyczy to danych wysłanego lub usługa obsługi.

### <a name="issue-you-are-not-seeing-any-data-in-the-azure-portal"></a>Problem: Nie widać żadnych danych w portalu Azure

#### <a name="probable-causes"></a>Prawdopodobne przyczyny

- Dołączenie do usługi analizy dzienników nie powiodło się
- Połączenie z usługą analizy dzienników jest zablokowane
- Agent pakietu OMS dla systemu Linux danych jest wykonywana kopia zapasowa

#### <a name="resolutions"></a>Rozwiązania
1. Sprawdź, czy przechodzenia do usługi analizy dzienników powiodła się przez sprawdzenie, czy istnieje następującego pliku:`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Przy użyciu Reonboard `omsadmin.sh` instrukcje wiersza polecenia
3. Jeśli przy użyciu serwera proxy, odwołują się do serwera proxy kroki rozwiązania, znajdujących się we wcześniejszej.
4. W niektórych przypadkach gdy Agent pakietu OMS dla systemu Linux nie mogą komunikować się z usługą, dane na agencie jest w kolejce do rozmiaru buforu pełnej, czyli 50 MB. Agent pakietu OMS Linux powinien zostać uruchomiony ponownie, uruchamiając następujące polecenie: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Tego problemu w 1.1.0-28 wersję agenta i nowszych.

### <a name="issue-omsagent-creates-excessive-number-of-user-process-on-computer-and-never-terminates-them"></a>Problem: OMSAgent tworzy nadmiernej liczby proces użytkownika na komputerze i nigdy nie kończy je
Po włączeniu rozwiązań do zarządzania, które obsługują zarządzanie maszyn wirtualnych systemu Linux inicjuje liczba procesów na agenta systemu Linux. Ale przed zakończeniem procesu inny proces jest uruchomiony ze względu na znany problem. 

#### <a name="resolutions"></a>Rozwiązania
Aby zmienić liczbę procesów użytkownika, które mogą być generowane przez OMSAgent, należy skonfigurować agenta, przy użyciu omsadmin.sh.  Liczba procesów, które są generowane, domyślnie jest 75 i przed zmianą limit, najpierw należy uruchomić następujące polecenie, aby zobaczyć, jak wiele procesów OMSAgent są aktualnie uruchomione: `ps aux | grep -E '^omsagent' | wc -l`.  
Aby sprawdzić, co bieżący limit jest ustawiony na, uruchamiając następujące polecenie:`cat /etc/security/limits.conf | grep -E '^omsagent'`

Użyj następujących poleceń, albo skonfigurować limit niestandardowy proces lub aby ustawić limit proces jej wartości domyślnej.

1. Aby ustawić limit procesu OMSAgent: `sudo /opt/microsoft/omsagent/bin/omsadmin.sh -n <specific number limit>`.<br>Należy pamiętać, że minimalny limit, który może należeć do 5.  

2. Aby ustawić limit procesu OMSAgent wartość domyślna:`sudo /opt/microsoft/omsagent/bin/omsadmin.sh -N`

Sprawdź, czy nowe ustawienia zostały zastosowane, uruchamiając następujące polecenie: `cat /etc/security/limits.conf | grep -E '^omsagent'`.  Jeśli nie widzisz nowej konfiguracji, które są stosowane, prawdopodobnie można ustawić limitu procesu zbyt niska.  
