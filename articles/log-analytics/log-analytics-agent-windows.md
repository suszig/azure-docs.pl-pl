---
title: "Łączenia komputerów z systemem Windows Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób podłączania komputerów z systemem Windows w innych chmur lub lokalnie z analizą dzienników z programu Microsoft Monitoring Agent (MMA)."
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
ms.date: 12/14/2017
ms.author: magoedte
ms.openlocfilehash: 35e271f943901091041f7b1e9fad6cb9cd46df5b
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/15/2017
---
# <a name="connect-windows-computers-to-the-log-analytics-service-in-azure"></a>Łączenie komputerów z systemem Windows z usługą analizy dzienników na platformie Azure

Aby monitorować i zarządzać nimi maszyn wirtualnych lub fizycznych komputerów w lokalnym centrum danych lub w innym środowisku chmury z analizy dzienników, należy wdrożyć program Microsoft Monitoring Agent (MMA) i skonfigurować go do zgłoszenia do jednego lub więcej analizy dzienników obszarów roboczych.  Agent obsługuje również rolę hybrydowy proces roboczy elementu Runbook automatyzacji Azure.  

Na monitorowanym komputerze z systemem Windows agent będzie wyświetlany jako usługa Microsoft Monitoring Agent. Usługa Microsoft Monitoring Agent zbiera dane zdarzeń z plików dziennika i dziennik zdarzeń systemu Windows, dane dotyczące wydajności i innych danych telemetrycznych. Nawet wtedy, gdy agent nie może skomunikować się z usługą analizy dzienników, któremu ma podlegać, agent będzie kontynuował działanie, a kolejki zebranych danych na dysku monitorowanego komputera. Po przywróceniu połączenia usługa Microsoft Monitoring Agent wysyła zebrane dane do usługi.

Agent może być zainstalowana za pomocą jednej z poniższych metod. Większości instalacji użyj kombinacji tych metod, aby zainstalować różne zestawy komputerów, zależnie od potrzeb.

* Instalacja ręczna. Instalator uruchomić ręcznie na komputerze przy użyciu Kreatora instalacji, w wierszu polecenia lub wdrożyć przy użyciu istniejących narzędzie do dystrybucji oprogramowania.
* Automatyzacja Azure Konfiguracja żądanego stanu (DSC). Przy użyciu usługi Konfiguracja DSC automatyzacji Azure za pomocą skryptu już wdrożony w środowisku komputerów z systemem Windows.  
* Skrypt programu PowerShell.
* Szablon Menedżera zasobów dla maszyn wirtualnych z systemem Windows lokalnymi w stosie Azure.  

Aby poznać wymagania sieci i systemu w celu wdrażania agenta systemu Windows, przejrzyj [zbierać dane ze środowiska z Azure Log Analytics](log-analytics-concept-hybrid.md#prerequisites).

## <a name="obtain-workspace-id-and-key"></a>Uzyskiwanie identyfikatora i klucza obszaru roboczego
Przed zainstalowaniem programu Microsoft Monitoring Agent dla systemu Windows, należy identyfikator i klucz obszaru roboczego analizy dzienników.  Te informacje są wymagane podczas instalacji z każdej metody instalacji, aby poprawnie skonfigurować agenta i upewnij się, że mogą się komunikować z analizy dzienników.  

1. W witrynie Azure Portal kliknij pozycję **Więcej usług** w lewym dolnym rogu. Na liście zasobów wpisz **Log Analytics**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Wybierz pozycję **Log Analytics**.
2. Na liście obszarów roboczych usługi Analiza dzienników wybierz obszar roboczy, które mają na skonfigurowanie agenta zgłoszenia do.
3. Wybierz pozycję **Ustawienia zaawansowane**.<br><br> ![Ustawienia zaawansowane usługi Log Analytics](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. Wybierz **połączonych źródeł**, a następnie wybierz **serwerów z systemem Windows**.   
5. Wartość z prawej strony **identyfikatora obszaru roboczego** i **klucza podstawowego**. Skopiuj i wklej obie wartości do ulubionego edytora.   
   
## <a name="install-the-agent-using-setup"></a>Zainstaluj agenta przy użyciu Instalatora
Poniższe kroki zainstalować i skonfigurować agenta dla analizy dzienników w chmurze Azure oraz Azure dla instytucji rządowych za pomocą Instalatora programu Microsoft Monitoring Agent na tym komputerze.  Program instalacyjny dla agenta jest zawarty w pobrany plik i musi zostać wyodrębniony do 

1. Na **serwerów z systemem Windows** wybierz odpowiedni **Pobierz agenta systemu Windows** wersji do pobrania w zależności od architektury procesora systemu operacyjnego Windows.
2. Uruchom Instalatora, aby zainstalować agenta na komputerze.
2. Na **powitalnej** kliknij przycisk **dalej**.
3. Na **postanowień licencyjnych** , zapoznaj się z licencyjnymi a następnie kliknij przycisk **zgadzam się**.
4. Na **Folder docelowy** strony, zmienić lub zachować domyślny folder instalacji, a następnie kliknij przycisk **dalej**.
5. Na **opcje instalacji agenta** wybierz Połącz agenta z do Analiza dzienników Azure (OMS), a następnie kliknij pozycję **dalej**.   
6. Na **Azure Log Analytics** strony, wykonaj następujące czynności:
   1. Wklej **identyfikator obszaru roboczego** i **klucz obszaru roboczego (klucz podstawowy)** skopiowanego wcześniej.  Jeśli komputer należy zgłosić obszaru roboczego analizy dzienników w chmurze Azure dla instytucji rządowych, wybierz **Azure instytucji rządowych Stanów Zjednoczonych** z **chmury Azure** listy rozwijanej.  
   2. Jeśli komputer musi komunikować się za pośrednictwem serwera proxy z usługą analizy dzienników, kliknij przycisk **zaawansowane** i podaj adres URL i numer portu serwera proxy.  Jeśli serwer proxy wymaga uwierzytelniania, wpisz nazwę użytkownika i hasło do uwierzytelniania za pomocą serwera proxy, a następnie kliknij przycisk **dalej**.  
7. Kliknij przycisk **dalej** po zakończeniu, podając wymagane ustawienia konfiguracyjne.<br><br> ![Wklej klucz podstawowy i identyfikator obszaru roboczego](media/log-analytics-quick-collect-windows-computer/log-analytics-mma-setup-laworkspace.png)<br><br>
8. Na **gotowy do instalacji** strony, przejrzyj wybrane opcje, a następnie kliknij przycisk **zainstalować**.
9. Na **konfiguracji została ukończona pomyślnie** kliknij przycisk **Zakończ**.

Po zakończeniu **programu Microsoft Monitoring Agent** pojawia się w **Panelu sterowania**. Aby upewnić się, to jest raportowanie do analizy dzienników, przejrzyj [zweryfikować łączności agenta analizy dzienników](#verify-agent-connectivity-to-log-analytics). 

## <a name="install-the-agent-using-the-command-line"></a>Zainstaluj agenta przy użyciu wiersza polecenia
Pobrany plik dla agenta jest pakietem instalacyjnym niezależne utworzone za pomocą IExpress.  Program instalacyjny programu agent i pliki pomocnicze są zawarte w pakiecie i wymagają do wyodrębnienia, aby można było poprawnie zainstalować przy użyciu wiersza polecenia pokazano w poniższych przykładach.  Ta metoda obsługuje konfigurowania agenta do zgłaszania Azure handlowych i w chmurze instytucji rządowych Stanów Zjednoczonych.  

>[!NOTE]
>Jeśli chcesz uaktualnić agenta, należy użyć analizy dzienników interfejs API obsługi skryptów. Zobacz temat [zarządzanie i obsługę agenta analizy dzienników systemu Windows i Linux](log-analytics-agent-manage.md) Aby uzyskać więcej informacji.

W poniższej tabeli wymieniono obsługiwane przez Instalatora dla agenta, w tym przypadku wdrożenia przy użyciu usługi Konfiguracja DSC automatyzacji konkretne parametry analizy dzienników.

|Opcje MMA                   |Uwagi         |
|---------------------------------------|--------------|
|ADD_OPINSIGHTS_WORKSPACE               | 1 = Konfigurowanie agenta z obszaru roboczego                |
|OPINSIGHTS_WORKSPACE_ID                | Identyfikator obszaru roboczego (guid) dla obszaru roboczego do dodania                    |
|OPINSIGHTS_WORKSPACE_KEY               | Klucz obszaru roboczego używany do uwierzytelniania początkowo z obszaru roboczego |
|OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE  | Określ środowiska chmury, w którym znajduje się obszar roboczy <br> 0 = komercyjnych chmury azure (ustawienie domyślne) <br> 1 = azure dla instytucji rządowych |
|OPINSIGHTS_PROXY_URL               | Identyfikator URI serwera proxy do użycia |
|OPINSIGHTS_PROXY_USERNAME               | Nazwa użytkownika do uzyskania dostępu uwierzytelnionego serwera proxy |
|OPINSIGHTS_PROXY_PASSWORD               | Hasło dostępu uwierzytelnionego serwera proxy |

1. Aby wyodrębnić pliki instalacyjne agenta, w wierszu polecenia z podwyższonym poziomem uprawnień uruchom `extract MMASetup-<platform>.exe` i zostanie wyświetlony monit dla ścieżki do wyodrębnienia plików.  Alternatywnie można określić ścieżkę przez przekazanie argumentów `extract MMASetup-<platform>.exe /c:<Path> /t:<Path>`.  Aby uzyskać więcej informacji na swtiches wiersza polecenia, obsługiwane przez IExpress, zobacz [przełączniki wiersza polecenia dla IExpress](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) , a następnie zaktualizować przykładu do własnych potrzeb.
2. Aby dyskretnie zainstalować agenta i skonfigurować go do raportu do obszaru roboczego w chmurze komercyjnych Azure, z folderu zostały wyodrębnione pliki instalacyjne do typu: 
   
     ```dos
    setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=0 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
    ```

   lub aby skonfigurować agenta do zgłaszania do chmury Azure instytucji rządowych Stanów Zjednoczonych, wpisz: 

     ```dos
    setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
    ```

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>Zainstaluj agenta przy użyciu usługi Konfiguracja DSC automatyzacji Azure

Poniższy przykładowy skrypt służy do instalowania agenta przy użyciu usługi Konfiguracja DSC automatyzacji Azure.   Jeśli nie masz konta automatyzacji, zobacz [wprowadzenie do usługi Automatyzacja Azure](../automation/automation-offering-get-started.md) zrozumieć wymagania i kroki tworzenia konta automatyzacji wymagana przed rozpoczęciem korzystania z usługi Konfiguracja DSC automatyzacji.  Jeśli nie masz doświadczenia w usłudze Konfiguracja DSC automatyzacji, przejrzyj [wprowadzenie do korzystania z usługi Konfiguracja DSC automatyzacji](../automation/automation-dsc-getting-started.md).

W poniższym przykładzie instalowana agent 64-bitowe, zidentyfikowane przez `URI` wartość. Umożliwia także 32-bitowej wersji zastępując wartość identyfikatora URI. Identyfikatory URI dla obu wersji są:

- Agent 64-bitowego systemu Windows — https://go.microsoft.com/fwlink/?LinkId=828603
- Agent 32-bitowego systemu Windows — https://go.microsoft.com/fwlink/?LinkId=828604


>[!NOTE]
>W tym przykładzie procedury i skrypt nie obsługuje uaktualniania agenta już wdrożony na komputerze z systemem Windows.

32-bitowe i 64-bitowe wersje pakietu agent mają kody innego produktu i nowych wersji wydanych również mieć unikatową wartość.  Kod produktu jest identyfikator GUID, który jest główny identyfikator aplikacji lub produktu i jest reprezentowana przez Instalatora Windows **ProductCode** właściwości.  `ProductId value` w **MMAgent.ps1** skryptu musi odpowiadać kodu produktu z 32-bitowy lub 64-bitowy agent pakiet Instalatora.

Aby pobrać kod produktu pakietu instalacji agenta bezpośrednio, można użyć Orca.exe z [Windows SDK składników dla deweloperów systemu Windows Instalator](https://msdn.microsoft.com/library/windows/desktop/aa370834%27v=vs.85%28.aspx) oznacza to składnik systemu Windows Software Development Kit lub przy użyciu Następujące PowerShell [przykładowy skrypt](http://www.scconfigmgr.com/2014/08/22/how-to-get-msi-file-information-with-powershell/) napisane przez Microsoft cenne Professional (MVP).

1. Importowanie konfiguracji DSC xPSDesiredStateConfiguration modułu na podstawie [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) w automatyzacji Azure.  
2.  Tworzenie zasobów zmiennej usługi Automatyzacja Azure *OPSINSIGHTS_WS_ID* i *OPSINSIGHTS_WS_KEY*. Ustaw *OPSINSIGHTS_WS_ID* do zestawu i identyfikator obszaru roboczego analizy dzienników *OPSINSIGHTS_WS_KEY* w kluczu podstawowym obszaru roboczego.
3.  Skopiuj skrypt i zapisz go jako MMAgent.ps1

    ```PowerShell
    Configuration MMAgent
    {
        $OIPackageLocalPath = "C:\Deploy\MMASetup-AMD64.exe"
        $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
        $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"

        Import-DscResource -ModuleName xPSDesiredStateConfiguration

        Node OMSnode {
            Service OIService
            {
                Name = "HealthService"
                State = "Running"
                DependsOn = "[Package]OI"
            }

            xRemoteFile OIPackage {
                Uri = "https://go.microsoft.com/fwlink/?LinkId=828603"
                DestinationPath = $OIPackageLocalPath
            }

            Package OI {
                Ensure = "Present"
                Path  = $OIPackageLocalPath
                Name = "Microsoft Monitoring Agent"
                ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
                Arguments = '/C:Deploy"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
                DependsOn = "[xRemoteFile]OIPackage"
            }
        }
    }

    ```

4. [Importuj skryptu konfiguracji MMAgent.ps1](../automation/automation-dsc-getting-started.md#importing-a-configuration-into-azure-automation) na koncie automatyzacji. 
5. [Przypisz komputer z systemem Windows lub węzeł](../automation/automation-dsc-getting-started.md#onboarding-an-azure-vm-for-management-with-azure-automation-dsc) konfiguracji. W ciągu 15 minut węzeł sprawdza konfigurację i agent zostanie przypisany do węzła.

## <a name="verify-agent-connectivity-to-log-analytics"></a>Sprawdź łączność agenta do analizy dzienników

Po zakończeniu instalaltion agenta jego weryfikowania został pomyślnie połączony i raportowania można przeprowadzić na dwa sposoby.  

Z komputera w **Panelu sterowania**, Znajdź element **programu Microsoft Monitoring Agent**.  Zaznacz go i na **Analiza dzienników Azure (OMS)** kartę, powinien być wyświetlany agenta komunikat z informacją: **Microsoft Monitoring Agent pomyślnie połączył się z usługą Microsoft Operations Management Suite.**<br><br> ![MMA stanu połączenia analizy dzienników](media/log-analytics-quick-collect-windows-computer/log-analytics-mma-laworkspace-status.png)

Można również wykonywać proste dziennik wyszukiwania w portalu Azure.  

1. W witrynie Azure Portal kliknij pozycję **Więcej usług** w lewym dolnym rogu. Na liście zasobów wpisz **Log Analytics**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Wybierz pozycję **Log Analytics**.  
2. Na stronie obszaru roboczego analizy dzienników, wybierz docelowy obszar roboczy, a następnie wybierz **wyszukiwania dziennika** kafelka. 
2. W okienku wyszukiwania dziennika, w polu wpisz kwerendę:  

    ```
    search * 
    | where Type == "Heartbeat" 
    | where Category == "Direct Agent" 
    | where TimeGenerated > ago(30m)  
    ```

W wynikach wyszukiwania zwróciła powinny zostać wyświetlone rekordy pulsu dla komputera, wskazujący, że jest połączony i raportowania do usługi.   

## <a name="next-steps"></a>Następne kroki

Przegląd [zarządzanie i obsługę agenta analizy dzienników systemu Windows i Linux](log-analytics-agent-manage.md) Aby dowiedzieć się więcej o sposobie zarządzania agentem podczas jego cykl życia wdrożenia na maszynach.  