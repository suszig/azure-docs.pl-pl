---
title: Przygotowanie komputera-hosta Azure stosu Development Kit (ASDK) | Dokumentacja firmy Microsoft
description: Zawiera opis sposobu przygotowania komputera-hosta Azure stosu Development Kit (ASDK) ASDK instalacji.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 8b1a6298ab32dc364aa1543e4a8d5db47b02a098
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="prepare-the-asdk-host-computer"></a>Przygotuj komputer-host ASDK
Przed zainstalowaniem ASDK na komputerze-hoście, należy przygotować środowisko ASDK dla instalacji. Gdy na komputerze deweloperskim zestaw hosta zostały przygotowane, uruchomi się z dysku twardego maszyny wirtualnej CloudBuilder.vhdx, aby rozpocząć wdrażanie ASDK.

## <a name="prepare-the-development-kit-host-computer"></a>Przygotowanie komputera-hosta development kit
Przed zainstalowaniem ASDK na komputerze-hoście, należy przygotować środowisko komputera hosta ASDK.
1. Zaloguj się jako Administrator lokalny na komputerze hosta development kit.
2. Upewnij się, że plik CloudBuilder.vhdx został przeniesiony do katalogu głównego dysku C:\ (C:\CloudBuilder.vhdx).
3. Uruchom następujący skrypt, aby pobrać plik Instalatora development kit (asdk installer.ps1) z [repozytorium narzędzia Azure stosu GitHub](https://github.com/Azure/AzureStack-Tools) do **C:\AzureStack_Installer** folderu na użytkownika Development kit hosta komputera:

  ```powershell
  # Variables
  $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/asdk-installer.ps1'
  $LocalPath = 'C:\AzureStack_Installer'
  # Create folder
  New-Item $LocalPath -Type directory
  # Download file
  Invoke-WebRequest $uri -OutFile ($LocalPath + '\' + 'asdk-installer.ps1')
  ```

4. Z konsoli programu PowerShell z podwyższonym poziomem uprawnień uruchom **C:\AzureStack_Installer\asdk-installer.ps1** skrypt, a następnie kliknij przycisk **przygotować środowisko**.

    ![](media/asdk-prepare-host/1.PNG) 

5. Na **vhdx wybierz Cloudbuilder** strony Instalatora, Znajdź i wybierz **cloudbuilder.vhdx** pliku, który pobrane i wyodrębnione w [poprzednie kroki](asdk-download.md). Na tej stronie można także opcjonalnie włączyć **dodać sterowniki** pole wyboru, aby dodać sterowniki do komputera hosta development kit. Kliknij przycisk **Dalej**.  

    ![](media/asdk-prepare-host/2.PNG)

6. Na **opcjonalne ustawienia** administrator lokalny Podaj informacje o koncie dla rozwoju zestawu hosta, a następnie kliknij przycisk **dalej**. Można również podać wartości opcjonalne następujące ustawienia:
  - **ComputerName**: Ta opcja umożliwia ustawienie nazwy hosta development kit. Nazwa musi spełniać wymagania dotyczące w pełni kwalifikowaną nazwę domeny i musi być 15 znaków lub mniej długości. Wartość domyślna to losową nazwę komputera wygenerowany przez system Windows.
  - **Strefa czasowa**: ustawia strefę czasową dla hosta development kit. Wartość domyślna to (UTC-8:00) czas pacyficzny (USA i Kanada).
  - **Konfiguracji statycznych adresów IP**: ustawia danego wdrożenia, aby użyć statycznego adresu IP. W przeciwnym razie gdy Instalator wykonuje ponowny rozruch w cloudbuilder.vhx, interfejsów sieciowych są skonfigurowane przy użyciu protokołu DHCP.

    ![](media/asdk-prepare-host/3.PNG)

  > [!IMPORTANT]
  > Jeśli nie zostaną podane poświadczenia administratora lokalnego, w tym kroku, należy bezpośrednio lub KVM dostęp do hosta po ponownym uruchomieniu komputera w ramach konfigurowania zestaw deweloperski.

7. Jeśli w poprzednim kroku wybrano konfiguracji statycznych adresów IP, należy teraz:
    - Wybierz kartę sieciową. Upewnij się, że możesz nawiązać połączenie karty przed kliknięciem przycisku **dalej**.
    - Upewnij się, że **adres IP**, **bramy**, i **DNS** wartości są poprawne, a następnie kliknij przycisk **dalej**.
13. Kliknij przycisk **dalej** można uruchomić proces przygotowywania.
14. Po przygotowaniu wskazuje **Ukończono**, kliknij przycisk **dalej**.
15. Kliknij przycisk **ponowny rozruch teraz** uruchomić na komputerze deweloperskim zestaw hosta cloudbuilder.vhdx i [kontynuować proces wdrażania](asdk-install.md).

    ![](media/asdk-prepare-host/4.PNG)


## <a name="next-steps"></a>Kolejne kroki
[Zainstaluj ASDK](asdk-install.md)