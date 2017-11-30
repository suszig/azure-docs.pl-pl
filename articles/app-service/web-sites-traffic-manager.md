---
title: "Kontrolowanie ruchu z usługi Azure Traffic Manager w usłudze Azure App Service"
description: "Ten artykuł zawiera informacje podsumowania dla usługi Azure Traffic Manager w odniesieniu do usługi Azure App Service."
services: app-service\web
documentationcenter: 
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: dabda633-e72f-4dd4-bf1c-6e945da456fd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2016
ms.author: cephalin
ms.openlocfilehash: efb732e9be62313eb199cb2cfbb1fa4d2cde0282
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="controlling-azure-app-service-traffic-with-azure-traffic-manager"></a>Kontrolowanie ruchu z usługi Azure Traffic Manager w usłudze Azure App Service
> [!NOTE]
> Ten artykuł zawiera informacje podsumowania dla Menedżera ruchu Microsoft Azure w odniesieniu do usługi Azure App Service. Więcej informacji na temat usługi Azure Traffic Manager sam można znaleźć, przechodząc na stronę łącza na końcu tego artykułu.
> 
> 

## <a name="introduction"></a>Wprowadzenie
Usługi Azure Traffic Manager można użyć do kontrolowania sposobu żądania od klientów sieci web są dystrybuowane do aplikacji w usłudze Azure App Service. Gdy punkty końcowe usługi aplikacji są dodawane do profilu Menedżera ruchu Azure, Azure Traffic Manager przechowuje informacje o stanu aplikacji Aplikacja usługi (uruchomionej, zatrzymanej lub został usunięty), dzięki czemu można zdecydować, które z tych punktów końcowych powinny odbierać dane.

## <a name="routing-methods"></a>metody routingu
Usługi Azure Traffic Manager korzysta z czterech różnych metod routingu. Te metody są opisane na poniższej liście odnoszą się do usługi Azure App Service.

* **[Priorytet](#priority):** przy użyciu aplikacji głównej dla całego ruchu, a następnie podaj kopii zapasowych w przypadku podstawowej lub tworzenia kopii zapasowej aplikacji są niedostępne.
* **[Ważone](#weighted):** rozpowszechniają ruchu zestawu aplikacji, równomiernie lub według wagi, które należy zdefiniować.
* **[Wydajność](#performance):** Jeśli masz aplikacje w różnych lokalizacjach geograficznych przy użyciu tej aplikacji "najbliższy" pod względem najniższe opóźnienia sieci.
* **[Geograficzne](#geographic):** bezpośredniego użytkowników do określonych aplikacji na podstawie których lokalizacji geograficznej ich zapytanie DNS pochodzi z. 

Aby uzyskać więcej informacji, zobacz [metody routingu ruchu Menedżera](../traffic-manager/traffic-manager-routing-methods.md).

## <a name="app-service-and-traffic-manager-profiles"></a>Usługi aplikacji i profile Menedżera ruchu
Aby skonfigurować kontrolę ruchu aplikacji usługi App Service, Utwórz profil w usłudze Azure Traffic Manager czy używa jednego z trzech obciążenia równoważenia metod opisanych wcześniej, a następnie dodaj punkty końcowe (w tym przypadku usługi App Service) dla których chcesz kontrolować ruch do profil. Stan aplikacji (uruchomiona, zatrzymana lub usunięte) jest regularnie przekazywane do profilu, aby usługi Azure Traffic Manager można kierować ruch odpowiednio.

Podczas przy użyciu usługi Azure Traffic Manager przy użyciu platformy Azure, należy uwzględnić następujące kwestie:

* We wdrożeniach aplikacji tylko w obrębie tego samego regionu usługi aplikacji zawiera już trybu failover i okrężnego funkcjonalność niezależnie tryb aplikacji.
* W przypadku wdrożeń w tym samym regionie, korzystających z usługi aplikacji — w połączeniu z innej usługi w chmurze platformy Azure można połączyć oba typy punktów końcowych na potrzeby scenariuszy hybrydowych.
* Można określić tylko jeden punkt końcowy usługi aplikacji na region w profilu. Po wybraniu aplikacji jako punktu końcowego dla regionu co pozostałe aplikacje w tym regionie stają się niedostępne do wybrania dla tego profilu.
* Punkty końcowe usługi aplikacji, które można określić w profilu Menedżera ruchu Azure jest wyświetlany w obszarze **nazwy domen** sekcja na stronie konfiguracji dla aplikacji w profilu, ale nie ma można konfigurować.
* Po dodaniu aplikacji do profilu, **adres URL witryny** na pulpicie nawigacyjnym portalu stronę instalacji aplikacji wyświetla domenę niestandardową adres URL aplikacji, jeśli zdefiniowano jeden. W przeciwnym razie wyświetla adres URL profilu Menedżera ruchu (na przykład `contoso.trafficmanager.net`). Nazwa domeny bezpośrednio aplikacji i adres URL Menedżera ruchu są widoczne na stronie Konfigurowanie aplikacji w obszarze **nazwy domen** sekcji.
* Niestandardowa nazwa domeny działa zgodnie z oczekiwaniami, ale oprócz dodanie ich do aplikacji, musisz również skonfigurować mapy DNS, aby wskazywał adres URL Menedżera ruchu. Aby uzyskać informacje o tym, jak skonfigurować niestandardową domenę na potrzeby aplikacji usługi app Service, zobacz [mapowania istniejącej nazwy DNS niestandardowej aplikacji sieci Web Azure](app-service-web-tutorial-custom-domain.md).
* Można dodawać tylko te aplikacje, które są w trybie standard lub premium profilu Menedżera ruchu Azure.

## <a name="next-steps"></a>Następne kroki
Koncepcyjna i techniczne omówienie usługi Azure Traffic Manager, zobacz [Omówienie Menedżera ruchu](../traffic-manager/traffic-manager-overview.md).

Aby uzyskać więcej informacji o korzystaniu z Menedżera ruchu z usługi aplikacji, zobacz wpisy blogu [przy użyciu usługi Azure Traffic Manager z witryn sieci Web Azure](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) i [usługi Azure Traffic Manager teraz można zintegrować z witryn sieci Web Azure](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/).

