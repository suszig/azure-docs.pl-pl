---
title: "Udostępnić w sieci web i aplikacji API apps użytkownikom stosu Azure | Dokumentacja firmy Microsoft"
description: "Samouczek, aby zainstalować dostawcę zasobów usługi aplikacji i utworzyć oferuje który zapewnić użytkownikom stosu Azure możliwość tworzenia sieci web i aplikacji API apps."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 7/03/2017
ms.author: brenduns
ms.reviewer: 
ms.custom: mvc
ms.openlocfilehash: 5978706f2cab69c83a49bfd0e15ae904a38c7bab
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/22/2018
---
# <a name="make-web-and-api-apps-available-to-your-azure-stack-users"></a>Udostępnić w sieci web i aplikacji API apps użytkownikom Azure stosu

Jako administrator chmury Azure stosu tworzenia oferty, które pozwalają użytkownikom (dzierżawcami), tworzenie aplikacji usługi Azure Functions i sieci web i interfejsu API. Zapewniając użytkownikom dostęp do tych aplikacji na żądanie, oparte na chmurze, można je zapisać czasu i zasobów. Aby to skonfigurować, obejmują:

> [!div class="checklist"]
> * Wdrażanie dostawcy zasobów usługi aplikacji
> * Tworzenie oferty
> * Testowanie oferty

## <a name="deploy-the-app-service-resource-provider"></a>Wdrażanie dostawcy zasobów usługi aplikacji

1. [Przygotowywanie hostów Azure stosu Development Kit](azure-stack-app-service-before-you-get-started.md). W tym wdrażanie dostawcy zasobów programu SQL Server, który jest wymagany do tworzenia niektórych aplikacji.
2. [Pobierz skrypty Instalatora i Pomocnika](azure-stack-app-service-deploy.md).
3. [Uruchom skrypt pomocnika do tworzenia wymaganych certyfikatów](azure-stack-app-service-deploy.md).
4. [Zainstaluj dostawcę zasobów usługi aplikacji](azure-stack-app-service-deploy.md) (potrwa kilka godzin, aby zainstalować i dla wszystkich ról procesu roboczego się pojawiać).
5. [Sprawdź poprawność instalacji](azure-stack-app-service-deploy.md#validate-the-app-service-on-azure-stack-installation).

## <a name="create-an-offer"></a>Tworzenie oferty

Na przykład można utworzyć ofertę, która umożliwia użytkownikom tworzenie systemów zarządzania zawartością sieci web DNN. Wymaga usługi SQL Server, który jest już włączony, instalując dostawcy zasobów programu SQL Server.

1.  [Ustaw limit przydziału](azure-stack-setting-quotas.md) i nadaj mu nazwę *AppServiceQuota*. Wybierz **Microsoft.Web** dla **Namespace** pola.
2.  [Tworzenie planu](azure-stack-create-plan.md). Nadaj mu nazwę *TestAppServicePlan*, wybierz pozycję **Microsoft.SQL** usługi, i **przydziału AppService** przydziału.

    > [!NOTE]
    > Aby zezwolić użytkownikom na tworzenie inne aplikacje, innych usług mogą być wymagane w planie. Wymaga na przykład usługi Azure Functions, że plan obejmuje **Microsoft.Storage** usługi, gdy wymaga Wordpress **Microsoft.MySQL**.
    > 
    >

3.  [Utwórz ofertę](azure-stack-create-offer.md), nadaj jej nazwę **TestAppServiceOffer** i wybierz **TestAppServicePlan** planu.

## <a name="test-the-offer"></a>Testowanie oferty

Po wdrożeniu dostawcy zasobów usługi aplikacji i utworzyć ofertę, może zalogować się jako użytkownik, subskrybować oferty i tworzenie aplikacji. Na przykład utworzymy system zarządzania zawartością DNN platformy. Należy najpierw utworzyć bazę danych SQL, a następnie DNN aplikacji sieci web.

### <a name="subscribe-to-the-offer"></a>Subskrybuj oferty
1. Zaloguj się do portalu Azure stosu (https://portal.local.azurestack.external) jako dzierżawcy.
2. Kliknij przycisk **uzyskania subskrypcji** > typ **TestAppServiceSubscription** w obszarze **Nazwa wyświetlana** > **wybierz ofertę**  >  **TestAppServiceOffer** > **utworzyć**.

### <a name="create-a-sql-database"></a>Tworzenie bazy danych SQL

1. Kliknij przycisk  **+**   >  **dane i magazyn** > **bazy danych SQL**.
2. Pozostaw wartości domyślne dla pól, z wyjątkiem w następujący sposób:
    - **Nazwa bazy danych**: DNNdb
    - **Maksymalny rozmiar w MB**: 100
    - **Subskrypcja**: TestAppServiceOffer
    - **Grupa zasobów**: zarządcy zasobów DNN
3. Kliknij przycisk **ustawienia logowania**, wprowadź poświadczenia dla bazy danych, a następnie kliknij przycisk **OK**. Te poświadczenia będą używane w dalszej części następujące kroki.
4. Kliknij przycisk **SKU** > Wybierz jednostki SKU SQL utworzony dla programu SQL Server obsługującego > **OK**.
5. Kliknij przycisk **Utwórz**.

### <a name="create-a-dnn-app"></a>Tworzenie aplikacji DNN    

1. Kliknij przycisk  **+**   >  **zobaczyć wszystkie** > **DNN platformy w wersji zapoznawczej** > **Utwórz**.
2. Typ *DNNapp* w obszarze **Nazwa aplikacji** i wybierz **TestAppServiceOffer** w obszarze **subskrypcji**.
3. Kliknij przycisk **Skonfiguruj wymagane ustawienia** > **Utwórz nowy** > typ **planu usługi aplikacji** nazwy.
4. Kliknij przycisk **warstwa cenowa** > **F1 bezpłatna** > **wybierz** > **OK**.
5. Kliknij przycisk **bazy danych** i wprowadź informacje dotyczące bazy danych SQL utworzony wcześniej.
6. Kliknij przycisk **Utwórz**.

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wdrażanie dostawcy zasobów usługi aplikacji
> * Tworzenie oferty
> * Testowanie oferty

ADVANCE do samouczka dalej, aby dowiedzieć się, jak:

> [!div class="nextstepaction"]
> [Wdrażanie aplikacji na platformie Azure oraz Azure stosu](user/azure-stack-solution-pipeline.md)
