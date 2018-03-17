---
title: "Wdrażanie Azure stosu Development Kit | Dokumentacja firmy Microsoft"
description: "W tym samouczku należy zainstalować ASDK korzystające ze skryptów Instalatora."
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
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: de31ed75b79ddb3832e32fad141ea7aef806d4d3
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-deploy-the-asdk-using-the-installer"></a>Samouczek: Wdrażanie ASDK za pomocą Instalatora
W tym samouczku wdrożeniem zestawu programowanie stosu (ASDK) Azure, w środowiskach nieprodukcyjnych. 

ASDK jest środowiskiem badań i rozwoju, które można wdrożyć do oceny i Wykaż stosu Azure funkcji i usług. Aby rozpocząć pracę z ASDK, należy przygotować hosta komputera, a następnie uruchom niektóre skrypty (instalacja zajmuje kilka godzin). Po wykonaniu tej można Zaloguj się do portali administratora i użytkownika na rozpoczęcie korzystania z usługi Azure stosu.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Pobierać i wyodrębniać pakiet wdrożeniowy
> * Przygotowanie komputera-hosta development kit 
> * Zainstaluj ASDK na komputerze hosta
> * Przeprowadź konfiguracje po wdrożeniu
> * Zarejestrować w usłudze Azure

## <a name="prerequisites"></a>Wymagania wstępne 
Przed zainstalowaniem ASDK, należy przygotować komputer, który będzie hostem zestaw deweloperski (Programowanie zestawu hosta). Na komputerze deweloperskim zestaw host musi spełniać minimalne wymagania dotyczące sprzętu, oprogramowania i wymagania dotyczące sieci. 

Należy również wybrać za pomocą usługi Azure Active Directory (Azure AD) lub usługi Active Directory Federation Services (AD FS) jako rozwiązanie tożsamości dla danego wdrożenia. 

Upewnij się że hosta zestawu programowanie spełnia minimalne wymagania sprzętowe i wprowadzone przed rozpoczęciem wdrażania, aby proces instalacji uruchamia sprawnie decyzji rozwiązania tożsamości. 

**[Przejrzyj zagadnienia związane z planowaniem wdrożenia ASDK](asdk-deploy-considerations.md)**

> [!TIP]
> Można użyć [wymagania dotyczące wdrażania stosu Azure Sprawdź narzędzie](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) po zainstalowaniu systemu operacyjnego na komputerze hosta programowanie zestawu, aby upewnić się, że sprzęt spełnia wszystkie wymagania.

## <a name="download-and-extract-the-deployment-package"></a>Pobierać i wyodrębniać pakiet wdrożeniowy
Po upewnieniu się, że komputer-host zestawu programowanie spełnia podstawowe wymagania dotyczące instalowania ASDK, następnym krokiem jest pobierać i wyodrębniać ASDK pakietu wdrożeniowego. Pakiet wdrażania zawiera plik Cloudbuilder.vhdx, który jest wirtualnego dysku twardego zawierającego rozruchowego system operacyjny i pliki instalacyjne stosu Azure.

Możesz pobrać pakiet wdrożeniowy hosta zestawu rozwoju lub do innego komputera. Pliki wyodrębnionego wdrożenia podjąć 60 GB wolnego miejsca na dysku, więc za pomocą innego komputera może pomóc zmniejszyć wymagania sprzętowe dla hosta development kit.

**[Pobierać i wyodrębniać Azure stosu Development Kit (ASDK)](asdk-download.md)**

## <a name="prepare-the-development-kit-host-computer"></a>Przygotowanie komputera-hosta development kit
Przed zainstalowaniem ASDK na komputerze-hoście, należy przygotować środowisko, a system jest skonfigurowany do uruchamiania z dysku VHD. Po przygotowaniu komputera-hosta development kit uruchamiany z dysku twardego maszyny wirtualnej CloudBuilder.vhdx tak, aby rozpocząć ASDK wdrożenia.

**[Przygotuj komputer-host ASDK](asdk-prepare-host.md)**

## <a name="install-the-asdk-on-the-host-computer"></a>Zainstaluj ASDK na komputerze hosta
Po przygotowaniu komputera-hosta development kit, można wdrożyć w obrazie CloudBuilder.vhdx ASDK. ASDK można wdrożyć przy użyciu graficznego interfejsu użytkownika (GUI) podane przez pobranie i uruchomienie skryptu programu PowerShell asdk installer.ps1 lub całkowicie z [wiersza polecenia](asdk-deploy-powershell.md). 

> [!NOTE]
> Opcjonalnie, po komputer-host załadowaniu CloudBuilder.vhdx, można skonfigurować [ustawieniami telemetrii stosu Azure](asdk-telemetry.md#set-telemetry-level-in-the-windows-registry) *przed* instalowanie ASDK.


**[Zainstaluj zestaw Azure stosu Development Kit (ASDK)](asdk-install.md)**

## <a name="perform-post-deployment-configurations"></a>Przeprowadź konfiguracje po wdrożeniu
Po zainstalowaniu ASDK, istnieje kilka zalecanych kontroli po instalacji i zmian konfiguracji, które należy utworzyć. Można sprawdzić poprawność instalacji został zainstalowany pomyślnie za pomocą polecenia cmdlet test-AzureStack i zainstalować narzędzia Azure PowerShell stosu i GitHub. 

Po wdrożeń, które używają usługi Azure AD musisz aktywować zarówno stosu Azure administratora i dzierżawcy portali. Aktywacja zgadza się na zapewnieniu stosu Azure portalu i usługi Azure Resource Manager odpowiednie uprawnienia (wyświetlane na stronie zgoda) dla wszystkich użytkowników z katalogu.

Należy również zresetować zasady wygasania haseł, aby upewnić się, że hasło dla hosta development kit nie wygasa przed zakończeniem okresu oceny.

> [!NOTE]
> Opcjonalnie można także skonfigurować [ustawieniami telemetrii stosu Azure](asdk-telemetry.md#enable-or-disable-telemetry-after-deployment) *po* instalowanie ASDK.

**[Zadania wdrażania POST ASDK](asdk-post-deploy.md)**

## <a name="register-with-azure"></a>Zarejestrować w usłudze Azure
Należy zarejestrować stosu Azure przy użyciu platformy Azure, co pozwala [pobieranie elementów witrynę Azure marketplace](asdk-marketplace-item.md) stos Azure.

**[Zarejestruj stosu Azure przy użyciu platformy Azure](asdk-register.md)**

## <a name="next-steps"></a>Kolejne kroki
Gratulacje! Po wykonaniu tych czynności, będziesz mieć Środowisko deweloperskie zestawu zarówno [administratora](https://adminportal.local.azurestack.external) i [użytkownika](https://portal.local.azurestack.external) portali. 

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Pobierać i wyodrębniać pakiet wdrożeniowy
> * Przygotowanie komputera-hosta development kit 
> * Zainstaluj ASDK na komputerze hosta
> * Przeprowadź konfiguracje po wdrożeniu
> * Zarejestrować w usłudze Azure

Przejdź do następnego samouczkiem, aby dowiedzieć się, jak dodać element stosu Azure marketplace.

> [!div class="nextstepaction"]
> [Dodawanie elementu stosu Azure marketplace](asdk-marketplace-item.md)




