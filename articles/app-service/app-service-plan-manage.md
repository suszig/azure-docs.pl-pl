---
title: "Zarządzanie plan usługi aplikacji na platformie Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak do usługi App Service planów wykonywania różnych zadań zarządzania plan usługi aplikacji."
keywords: "usługi aplikacji, usługi azure app service, skalowania, plan usługi app service, zmienić, tworzenia, zarządzania, zarządzanie"
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 4859d0d5-3e3c-40cc-96eb-f318b2c51a3d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: cephalin
ms.openlocfilehash: 5369946b1e8a4851ee940cf6fe91a1bdb94db5f3
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/17/2017
---
# <a name="manage-an-app-service-plan-in-azure"></a>Zarządzanie plan usługi aplikacji na platformie Azure

[Planu usługi aplikacji](azure-web-sites-web-hosting-plans-in-depth-overview.md) udostępnia zasoby aplikacji usługi app Service musi być uruchamiane. Ten przewodnik przedstawia sposób zarządzać plan usługi aplikacji. 

## <a name="create-an-app-service-plan"></a>Tworzenie planu usługi App Service

> [!TIP]
> Jeśli masz środowisko usługi aplikacji, zobacz [Utwórz plan usługi aplikacji w środowisku usługi aplikacji](environment/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan).

Można utworzyć pusty plan usługi aplikacji lub w ramach tworzenia aplikacji.

W [portalu Azure](https://portal.azure.com), kliknij przycisk **nowy** > **sieci Web i mobilność**, a następnie wybierz **aplikacji sieci Web** lub innego typu aplikacji usługi aplikacji.

![Utwórz aplikację w portalu Azure.][createWebApp]

Można następnie wybierz istniejący plan usługi aplikacji lub utworzyć plan dla nowej aplikacji.

 ![Tworzenie planu usługi aplikacji.][createASP]

Aby utworzyć plan usługi aplikacji, kliknij przycisk **[+] Tworzenie nowych**, typ **planu usługi aplikacji** nazwy, a następnie wybierz odpowiedni **lokalizacji**. Kliknij przycisk **warstwa cenowa**, a następnie wybierz odpowiednią warstwę cenową dla usługi. Wybierz **Wyświetl wszystkie** do widoku więcej cennik opcje, takie jak **wolne** i **Shared**. 

Po wybraniu warstwy cenowej kliknij **wybierz** przycisku.

<a name="move"></a>

## <a name="move-an-app-to-another-app-service-plan"></a>Przenieś aplikację do innego planu usługi aplikacji

Aplikację można przenieść do innego planu usługi aplikacji — tak długo, jak planu źródłowego i docelowego planu znajdują się w _tej samej grupie zasobów i region geograficzny_.

Aby przenieść aplikację do innego planu, przejdź do aplikacji, którą chcesz przenieść [portalu Azure](https://portal.azure.com).

W **Menu**, wyszukaj **planu usługi App Service** sekcji.

Wybierz **planu usługi aplikacji zmiany** do rozpoczęcia procesu.

**Zmień plan usługi aplikacji** otwiera **planu usługi aplikacji** selektora. Wybierz istniejący plan można przenieść tej aplikacji do. 

> [!IMPORTANT]
> **Planu usługi aplikacji wybierz** strony są filtrowane według następujących kryteriów: 
> - Istnieje w tej samej grupie zasobów 
> - Istnieje w tym samym regionie geograficznym 
> - Istnieje w tej samej przestrzeni sieci Web  
> 
> A _przestrzeni sieci Web_ jest konstrukcją logiczną w usłudze App Service, który definiuje grupę zasobów serwera. Region geograficzny (na przykład zachodnie stany USA) zawiera wiele webspaces Aby przydzielić klientów korzystających z usługi aplikacji. Obecnie zasobów usługi aplikacji — nie można przenieść między webspaces. 
> 

![Selektor planu usługi aplikacji.][change]

Każdy plan ma własną warstwy cenowej. Na przykład przenoszenie lokacji z **wolne** warstwy do **standardowe** warstwy, włącza wszystkie aplikacje przypisane do korzystania z funkcji i zasobów **standardowe** warstwy. Jednak przenoszenie aplikacji z wyższej planu warstwowych do dolnej planu warstwowych oznacza już mieć dostęp do niektórych funkcji. Jeśli aplikacja korzysta z funkcji, który nie jest dostępny w planie docelowej, występuje błąd, pokazujący, której funkcji jest używana, który nie jest dostępny. Na przykład, jeśli jednej z aplikacji korzysta z certyfikatów SSL, można napotkać komunikat o błędzie: `Cannot update the site with hostname '<app_name>' because its current SSL configuration 'SNI based SSL enabled' is not allowed in the target compute mode. Allowed SSL configuration is 'Disabled'.`w takim przypadku należy skalować warstwę cenową planu docelowego **podstawowe** lub nowszej, lub konieczne jest usunięcie wszystkich połączeń SSL aplikację, zanim będzie możliwe przeniesienie aplikacji do docelowego planu.

## <a name="move-an-app-to-a-different-region"></a>Przenieś aplikację w innym regionie

Region, w której aplikacja będzie działać to region planu usługi aplikacji, który znajduje się w. Jednak nie można zmienić regionu plan usługi aplikacji. Jeśli chcesz uruchomić aplikację w innym regionie, co alternatywa to aplikacji klonowania. Klonowanie tworzy kopię aplikacji w nowy lub istniejący plan usługi aplikacji w dowolnym regionie.

Można znaleźć **aplikacji w klonowania** w **narzędzi programistycznych** części menu.

> [!IMPORTANT]
> Klonowanie ma pewne ograniczenia, które możesz przeczytać temat na [klonowania aplikacji usługi aplikacji Azure](app-service-web-app-cloning.md).

## <a name="scale-an-app-service-plan"></a>Skalowanie planu usługi aplikacji

Aby skalować usługę aplikacji planu do warstwy cenowej, zobacz [skalowanie w górę aplikacji na platformie Azure](web-sites-scale.md).

Aby skalować w poziomie liczbę wystąpień aplikacji, zobacz [skalowanie liczby wystąpień ręcznie lub automatycznie](../monitoring-and-diagnostics/insights-how-to-scale.md).

<a name="delete"></a>

## <a name="delete-an-app-service-plan"></a>Usuwanie planu usługi aplikacji

Aby uniknąć nieoczekiwanego opłat, po usunięciu ostatniej aplikacji w planie usługi aplikacji, usług aplikacji spowoduje również usunięcie planu domyślnie. Jeśli wybrana opcja zachowania zamiast tego planu, należy zmienić planu **wolne** warstwę, aby nie zostały naliczone opłaty.

> [!IMPORTANT]
> **Planów usługi App Service** , która ma żadnych aplikacji powiązanych z nimi nadal spowodować naliczenie opłat, ponieważ nadal zarezerwować skonfigurowanych wystąpień maszyny Wirtualnej.

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Skalowanie w górę aplikacji na platformie Azure](web-sites-scale.md)

[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
