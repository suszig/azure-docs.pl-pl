---
title: "Rozwiązywanie problemów z Azure Cloud powłoki (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
description: "Rozwiązywanie problemów z powłoki w chmurze Azure"
services: azure
documentationcenter: 
author: maertendMSFT
manager: angelc
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/2/2017
ms.author: damaerte
ms.openlocfilehash: 89d5d8df9327c6136fbd00078f6a34f78d85032e
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="troubleshooting-azure-cloud-shell"></a>Rozwiązywanie problemów z powłoki w chmurze Azure

Następujące znane rozwiązania problemów w powłoce chmury Azure:

## <a name="general-resolutions"></a>Ogólne rozwiązania

### <a name="storage-dialog---error-403-requestdisallowedbypolicy"></a>Okno dialogowe magazynu — błąd: 403 RequestDisallowedByPolicy
- **Szczegóły**: podczas tworzenia konta magazynu za pośrednictwem chmury powłoki, to nie powiodło się z powodu zasad Azure wprowadzane przez administratora. Zawiera komunikat o błędzie:`The resource action 'Microsoft.Storage/storageAccounts/write' is disallowed by one or more policies.`
- **Rozdzielczość**: Skontaktuj się z administratorem usługi Azure, aby usunąć lub zaktualizować zasady Azure zezwalających na utworzenie magazynu.

### <a name="storage-dialog---error-400-disallowedoperation"></a>Okno dialogowe magazynu — błąd: 400 DisallowedOperation
 - **Szczegóły**: podczas korzystania z subskrypcji usługi Azure Active Directory, nie można utworzyć magazynu.
 - **Rozdzielczość**: Korzystanie z subskrypcji platformy Azure umożliwia tworzenie zasobów magazynu. Subskrypcje platformy Azure AD nie będą mogli tworzyć zasobów platformy Azure.

### <a name="terminal-output---error-failed-to-connect-terminal-websocket-cannot-be-established-press-enter-to-reconnect"></a>Terminal output — błąd: nie można nawiązać połączenia terminal: obiektu websocket nie powiodło się. Naciśnij klawisz `Enter` połączyć się ponownie.
 - **Szczegóły**: powłoki chmury wymaga możliwość ustanowienia połączenia obiektu websocket infrastruktury chmury powłoki.
 - **Rozdzielczość**: Sprawdź skonfigurowano ustawienia sieci, aby umożliwić wysyłanie żądań https i żądania protokołu websocket do domen na *. console.azure.com.

## <a name="bash-resolutions"></a>Bash rozwiązania

### <a name="cannot-run-az-login"></a>Nie można uruchomić az logowania

- **Szczegóły**: systemem `az login` nie będzie działać, ponieważ zostali już uwierzytelnieni w ramach konta używane do logowania się do portalu powłoki chmury lub Azure.
- **Rozdzielczość**: Korzystanie z konta, używany do podpisywania lub wyloguj się i ponownego uwierzytelnienia przy użyciu danego konta platformy Azure.

### <a name="cannot-run-the-docker-daemon"></a>Nie można uruchomić demona docker

- **Szczegóły**: powłoki chmury korzysta z kontenera do hostowania środowiska powłoki, w związku z tym systemem demona jest niedozwolone.
- **Rozdzielczość**: Korzystanie z [docker maszyny](https://docs.docker.com/machine/overview/), który jest instalowany domyślnie, aby zarządzać kontenery docker z hostem zdalnym Docker.

## <a name="powershell-resolutions"></a>Rozwiązania programu PowerShell

### <a name="no-home-directory-persistence"></a>Trwałość katalogu No $Home

- **Szczegóły**: wszystkie dane tej aplikacji (takich jak: git, vim i inne) zapisuje `$Home` nie jest zachowywane między sesjami programu PowerShell.
- **Rozdzielczość**: W profilu programu PowerShell, należy utworzyć łącze symboliczne do folderu określonej aplikacji w `clouddrive` do $Home.

### <a name="ctrlc-doesnt-exit-out-of-a-cmdlet-prompt"></a>CTRL + C nie powodować wyjście z wiersza polecenia Cmdlet

- **Szczegóły**: podczas próby zamknąć wiersz polecenia Cmdlet `Ctrl+C` monit nie zakończyć.
- **Rozdzielczość**: aby wyjść z wiersza, naciśnij klawisz `Ctrl+C` następnie `Enter`.

### <a name="gui-applications-are-not-supported"></a>Graficzny interfejs użytkownika aplikacji nie są obsługiwane.

- **Szczegóły**: Jeśli użytkownik uruchamia aplikację graficznego interfejsu użytkownika, monit nie zwraca. Na przykład gdy użytkownik klonów prywatne repozytorium GitHub, który jest włączone uwierzytelnianie dwuskładnikowe, ukończenia uwierzytelniania dwuskładnikowego zostanie wyświetlone okno dialogowe.
- **Rozdzielczość**: `Ctrl+C` aby zakończyć działanie polecenia.

### <a name="get-help--online-does-not-open-the-help-page"></a>Get-Help - online nie powoduje otwarcia strony pomocy

- **Szczegóły**: Jeśli użytkownik wpisze `Get-Help Find-Module -online`, takich jak jedną zobaczy komunikat o błędzie:`Starting a browser to display online Help failed. No program or browser is associated to open the URI http://go.microsoft.com/fwlink/?LinkID=398574.`
- **Rozdzielczość**: Skopiuj adres url, a następnie otwórz go ponownie ręcznie w przeglądarce.

### <a name="troubleshooting-remote-management-of-azure-vms"></a>Rozwiązywanie problemów z zdalne zarządzanie maszynami wirtualnymi platformy Azure

- **Szczegóły**: z powodu domyślne ustawienia zapory systemu Windows dla usługi WinRM użytkownik może się pojawić następujący błąd:`Ensure the WinRM service is running. Remote Desktop into the VM for the first time and ensure it can be discovered.`
- **Rozdzielczość**: Upewnij się, że maszyna wirtualna jest uruchomiona. Można uruchomić `Get-AzureRmVM -Status` Aby sprawdzić stan maszyny Wirtualnej.  Następnie dodaj nową regułę zapory na Maszynie wirtualnej zdalnego umożliwia nawiązywanie połączeń usługi WinRM z dowolnej podsieci, na przykład

 ``` Powershell
 New-NetFirewallRule -Name 'WINRM-HTTP-In-TCP-PSCloudShell' -Group 'Windows Remote Management' -Enabled True -Protocol TCP -LocalPort 5985 -Direction Inbound -Action Allow -DisplayName 'Windows Remote Management - PSCloud (HTTP-In)' -Profile Public
 ```
 Można użyć [Azure niestandardowe rozszerzenie skryptu](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-customscript) w celu uniknięcia logowania do zdalnego maszyny Wirtualnej do dodawania nowej reguły zapory.
 Powyższy skrypt do pliku, można zapisać powiedz `addfirerule.ps1`i przekaż go do Twojego kontenera magazynu systemu Azure.
 Spróbuj wykonać następujące polecenie:

 ``` Powershell
 Get-AzureRmVM -Name MyVM1 -ResourceGroupName MyResourceGroup | Set-AzureRmVMCustomScriptExtension -VMName MyVM1 -FileUri https://mystorageaccount.blob.core.windows.net/mycontainer/addfirerule.ps1 -Run 'addfirerule.ps1' -Name myextension
 ```

### <a name="dir-caches-the-result-in-azure-drive"></a>`dir`buforuje wynik dysku platformy Azure

- **Szczegóły**: wynik `dir` są buforowane na dysku platformy Azure.
- **Rozdzielczość**: po utworzeniu lub usunięcie zasobu w widoku dysku platformy Azure Uruchom `dir -force` do aktualizacji.
