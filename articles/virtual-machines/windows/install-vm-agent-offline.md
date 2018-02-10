---
title: Zainstaluj agenta maszyny Wirtualnej platformy Azure w trybie Offline | Dokumentacja firmy Microsoft
description: "Dowiedz się, zainstaluj agenta maszyny Wirtualnej platformy Azure w trybie Offline."
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: genli
ms.openlocfilehash: b38bfaffad3e214f3b854715e4d8030cde432bae
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="install-the-vm-agent-in-offline-mode-in-an-azure-windows-vm"></a>Zainstaluj agenta maszyny Wirtualnej w trybie offline w maszynie Wirtualnej Windows Azure 

Agent maszyny Wirtualnej zawiera przydatne funkcje, takie jak Resetowanie hasła administratora lokalnego i wypychania skryptu. W tym artykule przedstawiono sposób instalowania agenta maszyny Wirtualnej dla maszyny Wirtualnej, która jest w trybie offline. 

## <a name="when-to-use-offline-mode"></a>Kiedy należy używać w trybie offline

Musisz zainstalować agenta maszyny Wirtualnej w tryb offline w następującym scenariuszu:

- Można wdrożyć maszyny Wirtualnej platformy Azure, która jest nie ma agenta maszyny Wirtualnej zainstalowane lub nie działa agent maszyny Wirtualnej.
- Nie pamiętasz hasła administratora lub nie masz dostępu do maszyny Wirtualnej.

W tym scenariuszu należy zainstalować agenta maszyny Wirtualnej w trybie offline. 



## <a name="detailed-steps"></a>Szczegółowe procedury

**Krok 1: Dołączyć dysk systemu operacyjnego maszyny wirtualnej do innej maszyny Wirtualnej jako dysk z danymi**

1.  Usuń maszynę Wirtualną. Upewnij się, że wybrano **zachować dyski** przy tym.

2.  Dołącz dysk systemu operacyjnego jako dysku danych do innej maszyny Wirtualnej (Rozwiązywanie problemów z maszyny Wirtualnej). Aby uzyskać więcej informacji, zobacz [How to attach a data disk to a Windows VM in the Azure portal](attach-managed-disk-portal.md) (Jak dołączyć dysk danych do maszyny wirtualnej z systemem Windows w witrynie Azure Portal).

3.  Nawiąż połączenie z maszyną wirtualną rozwiązywania problemów. Otwórz **Zarządzanie komputerem** > **dysku zarządzania**. Upewnij się, że dysk systemu operacyjnego jest w trybie online i że jego partycji litery dysków przypisane.

**Krok 2: Modyfikowanie dysk systemu operacyjnego do zainstalowania agenta maszyny Wirtualnej**

1.  Należy podłączania pulpitu zdalnego na rozwiązywanie problemów dotyczących maszyny Wirtualnej.

2.  Na dysku systemu operacyjnego w przypadku dołączenia, przejdź do **\windows\system32\config**. Skopiuj wszystkie pliki jako zapasowy, w przypadku wycofywania jest wymagana.

3.  Uruchom Edytor rejestru (regedit.exe).

4.  Kliknij przycisk **HKEY_LOCAL_MACHINE** klucza, a następnie wybierz **pliku** > **Hive obciążenia** w menu.

    ![Gałąź obciążenia](./media/install-vm-agent-offline/load-hive.png)

5.  Przejdź do **\windows\system32\config\SYSTEM** na system operacyjny tego zostanie dołączony na dysku, a następnie wpisz BROKENSYSTEM jako nazwa gałęzi. Po wykonaniu tej czynności zobaczą rejestru hive w obszarze **HKEY_LOCAL_MACHINE**.

6.  Przejdź do **\windows\system32\config\SOFTWARE** na dysku systemu operacyjnego w przypadku dołączenia, wpisz nazwę gałęzi BROKENSOFTWARE.

7.  Jeśli masz bieżąca wersja agenta, która nie działa, wykonaj kopię zapasową bieżącej konfiguracji. Jeśli maszyna wirtualna nie ma zainstalowanego agenta maszyny Wirtualnej, przejdź do następnego kroku.  
      
    1. Zmień nazwę folderu \windowsazure na \windowsazure.old

    2. Eksportuj następujące rejestrów
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE \BROKENSYSTEM\\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\RdAgent

8.  Użyj istniejących plików w przypadku rozwiązywania problemów z maszyny Wirtualnej jako repozytorium instalacji agenta maszyny Wirtualnej: 

    1. Z rozwiązywania maszyny Wirtualnej należy wyeksportować poniższe podklucze w formacie rejestru (reg): 

        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\RdAgent

        ![Obraz o eksportowanie kluczy rejestru](./media/install-vm-agent-offline/backup-reg.png)

    2. Edytuj pliki te trzy rejestru, zmień **systemu** klucza wpisu **BROKENSYSTEM**, a następnie zapisz plik.
        ![Obraz o zmianę kluczy rejestru](./media/install-vm-agent-offline/change-reg.png)
    3. Dwukrotne kliknięcie plików rejestru do ich zaimportowania.
    4. Upewnij się, że następujące klucze są pomyślnie zaimportowane do gałęzi BROKENSYSTEM: WindowsAzureGuestAgent, WindowsAzureTelemetryService i RdAgent.

9.  Skopiuj folder agenta maszyny Wirtualnej z C:\windowsazure\packages do &lt;dysk systemu operacyjnego, który można dołączyć&gt;: \windowsazure\packages.
    ![Obraz o kopiowanie plików](./media/install-vm-agent-offline/copy-package.png)
      
    **Uwaga** nie należy kopiować folderu dzienników. Po uruchomieniu usługi nowe dzienniki będą generowane.

10.  Kliknij BROKENSYSTEM i wybierz **pliku** > **Zwolnij gałąź rejestru** z menu.

11.  Kliknij BROKENSOFTWARE i wybierz **pliku** > **Zwolnij gałąź rejestru** z menu.

12.  Odłącz dysk, a następnie utwórz ponownie maszynę Wirtualną za pomocą dysku systemu operacyjnego.

13.  Jeśli dostęp do maszyny Wirtualnej znajduje się teraz RDAgent systemem i dzienniki pobierania tworzone.

14. Jeśli jest to maszyny Wirtualnej utworzonej przy użyciu klasycznego modelu wdrażania, wszystko będzie gotowe. Jednak jeśli jest to maszyny Wirtualnej utworzonej przy użyciu modelu wdrażania usługi Resource Manager, należy również użyć programu Azure PowerShell można zaktualizować właściwości ProvisionGuestAgent, tak że Azure wie, że maszyna wirtualna jest zainstalowany agent. Aby to zrobić, uruchom następujące polecenia w programie Azure PowerShell:

        $vm = Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
        $vm.VM.ProvisionGuestAgent = $true
        Update-AzureVM –Name <VM name> –VM $vm.VM –ServiceName <cloud service name>

    Po uruchomieniu **Get-AzureVM** po wykonaniu tych kroków **GuestAgentStatus** właściwości powinny zostać wypełnione zamiast wyświetlenie pustej strony:

        Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
        GuestAgentStatus:Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVMModel.GuestAgentStatus

## <a name="next-steps"></a>Kolejne kroki

- [Omówienie usługi Azure agenta maszyny wirtualnej](agent-user-guide.md)
- [Rozszerzenia maszyn wirtualnych i funkcje systemu Windows](extensions-features.md)