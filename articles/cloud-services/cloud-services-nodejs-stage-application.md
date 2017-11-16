---
title: "Etap wdrażania usługi chmury (Node.js) | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wdrożyć aplikację Azure do środowiska pomostowego, a następnie wdrożyć w środowisku produkcyjnym przy użyciu wirtualnego adresu IP (VIP) wymiany."
services: cloud-services
documentationcenter: nodejs
author: craigshoemaker
manager: routlaw
editor: 
ms.assetid: d65d26a6-b424-49cd-a88c-7ef46bb112a8
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: cshoe
ms.openlocfilehash: 307741d0792b34332d98bfa4f2d62c9fd1cf07a1
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="staging-an-application-in-azure"></a>Przemieszczania aplikacji na platformie Azure
Spakowanych aplikacji można wdrożyć do środowiska pomostowego na platformie Azure do sprawdzenia przed przeniesieniem do środowiska produkcyjnego, w którym aplikacja jest dostępna w Internecie. Środowisko tymczasowe jest dokładnie takie same jak środowiska produkcyjnego, z wyjątkiem tego, że tylko dostęp do aplikacji przemieszczanego zaciemnionego adresu URL, który jest generowany przez usługę Azure. Po upewnieniu się, że aplikacja działa prawidłowo, można wdrożyć do środowiska produkcyjnego, wykonując wymiany wirtualnego adresu IP (VIP).

> [!NOTE]
> Kroki opisane w tym artykule dotyczą tylko aplikacji węzła hostowany jako usługa w chmurze Azure.
> 
> 

## <a name="step-1-stage-an-application"></a>Krok 1: Przemieszczanie aplikacji
To zadanie obejmuje sposób umieszczania aplikacji przy użyciu **Microsoft Azure PowerShell**.

1. Gdy usługa publikowania, po prostu Przekaż **-gniazdo** parametr **Publish-AzureServiceProject** polecenia cmdlet.
   
   ```powershell
   Publish-AzureServiceProject -Slot staging
   ```
2. Zaloguj się do [klasycznego portalu Azure] i wybierz **usługi w chmurze**. Po chmurze usługi jest tworzony i **przemieszczania** stan kolumna została zaktualizowana w celu **systemem**, kliknij nazwę usługi.
   
   ![Wyświetlanie działającej usłudze portalu][cloud-service]
3. Wybierz **pulpitu nawigacyjnego**, a następnie wybierz **przemieszczania**.
   
   ![pulpit nawigacyjny usługi w chmurze][cloud-service-dashboard]
4. Zanotuj wartość w **adres URL witryny** wpisu z prawej strony. Nazwa DNS jest zaciemnionego wewnętrzny identyfikator, który wygenerował Azure.
   
    ![adres url witryny][cloud-service-staging-url]

Teraz można sprawdzić, czy aplikacja działa poprawnie w środowisku przemieszczania za pomocą przemieszczania adres URL witryny.

## <a name="step-2-upgrade-an-application-in-production-by-swapping-vips"></a>Krok 2: Uaktualnianie aplikacji w środowisku produkcyjnym przez Trwa zamienianie adresów VIP
Po zweryfikowaniu uaktualnionej wersji aplikacji w środowisku przemieszczania, można szybko udostępnić go w środowisku produkcyjnym przez wymiany wirtualnych adresów IP (VIP) środowisk przemieszczania i produkcji.

> [!NOTE]
> Tego kroku przyjęto założenie, że masz już wdrożonych aplikacji do środowiska produkcyjnego i przemieszczane uaktualnionej wersji aplikacji.
> 
> 

1. Zaloguj się do [klasycznego portalu Azure], kliknij przycisk **usługi w chmurze** , a następnie wybierz nazwę usługi.
2. Z **pulpitu nawigacyjnego**, wybierz pozycję **przemieszczania**, a następnie kliknij przycisk **wymiany** w dolnej części strony. Spowoduje to otwarcie okna dialogowego wymiany wirtualnych adresów IP.
   
   ![okno dialogowe wymiany adresów VIP][vip-swap-dialog]
3. Przejrzyj informacje, a następnie kliknij przycisk **OK**. Dwa wdrożenia Rozpocznij aktualizowanie zmienia przemieszczania wdrożenia do środowiska produkcyjnego i wdrożenia produkcyjnego zmienia się na przemieszczania.

Pomyślnie przemieszczane wdrożenia i uaktualnienia wdrożenia produkcyjnego przez zamianę wirtualne adresy IP z wdrożeniem tymczasowych.

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Jak wdrożyć uaktualnienie usługi do środowiska produkcyjnego przez zamianę wirtualne adresy IP na platformie Azure]

[klasycznego portalu Azure]: http://manage.windowsazure.com
[cloud-service]: ./media/cloud-services-nodejs-stage-application/staging-cloud-service-running.png
[cloud-service-dashboard]: ./media/cloud-services-nodejs-stage-application/cloud-service-dashboard-staging.png
[cloud-service-staging-url]: ./media/cloud-services-nodejs-stage-application/cloud-service-staging-url.png
[vip-swap-dialog]: ./media/cloud-services-nodejs-stage-application/vip-swap-dialog.png
[Jak wdrożyć uaktualnienie usługi do środowiska produkcyjnego przez zamianę wirtualne adresy IP na platformie Azure]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
