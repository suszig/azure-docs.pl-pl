---
title: "Używanie automatyzacji Azure do wyzwalania zadania za | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać usługi Automatyzacja Azure służącą do wyzwalania zadania Menedżer StorSimple danych (w podglądzie prywatnym)"
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/16/2017
ms.author: vidarmsft
ms.openlocfilehash: 9691408bcd80afb6eba534f26749b76dd3bfe315
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-automation-to-trigger-a-job-private-preview"></a>Używanie automatyzacji Azure do wyzwalania zadania (podglądzie prywatnym)

W tym artykule opisano, jak uruchomić zadanie Menedżer StorSimple danych przy użyciu usługi Automatyzacja Azure.

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem upewnij się, że masz:

*   Zainstalowany program Azure Powershell. [Pobieranie programu Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*   Ustawienia konfiguracji w celu zainicjowania zadania przekształcania danych (instrukcjami, aby uzyskać te ustawienia są uwzględnione w tym miejscu).
*   Definicji zadania, który został poprawnie skonfigurowany w zasobie danych hybrydowych w grupie zasobów.
*   Pobierz `DataTransformationApp.zip` [zip](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip) plik z repozytorium github.
*   Pobierz `Get-ConfigurationParams.ps1` [skryptu](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Get-ConfigurationParams.ps1) z repozytorium github.
*   Pobierz `Trigger-DataTransformation-Job.ps1` [skryptu](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1) z repozytorium github.

## <a name="step-by-step"></a>Krok po kroku

### <a name="get-azure-active-directory-permissions-for-the-automation-job-to-run-the-job-definition"></a>Uprawnienia usługi Azure Active Directory dla definicji zadania uruchomienie zadania automatyzacji

1. Aby pobrać parametry konfiguracji usługi Active Directory, wykonaj następujące czynności:

    1. Otwórz program Windows PowerShell w komputerze lokalnym. Upewnij się, że [programu Azure PowerShell](https://azure.microsoft.com/downloads/) jest zainstalowany.
    1. Uruchom `Get-ConfigurationParams.ps1` skryptu (w folderze pobrane powyżej). W oknie programu PowerShell, wpisz następujące polecenie:

        ```
        .\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```

        ActiveDirectoryKey to hasło, które można użyć później. Wprowadź hasło wybranych przez użytkownika. Nazwa aplikacji może być dowolnym ciągiem.

2. Ten skrypt generuje następujące wartości, które mają być używane podczas wyzwalania elementu runbook automatyzacji. Zanotuj te wartości.

    - Identyfikator klienta
    - Identyfikator dzierżawy
    - Kluczy Active Directory (tak samo, jak to objęte powyżej)
    - Identyfikator subskrypcji

### <a name="set-up-the-automation-account"></a>Skonfiguruj konto automatyzacji

1. Zaloguj się do platformy Azure i otwórz konto automatyzacji.
2. Kliknij przycisk **zasoby** Kafelek, aby otworzyć listę zasobów.
3. Kliknij przycisk **modułów** Kafelek, aby otworzyć listę modułów.
4. Kliknij przycisk **+ Dodaj moduł** przycisk i Dodaj blok modułu jest uruchamiana.

    ![Ustawienia konta automatyzacji](./media/storsimple-data-manager-job-using-automation/add-module1m.png)

5. Po wybraniu `DataTransformationApp.zip` plików z komputera lokalnego, kliknij **OK** zaimportować modułu.

   Automatyzacja Azure importuje moduł do swojego konta, umożliwia wyodrębnianie metadanych dotyczących modułu. Ta operacja może potrwać kilka minut.

   ![Ustawienia konta automatyzacji](./media/storsimple-data-manager-job-using-automation/add-module2m.png)

   

6. Pojawi się powiadomienie jest wdrażany modułu i kolejne powiadomienie po zakończeniu procesu.  Można również sprawdzić stan **modułów** kafelka.

### <a name="to-import-the-runbook-that-triggers-the-job-definition"></a>Aby zaimportować element runbook, który wyzwala definicji zadania

1. W witrynie Azure Portal otwórz konto usługi Automation.
2. Kliknij przycisk **Runbook** Kafelek, aby otworzyć listę elementów runbook.
3. Kliknij przycisk **+ Dodaj element runbook** , a następnie **importowanie istniejącego elementu runbook**.

   ![Importuj istniejący element runbook](./media/storsimple-data-manager-job-using-automation/import-a-runbook.png)

4. Kliknij przycisk **pliku Runbook** i wybierz plik do zaimportowania `Trigger-DataTransformation-Job.ps1`.
5. Kliknij przycisk **Utwórz** można zaimportować elementu runbook. Nowy element runbook zostanie wyświetlony na liście elementów runbook dla konta automatyzacji.
7. Kliknij przycisk **wyzwalacza DataTransformation zadania** runbook, a następnie kliknij przycisk **Edytuj**.
8. Kliknij przycisk **publikowania** , a następnie **tak** po wyświetleniu monitu o potwierdzenie.


### <a name="to-run-the-runbook"></a>Aby uruchomić element runbook:
1. W witrynie Azure Portal otwórz konto usługi Automation.
2. Kliknij kafelek **Elementy runbook**, aby otworzyć listę elementów runbook.
3. Kliknij przycisk **wyzwalacza DataTransformation zadania**.
4. Kliknij pozycję **Uruchom**, aby uruchomić element Runbook.

   ![Uruchamianie elementu Runbook](./media/storsimple-data-manager-job-using-automation/run-runbook1m.png)

5. W **uruchomić elementu runbook** bloku, wprowadź wszystkie parametry. Kliknij przycisk **OK** można przesłać zadania przekształcania danych.

   ![Uruchamianie elementu Runbook](./media/storsimple-data-manager-job-using-automation/run-runbook2m.png)


## <a name="next-steps"></a>Następne kroki

[Użyj Menedżera danych StorSimple interfejsu użytkownika Służącego do przekształcenia danych](storsimple-data-manager-ui.md).