---
title: "Kontrolowanie aplikacji sieci Web platformy Azure przy użyciu usługi Azure Traffic Manager"
description: "Ten artykuł zawiera podsumowanie informacji dla usługi Azure Traffic Manager, ponieważ odnosi się do aplikacji sieci web platformy Azure."
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
ms.openlocfilehash: 93645aa5765d533b45fe2266f061ad61c0bf45d7
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2017
---
# <a name="controlling-azure-web-app-traffic-with-azure-traffic-manager"></a>Kontrolowanie aplikacji sieci Web platformy Azure przy użyciu usługi Azure Traffic Manager
> [!NOTE]
> Ten artykuł zawiera informacje podsumowania dla Menedżera ruchu Microsoft Azure w odniesieniu do aplikacji sieci Web Azure. Więcej informacji na temat usługi Azure Traffic Manager sam można znaleźć, przechodząc na stronę łącza na końcu tego artykułu.
> 
> 

## <a name="introduction"></a>Wprowadzenie
Usługi Azure Traffic Manager można użyć do kontrolowania sposobu żądania od klientów sieci web są dystrybuowane do aplikacji web apps w usłudze Azure App Service. Po dodaniu profilu Menedżera ruchu Azure punktów końcowych aplikacji sieci web, usługi Azure Traffic Manager przechowuje informacje o stanu aplikacji sieci web (uruchomionej, zatrzymanej lub został usunięty), dzięki czemu można zdecydować, które z tych punktów końcowych powinny odbierać dane.

## <a name="routing-methods"></a>metody routingu
Usługi Azure Traffic Manager używa trzech różnych metod routingu. Te metody są opisane na poniższej liście odnoszą się do aplikacji sieci web platformy Azure.

* **[Priorytet](#priority):** przy użyciu aplikacji sieci web głównej dla całego ruchu, a następnie podaj kopii zapasowych w przypadku podstawowej lub aplikacji sieci web kopii zapasowej jest niedostępny.
* **[Ważone](#weighted):** rozpowszechniają ruchu zestawu aplikacji sieci web równomiernie lub według wagi, które należy zdefiniować.
* **[Wydajność](#performance):** Jeśli masz aplikacje sieci web w różnych lokalizacjach geograficznych, użyj "najbliższy" aplikacja sieci web w postaci najniższe opóźnienia sieci.
* **[Geograficzne](#geographic):** bezpośredniego użytkowników do aplikacji sieci web określonego na podstawie których lokalizacji geograficznej ich zapytanie DNS pochodzi z. 

Aby uzyskać więcej informacji, zobacz [metody routingu ruchu Menedżera](../traffic-manager/traffic-manager-routing-methods.md).

## <a name="web-apps-and-traffic-manager-profiles"></a>Aplikacje sieci Web i profilów usługi Traffic Manager
Aby skonfigurować kontrolę ruchu w sieci web aplikacji, Utwórz profil w usłudze Azure Traffic Manager czy używa jednego z trzech obciążenia równoważenia metod opisanych wcześniej, a następnie dodaj punkty końcowe (w tym przypadku aplikacji sieci web) dla których chcesz kontrolować ruch do profilu. Stan aplikacji sieci web (uruchomiona, zatrzymana lub usunięte) jest regularnie przekazywane do profilu, aby usługi Azure Traffic Manager można kierować ruch odpowiednio.

Podczas przy użyciu usługi Azure Traffic Manager przy użyciu platformy Azure, należy uwzględnić następujące kwestie:

* Wdrożeń sieci web aplikacji tylko w obrębie tego samego regionu aplikacje sieci Web zawiera już trybu failover i funkcja okrężnego niezależnie tryb aplikacji sieci web.
* W przypadku wdrożeń w tym samym regionie, korzystających z aplikacji sieci Web w połączeniu z innej usługi w chmurze platformy Azure można połączyć oba typy punktów końcowych na potrzeby scenariuszy hybrydowych.
* Można określić tylko jeden punkt końcowy aplikacji sieci web dla regionu w profilu. Po wybraniu aplikacji sieci web jako punktu końcowego dla regionu co pozostałe aplikacje sieci web, w tym regionie stają się niedostępne do wybrania dla tego profilu.
* Punkty końcowe aplikacji sieci web, które można określić w profilu Menedżera ruchu Azure jest wyświetlany w obszarze **nazwy domen** sekcja na stronie konfiguracji dla aplikacji sieci web w profilu, ale nie ma można konfigurować.
* Po dodaniu aplikacji sieci web do profilu, **adres URL witryny** na pulpicie nawigacyjnym w sieci Web strony portalu aplikacji wyświetla adresu URL domeny niestandardowej aplikacji sieci web, jeśli zdefiniowano jeden. W przeciwnym razie wyświetla adres URL profilu Menedżera ruchu (na przykład `contoso.trafficmgr.com`). Nazwa domeny bezpośrednie aplikacji sieci web i adres URL Menedżera ruchu są widoczne na stronie Konfigurowanie aplikacji sieci web w obszarze **nazwy domen** sekcji.
* Niestandardowa nazwa domeny działa zgodnie z oczekiwaniami, ale oprócz dodanie ich do aplikacji sieci web, musisz również skonfigurować mapy DNS, aby wskazywał adres URL Menedżera ruchu. Aby uzyskać informacje na temat sposobu konfigurowania domeny niestandardowej dla aplikacji sieci web platformy Azure, zobacz [Konfigurowanie niestandardowej nazwy domeny dla usługi Azure witryny sieci web](app-service-web-tutorial-custom-domain.md).
* Można dodawać tylko aplikacje sieci web, które są w trybie standard lub premium profilu Menedżera ruchu Azure.

## <a name="next-steps"></a>Następne kroki
Koncepcyjna i techniczne omówienie usługi Azure Traffic Manager, zobacz [Omówienie Menedżera ruchu](../traffic-manager/traffic-manager-overview.md).

Aby uzyskać więcej informacji dotyczących korzystania z Menedżera ruchu z aplikacjami sieci Web, zobacz wpisy blogu [przy użyciu usługi Azure Traffic Manager z witryn sieci Web Azure](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) i [usługi Azure Traffic Manager teraz można zintegrować z witryn sieci Web Azure](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/).

