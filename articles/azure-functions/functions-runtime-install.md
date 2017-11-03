---
title: "Azure Functions instalacji środowiska uruchomieniowego | Dokumentacja firmy Microsoft"
description: "Jak zainstalować usługę Azure Functions środowiska uruchomieniowego"
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/08/2017
ms.author: anwestg
ms.openlocfilehash: 1e4188313a87d07f396e5f8edc8969dd5da2c436
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="install-the-azure-functions-runtime-preview"></a>Zainstaluj usługę Azure Functions podglądu środowiska wykonawczego

Jeśli chcesz zainstalować środowisko uruchomieniowe Functions Azure w wersji zapoznawczej, należy wykonać następujące kroki:

1. Upewnij się, że komputer przekazuje minimalne wymagania
1. Pobierz [Azure Functions środowiska wykonawczego w wersji zapoznawczej Instalatora](https://aka.ms/azafr). 
1. Zainstaluj środowisko uruchomieniowe Functions Azure w wersji zapoznawczej
1. Zakończ konfigurację podglądu środowiska uruchomieniowego funkcji platformy Azure

## <a name="prerequisites"></a>Wymagania wstępne

Przed zainstalowaniem podglądu środowiska uruchomieniowego funkcji Azure należy dysponować następującymi elementami:

1. Maszynie z systemem Microsoft Windows Server 2016 lub Microsoft Windows 10 twórców aktualizacji (Professional lub Enterprise Edition).
1. Wystąpienie programu SQL Server działających w sieci.  Minimalna wersja wymaganie to SQL Server Express.

## <a name="install-the-azure-functions-runtime-preview"></a>Zainstaluj usługę Azure Functions podglądu środowiska wykonawczego

Środowisko uruchomieniowe Functions Azure Instalatora w wersji zapoznawczej przeprowadzi Cię przez instalację środowisko uruchomieniowe Functions Azure w wersji zapoznawczej zarządzania i roli proces roboczy.  Istnieje możliwość zainstalowania roli zarządzania i proces roboczy na tym samym komputerze.  Jednak podczas dodawania więcej funkcji, należy wdrożyć jedną rolę procesu roboczego na dodatkowych komputerach możliwość skalowania funkcji na wielu pracowników.

## <a name="install-the-management-and-worker-role-on-the-same-machine"></a>Zainstaluj na tym samym komputerze zarządzania i roli procesu roboczego

1. Uruchom Instalatora programu Azure Functions środowiska wykonawczego w wersji zapoznawczej.

    ![Środowisko Azure Functions środowiska wykonawczego w wersji zapoznawczej Instalatora][1]

1. **Kliknij przycisk Dalej** zaliczki poza pierwszego etapu Instalatora
1. Po przeczytaniu warunki **umowy licencyjnej**, **zaznacz pole wyboru** akceptacji warunków i **kliknij przycisk Dalej** można poprawić.
1. Teraz wybierz role, którą chcesz zainstalować na tym komputerze **funkcje zarządzania roli** i/lub **roli procesu roboczego funkcji** i **, kliknij przycisk Dalej**

    ![Azure Functions środowiska wykonawczego w wersji zapoznawczej Instalator — Wybór roli][3]

    > [!NOTE]
    > Można zainstalować **roli procesu roboczego funkcji** na wiele innych komputerach, aby to zrobić, wykonaj te instrukcje, a następnie wybrać tylko **roli procesu roboczego funkcji** w Instalatorze.

1. **Kliknij przycisk Dalej** mają **Azure funkcji środowiska uruchomieniowego Instalator** zainstalować na komputerze.
1. Po wykonaniu tych czynności spowoduje uruchomienie Instalatora **narzędzia do konfiguracji środowiska uruchomieniowego funkcji Azure**.

    ![Azure Functions środowiska wykonawczego w wersji zapoznawczej Instalatora pełną][5]

    > [!NOTE]
    > Jeśli instalujesz na **systemu Windows 10** i **kontenera** funkcji nie został wcześniej włączony, **środowisko uruchomieniowe Functions Azure** Instalator wyświetli monit o ponowny rozruch Twojej maszyny, aby zakończyć instalację.

## <a name="configure-the-azure-functions-runtime"></a>Skonfiguruj usługę Azure Functions środowiska uruchomieniowego

Aby ukończyć instalację środowisko uruchomieniowe Functions Azure należy wykonać konfigurację.

1. **Narzędzie konfiguracji środowiska wykonawczego funkcji Azure** pokazuje role są zainstalowane na tym komputerze.

    ![Środowisko Azure Functions narzędzia do konfiguracji podglądu środowiska wykonawczego][6]

1. Kliknij przycisk **bazy danych** wprowadź **szczegóły połączenia dla wystąpienia programu SQL Server** i **kliknij przycisk Zastosuj**.  Jest to wymagane w celu obsługi funkcji Azure można utworzyć bazy danych do obsługi środowiska uruchomieniowego.
    
    ![Środowisko Azure Functions środowiska uruchomieniowego Podgląd bazy danych konfiguracji][7]

1. Kliknij przycisk **poświadczenia** kartę.  Na tym ekranie należy utworzyć dwa nowe poświadczenia do użycia z udziałem plików do obsługi wszystkich funkcji platformy Azure.  **Określ nazwę użytkownika i hasło** kombinacji **właściciela udziału pliku** i **użytkownika udziału pliku** i kliknij przycisk **Zastosuj**.

    ![Środowisko Azure Functions środowiska wykonawczego w wersji zapoznawczej poświadczenia][8]

1. Kliknij przycisk **udziału plików** kartę.  Na tym ekranie Podaj szczegóły **lokalizację udziału pliku**.  Można go utworzyć dla Ciebie lub można użyć istniejącego udziału plików i kliknij przycisk **Zastosuj**.  Wybierz nową lokalizację udziału plików, należy określić katalog do użycia przez środowisko uruchomieniowe Functions Azure.
    
    ![Udział pliku podglądu środowiska uruchomieniowego usługę Azure Functions][9]

1. Kliknij przycisk **IIS** kartę.  Ta karta przedstawia szczegóły witryn sieci Web w usługach IIS, który spowoduje utworzenie instalacji czasu wykonywania funkcji Azure.  **Kliknij przycisk Zastosuj** do wykonania.

    ![Środowisko Azure Functions środowiska wykonawczego w wersji zapoznawczej usług IIS][10]

1. Kliknij przycisk **usług** kartę.  Ta karta przedstawia stan usług w środowisku uruchomieniowym funkcji Azure instalacji.  Jeśli po wykonaniu konfiguracji początkowej **usługi aktywacji hosta funkcji Azure** nie działa kliknij **Uruchom usługę**

    ![Środowisko Azure Functions środowiska wykonawczego w wersji zapoznawczej Configruation pełną][11]

1. Na koniec przejdź do **portalu środowisko uruchomieniowe Functions Azure** jako`https://<machinename>/`

    ![Portal w wersji zapoznawczej usługi Azure Functions środowiska wykonawczego][12]


<!--Image references-->
[1]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer1.png
[2]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer2-EULA.png
[3]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer3-ChooseRoles.png
[4]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer4-Install.png
[5]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer5-InstallComplete.png
[6]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration1.png
[7]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration2_SQL.png
[8]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration3_Credentials.png
[9]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration4_Fileshare.png
[10]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration5_IIS.png
[11]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration6_Services.png
[12]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal.png