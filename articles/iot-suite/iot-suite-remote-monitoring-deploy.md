---
title: "Wdrażanie zdalne rozwiązanie monitorowania - Azure | Dokumentacja firmy Microsoft"
description: "W tym samouczku przedstawiono sposób udostępnienia zdalnego monitorowania wstępnie skonfigurowane rozwiązanie z azureiotsuite.com."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 08/09/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: babcf20b58af1415e0e658e0a622cb056e34642b
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="deploy-the-remote-monitoring-preconfigured-solution"></a>Wdrażanie zdalne wstępnie skonfigurowane rozwiązanie monitorowania

W tym samouczku przedstawiono sposób aprowizowania wstępnie skonfigurowanego rozwiązania do monitorowania zdalnego. Możesz wdrożyć rozwiązanie z azureiotsuite.com. Można także wdrożyć rozwiązanie przy użyciu interfejsu wiersza polecenia, aby dowiedzieć się więcej o tej opcji, zobacz [wdrożyć wstępnie skonfigurowane rozwiązanie z wiersza polecenia](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#deploy-a-pcs-from-the-command-line).

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Konfigurowanie wstępnie skonfigurowanego rozwiązania
> * Wdrażanie wstępnie skonfigurowane rozwiązanie
> * Zaloguj się do wstępnie skonfigurowane rozwiązanie

## <a name="prerequisites"></a>Wymagania wstępne

Do wykonania kroków tego samouczka jest potrzebna aktywna subskrypcja platformy Azure.

Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure](http://azure.microsoft.com/pricing/free-trial/).

## <a name="deploy-the-preconfigured-solution"></a>Wdrażanie wstępnie skonfigurowane rozwiązanie

Przed wdrożeniem wstępnie skonfigurowane rozwiązanie do subskrypcji platformy Azure, musisz wybrać niektóre opcje konfiguracji:

1. Zaloguj się do [azureiotsuite.com](https://www.azureiotsuite.com) poświadczenia konta przy użyciu platformy Azure i kliknij przycisk  **+**  tworzenie rozwiązań.

1. Kliknij pozycję **Wybierz** na kafelku **Zdalne monitorowanie**.

1. Na **utworzyć monitorowania zdalnego rozwiązania** wprowadź **Nazwa rozwiązania** dla zdalnego monitorowania wstępnie skonfigurowane rozwiązanie.

1. Wybierz **podstawowe** lub **Enterprise** wdrożenia. Jeśli są deplying Dowiedz się, jak to działa rozwiązania lub wybierz pozycję Uruchom pokaz **podstawowe** opcję, aby zminimalizować koszty.

1. Wybierz **Java** lub **.NET** jako język. Wszystkie mikrousług są dostępne jako implementacje Java lub .NET.

1. Przegląd **szczegóły rozwiązania** panelu, aby uzyskać więcej informacji o dostępnych opcjach konfiguracji.

1. W polach **Subskrypcja** i **Region** wybierz wartości, których chcesz użyć do aprowizacji rozwiązania.

1. Kliknij pozycję **Utwórz rozwiązanie**, aby rozpocząć proces aprowizowania. Zwykle trwa on kilka minut.

Aby uzyskać informacje dotyczące rozwiązywania problemów, zobacz [co robić, jeśli wdrożenie nie powiedzie się](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide#what-to-do-when-a-deployment-fails) w repozytorium GitHub.

## <a name="sign-in-to-the-preconfigured-solution"></a>Zaloguj się do wstępnie skonfigurowane rozwiązanie

Po zakończeniu procesu inicjowania obsługi administracyjnej można logowania się do zdalnego wstępnie skonfigurowane rozwiązanie monitorowania.

1. Na **elastycznie rozwiązań** wybierz nowe zdalnego rozwiązanie monitorowania.

1. Można wyświetlić informacje dotyczące rozwiązania monitorowania zdalnego w panelu, który pojawi się. Wybierz **pulpit nawigacyjny rozwiązania** do nawiązania połączenia zdalnego rozwiązanie monitorowania.

    > [!NOTE]
    > Rozwiązanie monitorowania zdalnego można usunąć z tego panelu po zakończeniu pracy z nim.

1. Zdalny pulpit nawigacyjny monitorowania rozwiązania wyświetla w przeglądarce.

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Konfigurowanie wstępnie skonfigurowanego rozwiązania
> * Wdrażanie wstępnie skonfigurowane rozwiązanie
> * Zaloguj się do wstępnie skonfigurowane rozwiązanie

Teraz, gdy wdrożono rozwiązanie monitorowania zdalnego następnym krokiem jest [Poznaj możliwości pulpit nawigacyjny rozwiązania](./iot-suite-remote-monitoring-explore.md).

<!-- Next tutorials in the sequence -->