---
title: "Zarządzanie profilami wersji interfejsu API w stosie Azure | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat profilów wersji interfejsu API Azure stosu"
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: sngun
ms.openlocfilehash: f986abebaf9117b040c149c10f6b2358bbdc0f44
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2017
---
# <a name="manage-api-version-profiles-in-azure-stack"></a>Zarządzanie profilami wersji interfejsu API Azure stosu

Funkcja interfejsu API profilów wersji w usłudze Azure App Service zapewnia sposób zarządzania różnice wersji platformy Azure i stosu Azure. Profil wersji interfejsu API to zbiór modułów programu AzureRM PowerShell z określonych wersji interfejsu API. Każdej platformy w chmurze ma zestaw obsługiwanych profile w wersji interfejsu API. Na przykład stosu Azure obsługuje wersję profilu określone, datę, takich jak **2017-03-09-profilu**, i obsługuje Azure *najnowsze* profilu wersji interfejsu API. Po zainstalowaniu profilu są zainstalowane moduły programu AzureRM PowerShell, które odpowiadają określonego profilu.

## <a name="install-the-powershell-module-required-to-use-api-version-profiles"></a>Zainstaluj moduł programu PowerShell wymaganych do używania profilów wersji interfejsu API

**AzureRM.Bootstrapper** moduł, który jest dostępny za pośrednictwem galerii programu PowerShell zawiera polecenia cmdlet programu PowerShell, które są wymagane do pracy z profilami wersji interfejsu API. Użyj następującego polecenia cmdlet do zainstalowania **AzureRM.Bootstrapper** modułu:

```PowerShell
Install-Module -Name AzureRm.BootStrapper
```
**AzureRM.Bootstrapper** modułu jest podgląd, aby uzyskać więcej informacji i funkcji mogą ulec zmianie. Aby pobrać i zainstalować najnowszą wersję tego modułu z galerii programu PowerShell, uruchom następujące polecenie cmdlet:

```PowerShell
Update-Module -Name "AzureRm.BootStrapper"
```

## <a name="install-a-profile"></a>Instalowanie profilu

Użyj **AzureRmProfile instalacji** polecenia cmdlet z **2017-03-09-profilu** profilu wersji interfejsu API, aby zainstalować moduły AzureRM wymagane przez stos Azure. 

>[!NOTE]
>Moduły administrator chmury Azure stosu nie są zainstalowane z tym profilem wersji interfejsu API. Moduły administrator powinien być zainstalowany oddzielnie jak określono w kroku 3 [Zainstaluj program PowerShell Azure stosu](azure-stack-powershell-install.md) artykułu.

```PowerShell 
Install-AzureRMProfile -Profile 2017-03-09-profile
```
## <a name="install-and-import-modules-in-a-profile"></a>Zainstaluj i zaimportuj moduły w profilu

Użyj **AzureRmProfile użyj** polecenia cmdlet, aby zainstalować i zaimportować moduły, które są skojarzone z profilem wersji interfejsu API. W sesji programu PowerShell można importować tylko jeden profil wersji interfejsu API. Aby zaimportować innego profilu wersji interfejsu API, musi Otwórz nową sesję programu PowerShell. **AzureRMProfile użyj** uruchomieniu polecenia cmdlet następujące zadania:  
1. Sprawdza, czy moduły programu PowerShell skojarzonych z profilem określona wersja interfejsu API są zainstalowane w bieżącym zakresie.  
2. Pobiera i instaluje modułów, jeśli nie są już zainstalowane.   
3. Importuje moduł do bieżącej sesji programu PowerShell. 

```PowerShell
# Installs and imports the specified API version profile into the current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser

# Installs and imports the specified API version profile into the current PowerShell session without any prompts.
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser -Force
```

Aby zainstalować i importowanie wybranych modułów AzureRM z profilu wersji interfejsu API, uruchom **AzureRMProfile użyj** polecenia cmdlet z *modułu* parametru:

```PowerShell
# Installs and imports the Compute, Storage, and Network modules from the specified API version profile into your current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Module AzureRM.Compute, AzureRM.Storage, AzureRM.Network
```

## <a name="get-the-installed-profiles"></a>Uzyskać zainstalowanych profile

Użyj **Get-AzureRmProfile** polecenia cmdlet, aby uzyskać listę dostępnych profilów wersji interfejsu API: 

```PowerShell
# Lists all API version profiles provided by the AzureRM.BootStrapper module.
Get-AzureRmProfile -ListAvailable 

# Lists the API version profiles that are installed on your machine.
Get-AzureRmProfile
```
## <a name="update-profiles"></a>Zaktualizować profile

Użyj **AzureRmProfile aktualizacji** polecenia cmdlet, aby zaktualizować tych modułów w profilu wersji interfejsu API do najnowszej wersji modułów, które są dostępne w galerii programu PowerShell. Firma Microsoft zaleca się uruchomienie **AzureRmProfile aktualizacji** polecenia cmdlet w nowej sesji programu PowerShell w celu uniknięcia konfliktów podczas importowania modułów. **AzureRmProfile aktualizacji** uruchomieniu polecenia cmdlet następujące zadania:

1. Sprawdza, czy najnowsze wersje moduły są zainstalowane w danym profilu wersji interfejsu API dla bieżącego zakresu.  
2. Wyświetlany jest monit o zainstalowanie modułów, jeśli nie są już zainstalowane.  
3. Instaluje i importuje zaktualizowane moduły do bieżącej sesji programu PowerShell.  

```PowerShell
Update-AzureRmProfile -Profile 2017-03-09-profile
```

Aby usunąć zainstalowanych wcześniej wersji modułów, aby aktualizację do najnowszej dostępnej wersji, użyj **AzureRmProfile aktualizacji** polecenia cmdlet wraz z *- RemovePreviousVersions* parametr:

```PowerShell 
Update-AzureRmProfile -Profile 2017-03-09-profile -RemovePreviousVersions
```

To polecenie cmdlet jest uruchamiane następujące zadania:  

1. Sprawdza, czy najnowsze wersje moduły są zainstalowane w danym profilu wersji interfejsu API dla bieżącego zakresu.  
2. Usuwa starsze wersje modułów z bieżącym profilu wersji interfejsu API i w bieżącej sesji programu PowerShell.  
3. Wyświetlany jest monit o zainstalowanie najnowszej wersji modułów.  
4. Instaluje i importuje zaktualizowane moduły do bieżącej sesji programu PowerShell.  
 
## <a name="uninstall-profiles"></a>Odinstaluj profilów

Użyj **AzureRmProfile Odinstaluj** polecenia cmdlet można odinstalować określonego profilu wersji interfejsu API:

```PowerShell 
Uninstall-AzureRmProfile -Profile 2017-03-09-profile
```

## <a name="next-steps"></a>Następne kroki
* [Install PowerShell for Azure Stack](azure-stack-powershell-install.md) (Instalowanie programu PowerShell dla usługi Azure Stack)
* [Konfigurowanie środowiska PowerShell użytkownika Azure stosu](azure-stack-powershell-configure-user.md)  
