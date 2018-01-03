---
title: "Integrowanie Azure DevTest Labs programu VSTS ciągłej integracji i dostarczania potoku | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zintegrować Azure DevTest Labs programu VSTS ciągłej integracji i dostarczania potoku"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: a26df85e-2a00-462b-aac1-dd3539532569
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: tarcher
ms.openlocfilehash: 3cd6939e890f59e7c5b188edd8620da5c9fcc935
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/19/2017
---
# <a name="integrate-azure-devtest-labs-into-your-vsts-continuous-integration-and-delivery-pipeline"></a>Integrowanie Azure DevTest Labs programu VSTS ciągłej integracji i dostarczania potoku
Można użyć **rozszerzenie Azure DevTest Labs zadania** zainstalowany w Visual Studio Team Services (VSTS) łatwo zintegrować kompilacji elementu konfiguracji/CD i wersji potoku o usłudze Azure DevTest Labs. Rozszerzenie instaluje trzy zadania, aby utworzyć Maszynę wirtualną, utworzyć niestandardowy obraz z maszyny Wirtualnej i usuwania maszyny Wirtualnej. Ten proces można łatwo, na przykład szybko wdrożyć "złotego obrazu" dla specyficznego testu zadania, a następnie usuń ją po zakończeniu testu.

W tym artykule pokazano, jak utworzyć i wdrożyć maszynę Wirtualną, utworzyć niestandardowy obraz, a następnie usuń maszynę Wirtualną, a wszystkie jako jeden pełny potok. Zwykle jednak należy użyć zadania indywidualnie w potoku kompilacji — Testowanie wdrażania własnych niestandardowych.

## <a name="before-you-begin"></a>Przed rozpoczęciem
Zanim będzie można zintegrować z potoku CI/CD z Azure DevTest Labs, należy najpierw zainstalować rozszerzenie z Visual Studio Marketplace.
1. Przejdź do [Azure DevTest Labs zadania](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks)
1. Wybierz opcję instalacji
1. Wykonaj kroki kreatora

## <a name="create-a-resource-manager-template"></a>Tworzenie szablonu usługi Resource Manager
W tej sekcji opisano sposób tworzenia szablonu usługi Azure Resource Manager, który jest używany do utworzenia maszyny wirtualnej platformy Azure na żądanie.

1. Wykonaj kroki opisane w temacie [użyć szablonu usługi Resource Manager](devtest-lab-use-resource-manager-template.md) do tworzenia szablonu usługi Resource Manager w ramach subskrypcji.
1. Dodaj [artefaktu WinRM](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-winrm) podczas tworzenia maszyny Wirtualnej (przed wygenerowaniem szablonu usługi Resource Manager).

   WinRM dostępu jest wymagany do użycia wdrażanie zadań takich jak **Azure File Copy** i **programu PowerShell na komputerach docelowych**.

   > [!NOTE]
   > Korzystając z usługi WinRM przy użyciu adresu IP udostępnionego, należy dodać regułę NAT do mapowania portu zewnętrznego portu usługi WinRM. Nie jest to wymagane w przypadku maszyny Wirtualnej jest tworzona przy użyciu adresu publicznego adresu IP.
   >
   >

1. Zapisz szablon jako plik na komputerze. Nadaj nazwę plikowi **CreateVMTemplate.json**.
1. Sprawdź szablon do systemu kontroli źródła.
1. Otwórz Edytor tekstu, a następnie skopiuj do niego poniższy skrypt.

   ```powershell
   Param( [string] $labVmId)

   $labVmComputeId = (Get-AzureRmResource -Id $labVmId).Properties.ComputeId

   # Get lab VM resource group name
   $labVmRgName = (Get-AzureRmResource -Id $labVmComputeId).ResourceGroupName

   # Get the lab VM Name
   $labVmName = (Get-AzureRmResource -Id $labVmId).Name

   # Get lab VM public IP address
   $labVMIpAddress = (Get-AzureRmPublicIpAddress -ResourceGroupName $labVmRgName
                   -Name $labVmName).IpAddress

   # Get lab VM FQDN
   $labVMFqdn = (Get-AzureRmPublicIpAddress -ResourceGroupName $labVmRgName
              -Name $labVmName).DnsSettings.Fqdn

   # Set a variable labVmRgName to store the lab VM resource group name
   Write-Host "##vso[task.setvariable variable=labVmRgName;]$labVmRgName"

   # Set a variable labVMIpAddress to store the lab VM Ip address
   Write-Host "##vso[task.setvariable variable=labVMIpAddress;]$labVMIpAddress"

   # Set a variable labVMFqdn to store the lab VM FQDN name
   Write-Host "##vso[task.setvariable variable=labVMFqdn;]$labVMFqdn"
   ```

1. Sprawdź skrypt w systemie kontroli źródła. Nazwa polecenie podobne do następującego **GetLabVMParams.ps1**.

   Ten skrypt uruchomienia agenta jako część definicji wersji zbiera wartości, które są potrzebne do wdrożenia aplikacji na maszynie wirtualnej, korzystając z kroków zadań takich jak **Azure File Copy** lub **programu PowerShell na komputerach docelowych**. Zazwyczaj używają tych zadań do wdrażania aplikacji na maszynie Wirtualnej platformy Azure i wymagają wartości, takie jak nazwa grupy zasobów maszyny Wirtualnej, adres IP i nazwy FQDN (FDQN).

## <a name="create-the-release-definition-in-release-management"></a>Tworzenie definicji wersji w Release Management
Wykonaj następujące kroki, aby utworzyć definicję wersji.

1. Otwórz **wersje** karcie **kompilacji i wydania** koncentratora i wybierz polecenie "**+**" ikony.
1. W **Tworzenie wersji definicji** okno dialogowe, wybierz opcję **pusty** szablonu i wybierz polecenie **dalej**.
1. Na następnej stronie wybierz **Wybierz później** , a następnie wybierz **Utwórz** do tworzenia nowej definicji wersji ze środowiskiem jeden domyślny i nie ma połączonego artefaktów.
1. W nowej wersji definicji, wybierz wielokropek (...) obok nazwy środowiska, aby otworzyć menu skrótów i wybierz **skonfigurować zmienne**. 
1. W **Konfiguruj - środowiska** okna dialogowego, wprowadź następujące wartości zmiennych w wersji definicji zadania:
   - **vmName**: Wprowadź nazwę przypisaną do maszyny Wirtualnej, podczas tworzenia szablonu usługi Resource Manager w portalu Azure.
   - **Nazwa użytkownika**: Wprowadź nazwę użytkownika przypisanych do maszyny Wirtualnej, podczas tworzenia szablonu usługi Resource Manager w portalu Azure.
   - **hasło**: Wprowadź hasło przypisane do maszyny Wirtualnej, podczas tworzenia szablonu usługi Resource Manager w portalu Azure. Użyj ikony "kłódki", aby ukryć i bezpieczne hasło.

   <a name="create-a-vm"></a>Tworzenie maszyny wirtualnej
   ---

   Pierwszy etap w tym wdrożeniu jest aby utworzyć maszynę Wirtualną do użycia jako "złotego obrazu" kolejnych wdrożeń. Możesz utworzyć maszyny Wirtualnej w ramach wystąpienia laboratorium DevTest Lab Azure za pomocą zadania opracowane specjalnie do tego celu. W definicji wersji, wybierz **+ Dodaj zadania** i Dodaj **Utwórz maszynę Wirtualną programu Azure DevTest Labs** zadania na karcie wdrażania. Skonfiguruj w następujący sposób:

   Zobacz [Azure DevTest Labs zadania](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) Aby utworzyć maszynę Wirtualną do użycia podczas kolejnych wdrożeń.
   - **Subskrypcja platformy Azure RM**: Wybierz połączenie z listy w obszarze **dostępne połączenia usługi Azure** lub utworzyć połączenie bardziej ograniczone uprawnienia do subskrypcji platformy Azure. Aby uzyskać więcej informacji, zobacz [punktu końcowego usługi Azure Resource Manager](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
   - **Nazwa laboratorium**: Wybierz nazwę wystąpienia utworzone wcześniej.
   - **Nazwa szablonu**: Wprowadź pełną ścieżkę i nazwę pliku szablonu zapisany w repozytorium kodu źródłowego. Wbudowane właściwości zarządzania zleceniami służy do uproszczenia ścieżki, na przykład:
      ```
      $(System.DefaultWorkingDirectory)/Contoso/ARMTemplates/CreateVMTemplate.json
      ```
   - **Parametry szablonu**: wprowadź parametry zmienne zdefiniowane w szablonie. Użyj nazwy zmiennych zdefiniowanych w środowisku, na przykład:
      ```
      -newVMName '$(vmName)' -userName '$(userName)' -password (ConvertTo-SecureString -String '$(password)' -AsPlainText -Force)
      ```
   - **Dane wyjściowe zmienne — identyfikator maszyny Wirtualnej laboratorium**: identyfikator nowo utworzona maszyna wirtualna jest wymagane dla kolejnych krokach. Ustawiono domyślną nazwę zmiennej środowiskowej, która jest automatycznie wypełniane przy użyciu tego Identyfikatora **zmienne wyjściowe** sekcji. Można edytować zmiennej, jeśli to konieczne, ale pamiętaj, aby używać poprawnej nazwy w kolejnych zadań. Identyfikator maszyny Wirtualnej laboratorium ma postać:
      ```
      /subscriptions/{subId}/resourceGroups/{rgName}/providers/Microsoft.DevTestLab/labs/{labName}/virtualMachines/{vmName}
      ```
1. Uruchom skrypt został utworzony wcześniej, aby zbierać dane maszyny wirtualnej DevTest Labs. W definicji wersji, wybierz **+ Dodaj zadania** i Dodaj **programu Azure PowerShell** zadań z **Wdróż** kartę. Skonfiguruj zadania w następujący sposób:

   Zobacz [Wdróż: programu Azure PowerShell](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/AzurePowerShell) i zbierać dane maszyny wirtualnej DevTest Labs, uruchom skrypt.

   - **Typ połączenia Azure**: Usługa Azure Resource Manager.
   - **Subskrypcja platformy Azure RM**: Wybierz połączenie z listy w obszarze **dostępne połączenia usługi Azure** lub utworzyć połączenie bardziej ograniczone uprawnienia do subskrypcji platformy Azure. Aby uzyskać więcej informacji, zobacz [punktu końcowego usługi Azure Resource Manager](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
   - **Typ skryptu**: plik skryptu.
   - **Ścieżka skryptu**: Wprowadź pełną ścieżkę i nazwę skryptu zapisany w repozytorium kodu źródłowego. Wbudowane właściwości zarządzania zleceniami służy do uproszczenia ścieżki, na przykład:
      ```
      $(System.DefaultWorkingDirectory/Contoso/Scripts/GetLabVMParams.ps1
      ```
   - **Argumenty skryptu**: jako argument skryptu wprowadź nazwę zmiennej środowiskowej, która została automatycznie wypełniane przy użyciu Identyfikator laboratorium maszyny Wirtualnej przez poprzednie zadanie, na przykład: 
      ```
      -labVmId '$(labVMId)'
      ```
   Skrypt zbiera wymagane wartości i przechowuje je w zmiennych środowiskowych w ramach definicji wersji, dzięki czemu można łatwo udostępnić im w krokach kolejnych zadań.

1. Teraz można wdrożyć aplikację do nowej maszyny Wirtualnej DevTest Labs. Zadania zwykle używana do wdrażania są **Azure File Copy** i **programu PowerShell na komputerach docelowych**.
   - Informacje o maszynie Wirtualnej, należy dla parametrów tych zadań są przechowywane w trzech zmiennych konfiguracji o nazwie **labVmRgName**, **labVMIpAddress**, i **labVMFqdn**w ramach definicji wersji.
   - Jeśli chcesz wypróbować tworzenia maszyny Wirtualnej DevTest Labs i obraz niestandardowy bez wdrażania aplikacji, możesz pominąć ten krok.

   <a name="create-an-image"></a>Tworzenie obrazu
   ---

   Następny etap jest można utworzyć obrazu maszyny wirtualnej w nowo wdrożonym w usłudze Azure DevTest Labs wystąpienia. Następnie można ten obraz tworzenia kopii maszyny Wirtualnej na żądanie, zawsze, gdy chcesz wykonać zadanie deweloperów lub uruchamianie niektórych testów. W definicji wersji, wybierz **+ Dodaj zadania** i Dodaj **utworzyć obraz niestandardowy Azure DevTest Labs** zadań z **Wdróż** kartę. Skonfiguruj w następujący sposób:

   Zobacz [Azure DevTest Labs zadania](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) do utworzenia obrazu.
   - **Subskrypcja platformy Azure RM**: Wybierz połączenie z listy w obszarze **dostępne połączenia usługi Azure** lub utworzyć połączenie bardziej ograniczone uprawnienia do subskrypcji platformy Azure. Aby uzyskać więcej informacji, zobacz [punktu końcowego usługi Azure Resource Manager](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
   - **Nazwa laboratorium**: Wybierz nazwę wystąpienia utworzone wcześniej.
   - **Nazwa niestandardowego obrazu**: Wprowadź nazwę dla niestandardowego obrazu.
   - **Opis elementu**: Opcjonalnie wprowadź opis, aby ułatwić później wybierz poprawny obraz.
   - **Źródło laboratorium maszyny Wirtualnej — identyfikator maszyny Wirtualnej laboratorium źródła**: zmiana domyślną nazwę zmiennej środowiskowej, która została automatycznie wypełniona o identyfikatorze laboratorium maszyny Wirtualnej przez wcześniejszych zadań go edytować w tym miejscu. Wartość domyślna to *$(labVMId)*.
   - **Dane wyjściowe zmienne - identyfikator obrazu niestandardowego**: należy identyfikator nowo utworzony obraz, jeśli chcesz zarządzać, lub usuń go. Ustawiono domyślną nazwę zmiennej środowiskowej, która jest automatycznie wypełniane przy użyciu tego Identyfikatora **zmienne wyjściowe** sekcji. W razie potrzeby można edytować zmiennej.
   
   <a name="delete-the-vm"></a>Usuń maszynę Wirtualną
   ---
   
   Ostatnim krokiem w tym przykładzie jest można usunąć maszyny Wirtualnej wdrożone w usłudze Azure DevTest Labs wystąpienia. Zazwyczaj należy usunąć maszynę Wirtualną, po wykonanie zadań deweloperów lub uruchom testy, które są potrzebne w przypadku wdrożonej maszyny Wirtualnej. W definicji wersji, wybierz **+ Dodaj zadania** i Dodaj **usunąć wirtualna Azure DevTest Labs** zadań z **Wdróż** kartę. Skonfiguruj w następujący sposób:

   Zobacz [Azure DevTest Labs zadania](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) można usunąć maszyny Wirtualnej.
   - **Subskrypcja platformy Azure RM**: Wybierz połączenie z listy w obszarze **dostępne połączenia usługi Azure** lub utworzyć połączenie bardziej ograniczone uprawnienia do subskrypcji platformy Azure. Aby uzyskać więcej informacji, zobacz [punktu końcowego usługi Azure Resource Manager](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
   - **Identyfikator maszyny Wirtualnej laboratorium**: zmiana domyślną nazwę zmiennej środowiskowej, która została automatycznie wypełniona o identyfikatorze laboratorium maszyny Wirtualnej przez wcześniejszych zadań go edytować w tym miejscu. Wartość domyślna to *$(labVMId)*.
1. Wprowadź nazwę dla definicji wersji i zapisz go.
1. Utworzyć nową wersję, wybierz ostatniej kompilacji, a następnie wdrożyć go na jednym środowiskiem w definicji.

Na każdym etapie Odśwież widok wystąpienia DevTest Labs w portalu Azure, maszyny Wirtualnej i tworzenia obrazu i usuwane ponownie maszyny Wirtualnej.

Obraz niestandardowy można teraz używać do tworzenia maszyn wirtualnych, gdy jest to wymagane.


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Kolejne kroki
* Dowiedz się, jak [utworzyć środowiska wielu maszyn wirtualnych z szablonami usługi Resource Manager](devtest-lab-create-environment-from-arm.md).
* Poznaj więcej szablonów usługi Resource Manager szybkiego startu DevTest Labs automatyzacji z [publicznego repozytorium DevTest Labs GitHub](https://github.com/Azure/azure-quickstart-templates).
* W razie potrzeby zajrzyj [VSTS Rozwiązywanie problemów z](https://docs.microsoft.com/vsts/build-release/actions/troubleshooting) strony.
